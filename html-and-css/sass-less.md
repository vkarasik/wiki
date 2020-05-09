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

```css
// SCSS LESS

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

h1 {
    color: $primary-button;
    .bordered(red)
}
```

## Extend

Наследование свойств из дугого класса.

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

