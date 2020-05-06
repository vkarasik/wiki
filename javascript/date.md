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

## Objects

```javascript
const person = {
    name: 'Joe',
    action: function(){},
    action() {} // ES6 syntax
}
```

### this

Значение this определяется тем, как была вызвана функция. Если функция вызвана как метод объекта, то this указывает на этот объект. Если функция вызвана за пределами объекта, то this указывает на window, в стргогом режиме undefined

```javascript
const person = {
    name: 'Joe',
    action() {} // ES6 syntax
}

person.action() // this person

let action = person.action // присвоим функцию из объекта
action() // this undefined

// Для явного указания используются .bind(), .call(), .apply()

let action = person.action.bind(person)
action() // this person
```

### Object Destructuring <a id="lecture_heading"></a>

Деструктурирующее присваивание – это специальный синтаксис, который позволяет «распаковать» массивы или объекты в переменные.

```javascript
const address = {
    city: 'Minsk',
    street: 'Main'
}

let { city, street } = address

city // "Minsk"
street // "Main"

let { city: a, street: b } = address

a // "Minsk"
b// "Main"
```

## Arrow Functions

```javascript
const person = {
    action() {
        setTimeout(function(){
            console.log(this)
        }, 1000)
    }
}

person.action() // undefined or window потому-то колбэк функция не является частью объекта


// Стрелочная функция всегда привязана к объекту в контексте которого вызывалась
const person = {
    action() {
        setTimeout(() => {
            console.log(this)
        }, 1000)
    }
}

person.action() // this person

```

