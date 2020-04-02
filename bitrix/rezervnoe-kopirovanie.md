# Резервное копирование

Чем мешьше размер архивов, тем быстрее разворачивается на OpenServer

### Проблемы при переносе

Часто при переносе Битрикса возникает следующая ошибка:

Внимание! Сайт работал в кодировке UTF-8. Конфигурация сервера не соответствует требованиям, установите mbstring.func\_overload=2 и mbstring.internal\_encoding=UTF-8.

Это означает что сервер не сконфигурирован для работы Битрикса в кодировке UTF-8.

Эту ошибку можно обойти следующим образом. Если архив уже распаковался, в файле /bitrix/php\_interface/dbconn.php закомментировать сроку:

```php
define("BX_UTF", true);
```

После завершения переноса раскомментировать.
