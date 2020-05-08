# Objects

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

