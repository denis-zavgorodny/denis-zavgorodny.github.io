---
layout: post
title: "webp - уже пора?"
date: 2015-05-20 10:25:57 +0300
comments: true
categories: [html, google, webp, images]
---
**webp** это "новый" формат изображений для web от google. Новый, в сравнении с общепринятыми форматами jpg, png, gif. По заявлениям самого [google](https://developers.google.com/speed/webp/docs/webp_lossless_alpha_study#results) их формат на 25-34% "легче" по сравнению с jpeg форматом (по [SSIM](https://ru.wikipedia.org/wiki/SSIM)) и на 26% меньше чем png по тем же оценкам. В реальности же легко можно получить **десятикратный** выигрыш в размере файла в случае конвертации больших jpg изображений в webp (например, вот таким [сервисом](http://image.online-convert.com/ru/convert-to-webp) или любым другим доступным вам способом).
<!-- more -->

Чтоб начать использовать webp достаточно просто указать путь к файлу в src тега `img` или использовать css свойство `background-image`. **Все!** Вы сэкономили драгоценные секунды во времени загрузки сайта и трафик пользователей. 

Но, есть и обратная сторона, к сожалению, пока поддержка этого формата [оставляет желать лучшего](http://caniuse.com/#feat=webp). Мы не имеем даже Chrome+Opera+Firefox+Safari связки, зато имеем поддержку Android (начиная с версии 4.0, что дает возможность использовать изображения webp в мобильных приложениях, где трафик очень важен).

В любом случае, всегда можно сделать приятно вашим пользователям с наличием Google Chrome и Opera, а остальные пользователи получат обычный jpg или png.

**С чего начать**
Начать нужно с конвертации изображений в webp формат. Можно подготовить изображения [заранее](https://www.npmjs.com/package/grunt-webp-compress) ([еще](https://github.com/somerandomdude/grunt-webp)), конвертировать [online](https://www.google.com.ua/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=webp%20online) или готовить webp на сервере.

Вариант с конвертацией на сервере видится наиболее интересным, так как готовить нужные webp вы можете сразу после загрузки изображений (jpg, png) на сайт (или в момент первого обращения к картинке по http).

Решений тут не так много. Например, google предоставляет консольную [утилитку](https://developers.google.com/speed/webp/docs/cwebp) для конвертации, но, вы должны скомпилировать ее из исходников, а такую возможность имеют далеко не все.
К счастью, ImageMagic также поддерживает работу с изображениями в формате webp и вы можете преобразовывать изображения при помощи,  например, php и [класса](http://php.net/manual/ru/book.imagick.php) `Imagick`.

```
$im = new Imagick();
$im->readImage($sourceFile);
$im->stripImage();
$im->setImageFormat("webp");
$im->setResolution(72, 72);
$im->setImageAlphaChannel(imagick::ALPHACHANNEL_ACTIVATE);
$im->setBackgroundColor(new ImagickPixel('transparent'));
$im->setCompression(true);
$im->setImageCompressionQuality(90);
$im->writeImage($destinationFile);
$im->destroy();
```

`setImageCompressionQuality` установите на свой вкус.

В конце концов, можно просто включить флаг `convert_jpeg_to_webp` в вашем NGINX c [PageSpeed](https://developers.google.com/speed/pagespeed/module/filter-image-optimize) (кстати, PageSpeed для Apache тоже поддерживает такую возможность).

Так почему же мы до сих пор не используем webp? Я вот задумался.