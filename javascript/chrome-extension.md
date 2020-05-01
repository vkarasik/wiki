# Chrome Extensions

{% embed url="https://developer.chrome.com/extensions" %}

## Extension

Расширение состоит из обычных html, js, css и json файлов. Каждое расширение должно иметь файл `manifest.json` который описывает расширение и хранит все основные настройки.

{% tabs %}
{% tab title="manifest.json" %}
```javascript
{
    "name": "Budget Manager",
    "version": "1.0",
    "description": "Extension to Track Spendings",
    "manifest_version": 2,
    "icons": {
        "16": "icon16.png",
        "48": "icon48.png",
        "128": "icon128.png"
    },
    "background": {
        "scripts": [
            "background.js"
        ],
        "persistent": false
    },
    "browser_action": {
        "default_icon": "icon48.png",
        "default_popup": "popup.html"
    },
    "permissions": [
        "storage",
        "notifications",
        "contextMenus"
    ],
    "options_page": "options.html"
}
```
{% endtab %}
{% endtabs %}

Расширение может состоять из нескольких связаных между собой компонентов:

* background scripts — скрипты выполняющиеся в фоне и по событиям
* content scripts — скрипты волняющиеся на определенных страницах
* options page — скрипты для настройки расширения
* UI page — интрефейс расширения

### permissions

Для использования различных API chrome мы должны указать что собираемся использовать в поле `permissions`

```javascript
"permissions": [
    "storage",
    "notifications",
    "contextMenus"
]
```

### browser\_action

Чтобы показать иконку на панели, которая может иметь всплывающее окно, подсказку, бэйдж используется поле browser\_action.

{% tabs %}
{% tab title="manifest.json" %}
```javascript
"browser_action": {
    "default_icon": "icon48.png",
    "default_popup": "popup.html"
}
```
{% endtab %}
{% endtabs %}

`default_popup` это html файл, к которому можно подключить скрипты и стили. Он отобразится при нажатии на иконку расширения.

#### badge

`chrome.browserAction.setBadgeText` позволяет вывести текст из 4-х символов над иконкой расширения. Цвет фона может быть изменен.

```javascript
chrome.browserAction.setBadgeText({ text: 'Text' });
```

### page action extension

Расширение которое работает на определнных страницах. Такое расширение по умолчанию неактивно. Его функциональность нужно описывать в background scripts.

```javascript
"page_action": {
    "default_icon": "icon48.png",
    "default_popup": "popup.html",
    "default_title": "Change Font"
},
"backround": {
    "scripts": [
        "eventPage.js"
    ],
    "persistent": false
}
```

Чтобы активировать расширение нужно вызвать метод `show(tabID)`

```javascript
chrome.tabs.query({ active: true, currentWindow: true }, function (tabs) {
  let tabID = tabs[0].id;
  chrome.pageAction.show(tabID);
});
```

### Content Scripts

Файлы которые загружаются в контексте страницы и могут работать с ее DOM но не с JS. Такой код исполняется в так называемом «изолированом мире».

Такие скрипты не могут использовать большую часть API, поэтому основной функционал реализуется за счет обмена сообщениями между background и popup скриптами.

```javascript
"content_scripts": [
    {
        "matches": [
            "https://developer.chrome.com/*"
        ],
        "css": [
            "myStyles.css"
        ],
        "js": [
            "contentScript.js"
        ]
    }
],

"permissions": [
    "https://developer.chrome.com/*"
]
```

### tabs

Позволяет получить массив объектов с инфо об открытых вкладках. Для получения активной вкладки можно использовать:

```javascript
chrome.tabs.query({ active: true, currentWindow: true }, function (tabs) {
  alert(JSON.stringify(tabs));
});
```



### background page

Страница, а точнее скрипт который выполняется в фоне и реагирует на события, например когда контент скрипт присылает сообщение. По другому может называться Event Page.

{% tabs %}
{% tab title="manifest.json" %}
```javascript
"background": {
    "scripts": [
        "background.js"
    ],
    "persistent": false // в большинстве случаев false, 
    // и будет выполняться не постоянно, а по событию.
}
```
{% endtab %}
{% endtabs %}

### contextMenus

Позволяет добавляет и вызывать расширение через контекстное меню. Доступны разные типы и контексты в которых оно будет доступно, например: выделение, клик на картинке и тд. Этот скрипт должен находится в background scripts.

```javascript
const contextMenuItem = {
  type: 'normal',
  id: 'extID',
  title: 'Ext Title',
  contexts: ['selection'],
};

chrome.contextMenus.create(contextMenuItem);

chrome.contextMenus.onClicked.addListener(function(event){
  // event объект с информацией о клике
  event.selectionText // выделенный текст
})
```

### chrome.storage

Часть API chrome которое позволяет использовать хранилище. Не путать с localStorage. Может хранить объекты. Данные можно хранить и локально, и синхронизировать между устройствами. Хранилище ограничено рамками текущего расширения.

#### onChanged

Событие изменения в storage.

```javascript
chrome.storage.onChanged.addListener(function (changes, storageName) {
  changes.key.newValue // новое значение ключа
  changes.key.oldValue // старое значение ключа
});
```

### options page

Для настройки расширения можно использовать отдельную страницу, которая доступна через контекстное меню расширения, либо через страницу расширений.

{% tabs %}
{% tab title="manifest.json" %}
```javascript
"options_page": "options.html"
```
{% endtab %}
{% endtabs %}

### notifications

Позволяет выводить уведомления. В зависимости от типа может содержать разные элементы: иконки, кнопки, прогресс, картинки, текст.

```javascript
const notificationOptions = {
  type: 'basic',
  iconUrl: 'icon128.png',
  title: 'Limit reached!',
  message: "Sorry! You've reached limit!",
};

chrome.notifications.create('notificationID', notificationOptions);
```

