---
title: magic-inline
date: 2017-04-23 16:33:53
tags: [web, css, baseline, align]
---

Inline-block элементы - очень мощное средство разметки. Наверняка сейчас вы говорите: да ладно, есть же Grid Layout, есть flexbox и в 2017 году никого inline-block -ами не удивить. Да, вы правы. Но, давайте представим...

Помните как вы раскладывали сетку с блоками при помощи float: left? 

<!-- more -->

<p data-height="265" data-theme-id="0" data-slug-hash="ybJmpb" data-default-tab="result" data-user="denisz" data-embed-version="2" data-pen-title="ybJmpb" class="codepen">See the Pen <a href="https://codepen.io/denisz/pen/ybJmpb/">ybJmpb</a> by Denis Zavgorodny (<a href="http://codepen.io/denisz">@denisz</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

А помните, что бывало, если один из блоков становился высотой большей чем остальные?

<p data-height="265" data-theme-id="0" data-slug-hash="QvEeQK" data-default-tab="result" data-user="denisz" data-embed-version="2" data-pen-title="QvEeQK" class="codepen">See the Pen <a href="https://codepen.io/denisz/pen/QvEeQK/">QvEeQK</a> by Denis Zavgorodny (<a href="http://codepen.io/denisz">@denisz</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Сейчас это проблемы третьего мира, а когда-то такие вещи изрядно досаждали. И вот тогда на помощь приходили inline-block элементы:

<p data-height="265" data-theme-id="0" data-slug-hash="PmzMew" data-default-tab="result" data-user="denisz" data-embed-version="2" data-pen-title="PmzMew" class="codepen">See the Pen <a href="https://codepen.io/denisz/pen/PmzMew/">PmzMew</a> by Denis Zavgorodny (<a href="http://codepen.io/denisz">@denisz</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Мы задешево получали гибкую сетку, раскладывали блоки плиткой и радовались. Но это еще не все. Так как inline-block снаружи ведет себя как inline элемент, то группы таких элементов выстраиваются в строку. Да-да, точно так же (ну или почти точно так же), как буквы и слова выстраиваются в строки. А раз так, то мы получаем еще один мощный инструмент - вертикальное выравнивание блоков в контейнере строки.

Наверняка, вы знаете о том, что есть CSS свойство `vertical-align` и оно влияет на то, какое вертикальное выравнивание получит элемент (или дочерние элементы, в случае применения этого свойства к ячейке таблицы).

И вот тут начинается самое интересное. Вся механика вертикального выравнивания базируется на понятиях высоты контейнера строки (line box) и базовой линии (base line).

Начнем с контейнера строки. Спецификация сообщает нам, что контейнер строки заполняется горизонтально, ширина контейнера строки определяется содержимым, а если содержимого слишком много, то часть его переносится в следующий контейнер строки. При этом, контейнеры строки располагатся горизонтально один под другим. Тут все просто. В спецификации также сказано, что высота контейнера строки ограничивается самой верхней точкой содержащегося в ней родительского элемента (это может быть буква, замещаемый элемент или другой inline-block элемент) сверху и самой нижней снизу, таким образом, что контейнер строки "достаточно высок", чтоб охватить все содержащиеся в нем элементы с учетом свойства `vertical-align`.

Простой пример с вертикальным выравниванием блоков представлен ниже:

<p data-height="265" data-theme-id="0" data-slug-hash="gWMXdZ" data-default-tab="result" data-user="denisz" data-embed-version="2" data-pen-title="gWMXdZ" class="codepen">See the Pen <a href="https://codepen.io/denisz/pen/gWMXdZ/">gWMXdZ</a> by Denis Zavgorodny (<a href="http://codepen.io/denisz">@denisz</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Высота контейнера строки в данном примере будет равняться высоте самого высокого блока c4, так как контейнер строки должен охватывать собержащиеся в нем блоки. Синие блоки образуют новый контейнер строки, потому что они не могут расположиться в одну строку с красными. Высота контейнера строки с синими блоками определяется аналогично.



**Ссылки для работы**


https://www.w3.org/TR/CSS2/visudet.html#propdef-vertical-align

https://www.w3.org/TR/CSS22/visudet.html#line-height

https://www.w3.org/TR/CSS22/visudet.html#inline-box-height

https://www.google.com.ua/webhp?sourceid=chrome-instant&rlz=1C5CHFA_enUA713UA715&ion=1&espv=2&ie=UTF-8#q=%D1%81%D0%BC%D0%B5%D1%89%D0%B5%D0%BD%D0%B8%D0%B5+baseline+%D0%B4%D0%BB%D1%8F+inline-block

https://web-standards.ru/articles/vertical-align/

https://htmlacademy.ru/blog/94-the-vertical-align-property

https://www.smashingmagazine.com/2012/12/css-baseline-the-good-the-bad-and-the-ugly/

http://stackoverflow.com/questions/25836191/vertical-alignment-based-on-x-height/26074244

https://www.w3.org/TR/REC-CSS2/visudet.html#line-height

https://ru.stackoverflow.com/questions/619536/%D0%9A%D0%B0%D0%BA-%D0%B2%D1%8B%D1%81%D1%82%D1%80%D0%B0%D0%B8%D0%B2%D0%B0%D1%8E%D1%82%D1%81%D1%8F-%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%BE%D0%B2%D1%8B%D0%B5-%D0%B1%D0%BB%D0%BE%D0%BA%D0%B8-%D0%B2%D0%BD%D1%83%D1%82%D1%80%D0%B8-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D0%B0-%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B8