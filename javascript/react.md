# React

## Установка

Для того чтобы установить все необходимое окружение, запустим:

```text
npx create-react-app react-tutorial
```

Затем перейдем в папку и запустим сервер:

```text
cd react-tutorial
npm start
```

## JSX

JSX — Java Script XML. Позволяет создавать элементы как html шаблоны, и использовать внутри них любой валидный js используя скобки. JSX можно использовать как обычные объекты. Их можно возвращать из функций, их можно передавать как аргументы.

```jsx
const heading = <h1 className="site-heading">Hello, React. Today is: {JSON.stringify(new Date)}</h1>

// То же самое можно делать так:

const heading = React.createElement('h1', { className: 'site-heading' }, 'Hello, React!')

// Вставляем в контейнер

ReactDOM.render(element, document.getElementById('root'));
```

Чтобы использовать автодополнения, компонент можно создавать с раширением .jsx, но по большому счету разницы нет, можно и .js

### Атрибуты

Атрибуты также устанавливаются с помощью фигруных скобок. Класс приваивается артибутом `className`. Обработчики используют camelCase, например `onClick`

При установке обработчика событий, метод присваеваем без скобок. 

```jsx
doSomething(){
    console.log('Action')
}

<h1 className={name} onClick={doSomething}>Hello!</h1>
```

### Обработчик события

Для передачи аргумента функции можно использовать стрелочную функцию

```jsx
doSomething = arg => {
    console.log(arg)
}

<button onClick={ () => this.doSomething(arg) }>Hello!</button>
```

### Стили

В элемент можно передать объект со стилями, все названия свойств длжны быть camelCase

```jsx
styles = {
  color: 'blue',
  margin: 10 // автоматически дополнится px. Другие еденицы нужно указывать явно: "10%"
}
// . . .
<h1 style={styles}>Hello!</h1>

<h1 style={{ color: "blue" }}>
```

### Списки

При передаче в качестве выражения массива или функции возвращающей массив, элементы извлекаются из него. Для того чтобы React мог следить за элементами списка, нужно присвоить каждому элементу `key`. Использовать в качесве `key`индекс элемента можно, но нежелательно, ибо они могут меняться, нужно что-то более уникальное. Уникальность должна быть только в пределах этого списка.

```jsx
render() {
  return (
    <React.Fragment>
      <ul>
        {['item1', 'item2'].map((i, index) => (
          <li key={i + index.toString()}>{i}</li>
        ))}
      </ul>
    </React.Fragment>
  );
}
```

## Компонент

{% code title="component.jsx" %}
```jsx
import React, { Component } from 'react';

class Counter extends Component {
  state = {};
  render() {
    return <h1>Hello!</h1>;
  }
}

ReactDom.render(<Counter />, document.getElementById("root"));
```
{% endcode %}

Чтобы вернуть из компонента два элемента без общего родителя, нужно либо обренуть их, либо использовать `React.Fragment`

```jsx
render() {
  return (
    <React.Fragment>
      Some text.
      <h2>A heading</h2>
    </React.Fragment>
  );
}
```

### state

Объект со всеми данными которые нужны компоненту, и обращаться к которому можно через `this`

{% code title="component.jsx" %}
```jsx
class Counter extends Component {
  state = {
    count: 0
  };
  render() {
    return <h1>Hello! Count is: {this.state.count}</h1>;
  }
}
```
{% endcode %}

### setState\(\)

Для записи в объект `state`используется метод `setState()` в который передается объект. Если свойство уже существует, оно перезапишется, если новое — добавится. Это метод сообщает что компонет изменится, React строит новый virtual dom и  сравнивает со старым. После, обновляет только изменения.

```jsx
increment() {
    this.setState({ count: this.state.count + 1});
}
```

### props

Каждый компонент имеет свойство `props`, по сути это объект который содержит все \(кроме `key`\)атрибуты которые были присвоены комопненту.

```jsx
<Counter key={id} value={value} />

class Counter extends Component {
  state = {};
  render() {
    return <div>{this.props.value}</div>;
  }
}
```

