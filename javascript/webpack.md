---
description: '#webpak #node #npm'
---

# Webpack

## Пакеты

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

### normalize.css

```text
npm install normalize.css
```

Теперь в css файле можно указать путь импорта

```javascript
@import '~normalize.css'; // тильда это импорт из node_modules
```

## package.json

Для настройки команд используется свойство `scripts`

```javascript
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --mode development",
    "build": "webpack --mode production",
    "watch": "webpack --mode development --watch" // следить за изменениями
  }
```

Для того чтобы наш пакет был приватным есть специальное свойство. Также можно убрать строчку `main`

```javascript
// "main": "app.js",
  "private": true,
```

## webpack.config.js

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
    new CleanWebpackPlugin()
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
      },
      {
          test: /\.(png|jpe?g|gif)$/i,
          use: [{
              loader: 'file-loader',
              options: {
                  // name: '[name].[ext]', // настройки нейминга. По умолчанию название генерируется как хэш
                  outputPath: 'img' // выходная папка
              }
          }],
      }]
  }
};
```

### resolve

Позволяет не указывать расширение файла при указании пути.

```javascript
resolve: {
    extensions: ['.js', '.json', '.png', '.css']
}
```

```javascript
import Post from './module.js';
import './css/styles.css';
import './img/logo.png';

// Теперь можем писать так:

import Post from './module';
import './css/styles';
import './img/logo';
```

Позволяет указать alias для путей

```javascript
import '../../../css/styles.css';
import '../img/logo.png';

// Теперь можем писать так:

import '@css/styles';
import '@img/logo';
```

```javascript
resolve: {
    alias: {
        '@img': path.resolve(__dirname, 'src/img'),
        '@css': path.resolve(__dirname, 'src/css'),
        '@': path.resolve(__dirname, 'src'),
    }
}
```

## Плагины

### Работа с файлами .html

```text
npm install --save-dev html-webpack-plugin
```

Для настройки добавляем в `webpack.config.js`

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
  
  // . . .
  
plugins: [
  new HtmlWebpackPlugin({
    title: 'My Webpack', // игнорируется еслм указан template
    template: './index.html' // файл шаблон, если не задан, то будет создаваться чистый html5 документ
  })
]
```

### Очистка dist/

```text
npm install --save-dev clean-webpack-plugin
```

```javascript
const {CleanWebpackPlugin} = require('clean-webpack-plugin');

// . . .

plugins: [
  new CleanWebpackPlugin()
]
```

## Loaders

### css-loader

Добавляют возможность работать с `import` файлами других типов: css, less и тд.

```text
npm install --save-dev css-loader
```

Тперь в index.js можно писать так:

```javascript
import './css/styles.css';
```

### style-loader

Вставляет стили на старницу. Может делать это разными способами в зависимости от настроек. По умолчанию стили будут динамически вставлены через javascript inject при загрузке страницы в `<style>` 

```javascript
npm install --save-dev style-loader
```

```javascript
module: {
    rules: [{
        test: /\.css$/, // регулярное выражение
        use: [{
                loader: 'style-loader',
                options: {
                    injectType: 'styleTag'
                }
            },
            'css-loader'
        ] // порядок важен: справа налево. Первым файл пропустится через css-loader
    }]
}
```

### file-loader

Добавляет возможнось импортировать и собирать различные файлы.

```text
$ npm install file-loader --save-dev
```

```javascript
module: {
    rules: [{
        test: /\.(png|jpe?g|gif)$/i,
        use: [{
            loader: 'file-loader',
            options: {
                // name: '[name].[ext]', // настройки нейминга. По умолчанию название генерируется как хэш
                outputPath: 'img' // выходная папка
            }
        }],
    }]
}
```

### xml-loader

Работа с xml

```bash
npm install --save xml-loader
```

```bash
module: {
    rules: [{
        test: /\.xml$/i,
        use: ['xml-loader'],
    }]
}
```

```javascript
import xml from './xml/export.xml';
```

### csv-loader

```bash
npm install -save-dev csv-loader
npm install -save-dev papaparse
```

```bash
module: {
    rules: [{
        test: /\.csv$/i,
        use: ['csv-loader'],
    }]
}
```

```javascript
import csv from './csv/export.csv';
```

