---
description: '#bitrix'
---

# Обмен с 1С

Чтобы протестировать загрузку товаров, нужно кинуть файлы обмена в 

`/upload/1cexchange/`

_Далее под админом открываем_

_`/bitrix/admin/1c_exchange.php?type=catalog&mode=import&filename=ИМЯ`_`ФАЙЛА{import.xml, offers.xml и т.д.}`

И дальше обновляем страницу, анализируя шаги обмена.

Для тестирования выгрузки заказов с сайта открываем 

`/bitrix/admin/1c_exchange.php?type=sale&mode=query`

Этот же фид видит 1С при запросе заказов.

