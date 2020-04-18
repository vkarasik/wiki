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

JSX — Java Script XML. Позволяет создавать элементы как html шаблоны, и использовать внутри них любой валидный js используя скобки. JSX можно использовать как обычные объекты. Их можно возвращать из функций, их можно передавать как аргументы

```jsx
const heading = <h1 className="site-heading">Hello, React. Today is: {JSON.stringify(new Date)}</h1>

// То же самое можно делать так:

const heading = React.createElement('h1', { className: 'site-heading' }, 'Hello, React!')

// Вставляем в контейнер

ReactDOM.render(element, document.getElementById('root'));
```

Чтобы использовать автодополнения, компонент можно создавать с раширением .jsx, но по большому счету разницы нет, можно и .js

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

