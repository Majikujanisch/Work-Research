 [Zurück](Research.md)

# React

## Code Splitting

 One huge aspect of React and NextJS is Code Splitting. This happens through many ways, one are React.Components subclasses. These look like this:
``` jsx
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}
```
They are used to change data, react then will efficiantly update and re-render the components. They take in parameters called props or properties and return a hierarchy of views to display via the render method.

In order to keep everything clean many use JSX Syntax and with that you can write the Component above like this:
```jsx
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1',  null, "Shopping List for ", props.name),
  React.createElement('ul', null, 
  React.createElement("li", null, "Instagram"), 
  React.createElement("li", null, "WhatsApp"), 
  React.createElement("li", null, "Oculus"))
);
```
The ShoppingList can now be called like any other HTML attribute which makes making UIs easier by using small components.

To use the props of a component you use {this.props.value}.

For a component to remember something, they use state. These can be set in the constructor of a component like this:
```jsx
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => console.log('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

If its hard to follow the component tree logic you can install the react Devtools extension for chromium and firefox.[link](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)

## Function Components

In Case a class takes only props and has no state, it can be written as a function as following:
```jsx
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```