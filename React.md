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
They are used to change data, react then will efficiantly update and re-render the comp