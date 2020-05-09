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

