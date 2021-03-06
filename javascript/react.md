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

```jsx
<h1 className={name}>Hello!</h1>
```

### Обработчик события

При установке обработчика событий, метод утанавливаем без скобок — т.е. передаем ссылку на функцию.

```jsx
doSomething(){
    console.log('Action')
}

<h1 className={name} onClick={this.doSomething}>Hello!</h1>
```

Для того чтобы передать функции обработчику `this`, нужно в `counstructor`создать метод и вернуть в него функуцию с методом `.bind(this)` \(из прототипа\). Потому что методы объявленные в классе не привязаны к this.

```jsx
constructor() {
  super();
  this.handleIncrement = this.handleIncrement.bind(this); // привяжем this явно
}

handleIncrement() {
  console.log(this);
}

<button onClick={this.handleIncrement}>

// Другой способ передать this 
// используя факт что стрелочная функция не теряет this
// Здесь мы используем свойства классов. Оно доступно не во всех браузерах

handleIncrement = () => {
  console.log('clicked', this);
};



```

Для передачи аргумента функции можно использовать стрелочную функцию.

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

Чтобы установить что-то в `state`при инициализации компонента можно использвать `constructor()`

```jsx
state = {}

constructor(props){
  super(props);
  this.state = this.props.something
}
```

Состоянием должен управлять тот компонент которомому оно принадлежит. Таким образом, если событие дочернего компонента должно изменить state родителя, то мы должны поднять это событие в родитель и передать его в дочерний компонент через props.

```jsx
render() {
  return (
    <React.Fragment>
      {this.state.counters.map((counter) => (
        <Counter key={counter.id} onDelete={this.handleDelete} value={counter.value} selected={true} />
      ))}
    </React.Fragment>
  );
}

handleDelete = () => {
  console.log(1);
};

```

### setState\(\)

Для записи в объект `state`используется метод `setState()` в который передается объект. Если свойство уже существует, оно перезапишется, если новое — добавится. Это метод сообщает что компонет изменится, React строит новый virtual dom и  сравнивает со старым. После, обновляет только изменения. Метод доступен после рендера и размещения компонента в DOM.

```jsx
increment() {
    this.setState({ count: this.state.count + 1});
}
```

### props

Каждый компонент имеет свойство `props`, по сути это объект который содержит все \(кроме `key`\) атрибуты которые были присвоены комопненту. Отличия от state в том, что к state нет доступа извне комопонента. `Props` свойство только для чтения.

```jsx
<Counter key={id} value={value} />

class Counter extends Component {
  state = {};
  render() {
    return <div>{this.props.value}</div>;
  }
}
```

В функциональном стиле, без `state`

```jsx
const MyComponent = (props) => {
return ( <div>{props}</div> );
}

export default MyComponent;
```

### props.children

Если комопонет в JSX выражении содержит закрывающий тег, то все что находится внутри будет передано компоненту в свойстве `props.children`. 

```jsx
<Counter>
    <h1>Child</h1>
</Counter>
```

### lifecycle hooks

Компонент проходит через несколько фаз в течении своего жизнененнгого цикла. Есть несколько методов которые можно добавить, и реакт будет автоматически вызывать их на каждой фазе. Эти методы называются Lifecycle Hooks. Это позволяет нам выполнять какие-то действия в нужный момент. Основные фазы и хуки:

* Mounting \(монтирование\) — в этой фазе инстанс компонента создается и вставляется в DOM. 
  * constructor
  * render
  * componentDidMount — выполняется после размещения компонента в DOM
* Updating — фаза в которой меняется state или props

  * render
  * componentDidUpdate \(prevProps, prevState\)

  Unmounting — фаза в которой компонент удаляется из DOM

  * componentWillUnmount

