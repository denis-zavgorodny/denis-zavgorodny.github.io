---
layout: post
title: "Формирование мета-тегов страницы сайта на Bitrix для людей и роботов. Часть 1."
date: 2015-03-22 22:31:24 +0200
comments: true
categories: [PHP, Bitrix, Русский язык, phpmorphy]
---

У "1С-Битрикс Управление сайтом" есть чудесный механизм формирования meta-тегов с автоподстановками и барышнями. Да только вот текст, формируемый этими подстановками, выходит не очень читаемым. Но, возможность подставить в meta-данные страницы разные параметры, взятые, например, у товара (название, цвет, цена) сама по себе шикарна. 
Клево иметь возможность задать Title странице: "Купить {this.NAME} {this.COLOR} за {this.PRICE}" и получить красивые заголовки. А еще круче иметь возможность получить заголовки, которые не разрывают мозг своим набором несогласованных между собой слов - "Купить Джинсы Красный 10 000 руб".

А что нужно сделать первым делом? Пойти в Маркет плейс Битрикса. Попытка окончилась неудачей, потому начались поиски решения.
<!-- more -->

Рассматривался вариант с сервисом Yandex, но он был отброшен как низкопроизводительный. Гонять запросы на каждый  товар и тег или группу товаров было не совсем разумно. И тогда вспомнился [phpmorphy](http://phpmorphy.sourceforge.net), который кто-то из коллег использовал в проектах. Нужно сказать, что документация по phpmorphy не радует, а еще не радовали мои познания в русском языке. Пришлось вспоминать школьную программу.

В итоге, задача свелась к следующему:

- добавить к стандартным средствам СЕО-тегов Битрикс возможность обработки текста через phpmorphy;
- разобраться с phpmorphy;
- в идеале, оформить это все в виде модуля для Битрикс и положит в Маркетплейс;

До модуля еще не добрались, потому речь пойдет о том, **как добавить кастомные обработчики при генерации метатегов**.
```
Каталог {=this.Name} в {this.parent.NAME}
```
Так можно делать "из коробки", а что если нужно выполнить свою функцию, например, приведения текста к верхнему регистру, например, вот так:
```
Каталог {=this.Name} в {=stringclass "upstring" {this.parent.NAME}}
```

У **iblock** есть событие [*OnTemplateGetFunctionClass*](http://dev.1c-bitrix.ru/community/webdev/user/87386/blog/9317/) описанное еще и [тут](http://pilezkiy.com/blog/)

Начинаем слушать событие:
```
use Bitrix\Main;
$eventManager = Main\EventManager::getInstance();
$eventManager->addEventHandler(
	"iblock", 
	"OnTemplateGetFunctionClass", 
	"OnTemplateGetFunctionClassHandler"
);

function OnTemplateGetFunctionClassHandler(Bitrix\Main\Event $event) {
	$arParam = $event->getParameters();
	$functionClass = $arParam[0];
	// Проверяем существует ли класс
	if (is_string($functionClass) && class_exists($functionClass)) {
		$result = new Bitrix\Main\EventResult(1,$functionClass);
		return $result;
	}
}
```

В $arParam попадает массив от строки *{=stringclass "upstring" {this.parent.NAME}}*, разобранной по пробелу строки:
```
$arParam[0] -> stringclass 
$arParam[1] -> upstring 
$arParam[2] -> результат компиляции {this.parent.NAME} 
```
и пытается исполниться метод **calculate** класса **stringclass** наследованного от **Bitrix\Iblock\Template\Functions\FunctionBase**.

Сам **FunctionBase** лежит тут */bitrix/modules/iblock/lib/template/functions/fabric.php*

Код класса быдет выглядть примерно вот так:
```
class stringclass extends Bitrix\Iblock\Template\Functions\FunctionBase {
	// Функция обработки строки
	public function calculate(array $parameters) {
		$string = "";
		//Тут что-то делаем со строкой
		...

		return $string;
	}
	//
	public function onPrepareParameters(\Bitrix\Iblock\Template\Entity\Base $entity, $parameters) {
		$arguments = array();
		/** @var \Bitrix\Iblock\Template\NodeBase $parameter */
		foreach ($parameters as $parameter) {
		$arguments[] = $parameter->process($entity);
		}
		return $arguments;
	}
}
```
Метод **calculate** получает на вход массив *$parameters*, заблаговременно подготовленный методом **onPrepareParameters**. Результатом работы метода **calculate** должна быть строка.

В продолжении речь пойдет о том, как используя всего в пару методов phpmorphy генерировать довольно человекочитаемые строки.
