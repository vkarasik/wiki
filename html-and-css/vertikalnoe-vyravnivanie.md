---
description: '#html #css'
---

# Вертикальное выравнивание

Свойство vertical-align применяется для ячеек таблицы, где отлично действует. Мы можем вывести наш элемент как ячейку таблицы и использовать для него свойство vertical-align для вертикального центрирования содержания.

```markup
<style>
#parent {display: table;} 
#child {
	display: table-cell;
	vertical-align: middle;
}
<style>

<div id="parent">
    <div id="child">Содержание</div>
</div>
```

