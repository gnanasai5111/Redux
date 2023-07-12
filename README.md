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
  
### Terminology

  - Action is an object that contains type as a property.You can also add extra properties like payload etc.
  - Action Creator is a function that returns an action.

```
// Action
const BUY_CAKE = "BUY_CAKE";

// Action Creator 
function buyCake() {
  return {
    type: BUY_CAKE,
    payload: "First redux action"
  };
}

// default or initial state
const initialState = { cakes: 10 };

// reducer
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case BUY_CAKE:
      return { ...state, cakes: state.cakes - 1 };
    default:
      return state;
  }
};
```

### getState()

- Allows access to the state via getState()

### dispatch(action)

- Allows state to be updated via dispatch(action)

### subscribe(listener)

- Registers listeners via subscribe(listener)

### Example

```
// Importing the library
const redux = require("redux");

// Creating the store
const createStore = redux.createStore;

//Creating an action
const BUY_CAKE = "BUY_CAKE";

// Creating an Action Creator
function buyCake() {
  return {
    type: BUY_CAKE,
    payload: "First redux action"
  };
}

// Initial state
const initialState = { cakes: 10 };

// Defining the reducer 
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case BUY_CAKE:
      return { ...state, cakes: state.cakes - 1 };
    default:
      return state;
  }
};


// Creating the store and passsing the reducer 
const store = createStore(reducer);


// To get the state of application
console.log(store.getState());

// To Listen to changes in the store.It returns a function 
const unsubscribe = store.subscribe(() =>
  console.log("updated state", store.getState())
);

// Dispatching an action
store.dispatch(buyCake());

// Unsubscribing the store
unsubscribe();

// console.log(store.getState());


```

### Using multiple reducers

```
const redux = require("redux");
const createStore = redux.createStore;
const combineReducers = redux.combineReducers;

const BUY_CAKE = "BUY_CAKE";
const BUY_ICECREAM = "BUY_ICECREAM";
const ADD_CAKE = "ADD_CAKE";
const ADD_ICECREAM = "ADD_ICECREAM";

function buyCake() {
  return {
    type: BUY_CAKE,
    payload: "First redux action"
  };
}
function buyIcecream() {
  return {
    type: BUY_ICECREAM,
    payload: "First redux action"
  };
}
function addCake() {
  return {
    type: ADD_CAKE,
    payload: "First redux action"
  };
}
function addIcecream() {
  return {
    type: ADD_ICECREAM,
    payload: "First redux action"
  };
}
const initialCakeState = { cakes: 10 };
const initialIcecreamState = { icecream: 5 };

const cakeReducer = (state = initialCakeState, action) => {
  switch (action.type) {
    case BUY_CAKE:
      return { ...state, cakes: state.cakes - 1 };
    case ADD_CAKE:
      return { ...state, cakes: state.cakes + 1 };
    default:
      return state;
  }
};

const icecreamReducer = (state = initialIcecreamState, action) => {
  switch (action.type) {
    case BUY_ICECREAM:
      return { ...state, icecream: state.icecream - 1 };

    case ADD_ICECREAM:
      return { ...state, icecream: state.icecream + 1 };
    default:
      return state;
  }
};

// Combine reducers for using multiple reducers
const rootReducer = combineReducers({
  cake: cakeReducer,
  icecream: icecreamReducer
});
const store = createStore(rootReducer);

console.log(store.getState());
const unsubscribe = store.subscribe(() =>
  console.log("updated state", store.getState())
);
store.dispatch(buyCake());
store.dispatch(buyIcecream());
store.dispatch(addIcecream());
store.dispatch(addCake());
unsubscribe();

```
