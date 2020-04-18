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

JSX — Java Script XML. Позволяет создавать элементы как html шаблоны, и использовать внутри них js выражения.

```jsx
const heading = <h1 className="site-heading">Hello, React</h1>

// То же самое можно делать так:

const heading = React.createElement('h1', { className: 'site-heading' }, 'Hello, React!')

// Вставляем в контейнер

ReactDOM.render(element, document.getElementById('root'));
```



