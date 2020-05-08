# Functions

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

