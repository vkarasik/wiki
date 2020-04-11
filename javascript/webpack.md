---
description: '#webpak #node #npm'
---

# Webpack

```bash
npm init
```

```bash
npm i -D webpack webpack-cli
```

`-P`, `--save-prod` , `--save`: сохранит зависимости в "dependencies": { }. Это код для продакшена, он будет включен в конечный продукт. Добавляй сюда только те библиотеки, которые будут использованы при работе конечного продукта \(вэб страницы например\).

`-D`, `--save-dev`: сохранит пакет в "devDependencies": { }. Это пакеты, которые используют в процессе девелопмента, препроцессоры LESS, SASS, валидаторы кода, JShint Slint, препроцессоры JS: Babel. Эти пакеты не будут включены в конечный продукт.

```javascript
 "devDependencies": {
    "webpack": "^4.42.1", // основной функционал
    "webpack-cli": "^3.3.11" // командная строка для webpack
  }
```

### package.json

Для настройки команд используется свойство `scripts`

```javascript
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --mode development",
    "build": "webpack --mode production"
  }
```

### webpack.config.js

В зависимости от указаных опций в этом файле, webpack будет собирать проект.

```javascript
const path = require('path');
const {CleanWebpackPlugin} = require('clean-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  context: path.resolve(__dirname, 'src'), // путь к стартовому файлу. Теперь можно запускать сборку из любого места
  mode: 'production', // режим сборки для продакшена (все минифицировано)
  //mode: 'development', // режим сборки для разработки
  
  //entry: './index.js', // входная точка, отсюда начнется сборка
  entry: { // входных точек может быть несколко
    main: './index.js',
    anotherscript: './another.js'
  },
  
  output: {
    path: path.resolve(__dirname, 'dist'),
    //filename: 'bundle.js' // выходной файл
    //filename: '[name].bundle.js', // prefix из enrty
    filename: '[name].[contenthash].js' // prefix из enrty, имя как hash — для решения проблемы с кешированием скриптов

  },
  
  plugins: [
    new HtmlWebpackPlugin({
      title: 'My Webpack', // игнорируется еслм указан template
      template: './index.html' // файл шаблон, если не задан, то будет создаваться чистый html5 документ
    }),
    new CleanWebpackPlugin() // 
  ],
  
  module: {
      rules: [{
          test: /\.css$/, // регулярное выражение
          use: [{
                  loader: 'style-loader',
                  options: {
                      injectType: 'linkTag' // вставка стилей через link
                  }
              },
              'css-loader'
          ] // порядок важен: справа налево. Первым файл пропустится через css-loader
      }]
  }
};
```

### Плагины

#### Работа с файлами .html

```text
npm install --save-dev html-webpack-plugin
```

#### Очистка dist/

```text
npm install --save-dev clean-webpack-plugin
```

## Loaders

Добавляют возможность работать с `import` файлами других типов: css, less и тд.

#### css-loader

```text
npm install --save-dev css-loader
```

```javascript
import './css/styles.css';
```

#### style-loader

Вставляет стили на старницу. Может делать это разными способами в зависимости от настроек. По умолчанию стили будут вставлены через javascript inject при загрузке страницы в `<style>` 

```javascript
npm install --save-dev style-loader
```





