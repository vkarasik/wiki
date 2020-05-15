# SASS LESS

## Переменные

```css
// SCSS
$primary-button: blue;

button {
    background: $primary-button;
}

// LESS
@primary-button: blue;

button {
    background: @primary-button;
}
```

{% hint style="info" %}
Также см. maps
{% endhint %}

## Nesting

```css
// SCSS
body {
  button {
    color: red;
  }
}

// LESS
body {
  button {
    color: red;
  }
}
```

## Parent Selector

Символ & возвращает родительский селектор или всю цепочку. Таким образом можно конкатенировать селекторы через вложение, или вложенный селектор может стать родительским.

```css
// SCSS LESS
.header {
  .menu {
    border-radius: 5px;
    .no-borderradius & {
      background-image: url('images/button-background.png');
    }
  }
}

.header .menu {
  border-radius: 5px;
}
.no-borderradius .header .menu {
  background-image: url('images/button-background.png');
}
```

## Импорт

При использовании импорта CSS есть недостаток в виде запросов к импортируемым файлам. В препроцессорах импорт происходит на этапе компиляции.

В SCSS файлы с именами типа \_style.scss не будут скомпилированы в css. Такой нейминг используют для импорта.

```css
// SCSS 
@import './header'; // Нежелательно
@use './header';


//LESS

@import './header';
```

## Миксины

С помощью миксинов можно «подмешивать» одни правила в другие. В миксины можно передавать агрументы.

```css
// SCSS
@mixin bordered($color) {
  border: 1px solid $color;
}

header {
  button {
    color: $primary-button;
    @include bordered(red);
  }
}

// LESS

.bordered(@color) {
    border: 1px solid @color;
}

.colored{
  color: red;
}

h1 {
    .colored();
    .bordered(red);
}
```

{% hint style="info" %}
В LESS миксин объявленный со скобками, не будет выведен в конечный CSS. Это аналогично placeholder в SASS. См. Extend
{% endhint %}

## Extend

Наследование свойств из дугого класса. В SASS объявления типа %classname не попадают в конечный CSS, используются для шаринга общих свойств, и называются placeholder.

```css
// SCSS
.radius {
  border-radius: 5px;
}

header {
  button {
    @extend .radius;
  }
}

%bordered{ // Это объявление не попадет в CSS
  border: 1px solid red;
}

header {
  button {
    @extend %bordered;
  }
}

// LESS

.radius {
  border-radius: 5px;
}

header {
  button {
    &:extend(.radius);
  }
}
```

## Maps

Переменные подобные объектам — key: value.

```css
// SCSS
$colors: (
  'primary': red,
  'another': blue,
);

button {
  color: map-get($colors, primary);
}

// LESS
@colors: {
    primary: red;
    another: blue
}

h1 {
    color: @colors[another];
}
```

## Functions

```css
// SCSS
$colors: (
  'primary': red,
  'another': blue,
);

@function color($color-name) {
  @return map-get($colors, $color-name);
}

button {
  color: color(another);
}

```

