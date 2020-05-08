# Java Script

## Let, Const, Var

Переменная `let`всегда видна именно в том блоке {…}, где объявлена, и не более. Видны только после объявления и только в текущем блоке. Может быть глобальной.

Объявление `const` задаёт константу, то есть переменную, которую нельзя менять

Переменная `var` ограничена областью видимости функциив которой объявлена. В глобальной области видимости var прикрепляется к window. 

```javascript
var a = 1
let b = 2
const c = 3

window.a // 1
window.b // undefined
window.c // undefined
```

## Increment

```javascript
let a = 1;
console.log(++a); // 2
console.log(a); // 2

let a = 1;
console.log(a++); // 1
console.log(a); // 2
```

