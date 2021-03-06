---
description: >-
  Инструмент сборки фронтенда. Он позволяет автоматизировать повторяющиеся
  задачи (сборка и минификация CSS и JS файлов, запуск тестов, перезагрузка
  браузера и другие).
---

# Gulp

### Установка

```bash
# Устанавливаем глобально Gulp
npm install --global gulp

# Создаем package.json в проекте
npm inin

# Установить Gulp как devDependencies
npm install gulp --save-dev
```

Ключ `--save-dev` говорит о том, что информация о плагине \(имя в репозитории и его версия\) будет добавлена в конфиг package.json и запомнит его для данного проекта. Поскольку мы не храним в гите тяжеловесную папку с плагинами node\_modules, сохраненная в конфиге информация об установленных плагинах позволит всего одной командой npm install развернуть в проекте все нужные плагины.

### gulpfile.js

```javascript
// собираем все наши плагины
var gulp  = require('gulp'),
    gutil = require('gulp-util');

// создаем задачу, которая будет выполняться по умолчанию
gulp.task('default', function() {
  return gutil.log('Gulp is running!')
});

```

### API

#### gulp.task

`gulp.task` определяет наши задачи. В качестве аргументов принимает название, зависимости \(массив\) и функцию \(основные действия\). Зависимостей может и не быть:

```javascript
gulp.task('mytask', function() {
  //сделать что-то
});

gulp.task('dependenttask', ['mytask'], function() {
  //сделать что-то после того, как 'mytask' будет выполнен
});
```

В простом случае функции `gulp.task` передается только два параметра: название таска и функция. Таск также может включать в себя выполнение других тасков. К примеру, мы можем определить таск build, который будет запускать выполнение двух других тасков: css и js:

```javascript
gulp.task('build', ['css', 'js']);
```

Таски css и js будут выполняться асинхронно, поэтому нельзя гарантировать, что один из тасков выполнить раньше другого. Если требуется проверить, что таск завершился, перед запуском следующего, можно определить зависимость следующим образом:

```javascript
gulp.task('css', ['js'], function() {
    ...
});
```

Теперь Gulp будет запускать таск js и ждать его завершения перед запуском таска css. Также Gulp позволяет определить таск по умолчанию, который будет запускаться по команде gulp:

```javascript
gulp.task('default', function() {
    ...
});
```

#### gulp.src

`gulp.src` указывает на файлы, которые мы хотим использовать. Он использует .pipe для доступа к файлам через плагины.  
`gulp.dest` указывает на папку, в которую мы хотим сохранить измененные файлы.  
gulp.src и gulp.dest используется для простой копии файлов:

```javascript
gulp.task('copyHtml', function() {
  // скопировать все html файлы из source/ в public/
  gulp.src('source/*.html').pipe(gulp.dest('public'));
});
```

В gulp встроена система реагирования на изменения файлов gulp.watch. Вы можете использовать эту задачу для запуска других необходимых вам задач при изменении файлов. будет выполнять задачу copyHtml когда любой html файл в папке source изменится:

```javascript
gulp.watch('source/**/*.html', ['copyHtml']);
```

### Шаблоны выборки

Функция gulp.src\(\) в качестве параметра принимает маску файлов и возвращает поток, представляющий эти файлы, который уже может быть передан на вход плагинам. Примерами таких масок могут быть:  


```text
js/app.js — файл app.js в директории js
js/*.js — все файлы с расширением .js в директории js
js/**/*.js — все файлы с расширением .js в директории js и всех дочерних директориях
!js/app.js — исключает определенный файл
*.+(js|css) — все файлы в корневой директории с расширениями .js или .css
```

Например, чтобы задать маску для всех файлов .js в директории js, кроме уже минифицированных, можно использовать маску:

```javascript
gulp.src(['js/**/*.js', '!js/**/*.min.js'])
```

### Структура каталогов проекта

```javascript
|—project/
	|—app/
	|—css/
	|—js/
	|—less/
	|—fonts/
	|—img/
	index.html
|—dist/
|—gulpfile.js
|—package.json
|—mode_modules/

```

### Потоки

Потоки позволяют пройти некоторым данным через ряд, как правило, небольших функций, которые модифицируют данные, а затем передают их следующей функции.

Функция gulp.src\(\) получает строку, которая соответствует файлу или набору файлов, и создаёт поток объектов представляющих эти файлы. Затем они переходят \(или перетекают\) в функцию uglify\(\), которая принимает объекты файлов и возвращает новые объекты файлов с минимизированным исходником. Этот результат затем перетекает в функцию gulp.dest\(\), которая сохраняет изменённые файлы и т.п.

