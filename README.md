## Redux

- Redux is a predictable state container for Javascript applications.
- It is library and can be used for any Javacript framework or library and even vanilla javascript.

### React-Redux

- It is a library that provides bindings to use React and Redux together in an application.


### Three core concepts in redux

1. Actions - It as an event that describes something that happened in the application.
2. Reducer - Ties the store and action together and carries out the state transition depending on the action.
3. Store - Holds the state of the application.


### Three principles

1. The state of the whole application is stored in an object tree in a single store.
2. The only way to change the state is to dispatch an action.
3. To specify how the state tree is transformed by actions,you write pure reducers

### Pure Reducers

- They are basically pure functions that take prevState and action as inputs and returns new state

  ```
  Reducer- (prevState,action)=>newState
  ```
