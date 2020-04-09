---
description: '#htaccess #apache'
---

# htaccess

Иногда нужно сделать так, чтобы в случае отсутствия в каталоге файла, который показывается по умолчанию, листинг, то есть список файлов в каталоге, не выдавался \(не показывать каталог сайта — его содержимое в случае отсутвия в нем индексного файла - index.html, index.php... \). В этом случае добавим в .htaccess такую строчку:

**`Options -Indexes`**

Источник:[**`http://htaccess.net.ru/doc/htaccess/Options.php`**](http://htaccess.net.ru/doc/htaccess/Options.php)

Или создать index.html нулевого размера. Тогда Apache будет показывать его.

