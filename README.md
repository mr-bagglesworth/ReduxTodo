# Redux Todo list
> putting together freecodecamp's redux todo list with create-react-app

## To get going
- clone the repo
- **npm start** in the directory
- [http://localhost:3000](http://localhost:3000) to view it in the browser

### My Notes
- [my redux notes](https://hackmd.io/s3QcTxWsSLmQwHCstxmcdQ?both#one) - knowledge this basic app is built with
- started with [this as the first step](https://hackmd.io/s3QcTxWsSLmQwHCstxmcdQ?both#1-Manage-State-Locally-first)
- drew inspiration [from here](https://codesandbox.io/s/9on71rvnyo) for the second stage, but attempting to have one connection to the redux store, as opposed to multiple

### Architecture
> a diagram to help you and me understand the whole redux thing...

![redux data flow diagram](redux-dataflow.jpg "Basic data flow diagram")

### Things I learnt
- I can cut down on the amount of code I need to write with:
    - Named (as opposed to default) exports with function components
    - Destructuring when passing arguments into a function like so (sort of already knew this though I didn't practice it before):

**To destructure props:**    
```javascript
export const TodoAdd = ({ onChange, onClick, input }) => {
  return (
    <>
      <input className="text-input" onChange={onChange} value={input} />
      <button className="button" onClick={onClick}>
        Add
      </button>
    </>
  );
};
```
**To import the above:**
```javascript
import { TodoAdd } from "./TodoAdd";
```

- Redux makes you think of architecture **a lot**.
- Several files (action creators, action types, reducers, and selectors) need to be refactored from time to time in order to work together correctly.
- I can have one top level class component (**Interface.js**) that is connected to the Redux Store (through **InterfaceContainer.js**), and passes down all props to child function components. Multiple connector containers to Redux Store are not required.
- Not all application state should be added to the Redux store. For example, an unsubmitted todo list item is best stored in a component's local state.
- Multiple actions can be dispatched to props like so:
**InterfaceContainer.js**
```javascript
const mapDispatchToProps = dispatch => {
  return {
    submitTodo: function(message) {
      dispatch(addTodo(message));
    },
    toggleTodo: function(message) {
      dispatch(checkTodo(message));
    }
  };
};
```

- Reducers may be the trickiest files. They take an action / action creator and the state object, and return a new state object **without** mutating the original state. Think of the [reduce method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce).
- Multiple reducers should be combined and connected with the store.
- The structure of the state in the store may change when you combine reducers. It's reducer is added as a property, with the data it manages as it's value. Check the map state to props function to ensure you have the correct state you need for the component afterwards. The name of the reducer function will be the property you need in the state.
```javascript
const mapStateToProps = state => {
  return {
    messages: state.todoReducer, // messages: state before activeItem was added for menu
    activeItem: state.menuReducer
  };
};
```

- Selector functions (**selectors.js**) are used to modify the state from the store into a form that can be used by React Components. An example of this is in **TodoList.js**, where functions are used to:
  1. Combine an array of IDs relating to todo items, and an array of objects relating to todo item text and completed state.
  2. Filter the items according to current menu state - i.e. toggle completed items on menu item click
