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

### Middlewarre

- It is the suggested way to extend redux with custom functionality

  ```

  const redux = require("redux");
  const createStore = redux.createStore;
  const combineReducers = redux.combineReducers;
  const reduxLogger = require("redux-logger");
  const logger = reduxLogger.createLogger();
  const applyMiddleware = redux.applyMiddleware;
  const store = createStore(rootReducer, applyMiddleware(logger));

  ```

  ### Async action Creators

  Redux thunk- It allows the action creator to return a function instead of an object.

```
import axios, * as others from "axios";
const redux = require("redux");
const createStore = redux.createStore;
const combineReducers = redux.combineReducers;
const reduxLogger = require("redux-logger");
const logger = reduxLogger.createLogger();
const applyMiddleware = redux.applyMiddleware;

const thunkMiddleware = require("redux-thunk").default;

const FETCH_USERS_LOADING = "FETCH_USERS_LOADING";
const FETCH_USERS_SUCCESS = "FETCH_USERS_SUCCESS";
const FETCH_USERS_FAILURE = "FETCH_USERS_FAILURE";

function fetchUsersLoading() {
  return {
    type: FETCH_USERS_LOADING,
    payload: null
  };
}
function fetchUsersSuccess(data) {
  return {
    type: FETCH_USERS_SUCCESS,
    payload: data
  };
}
function fetchUsersFailure(err) {
  return {
    type: FETCH_USERS_FAILURE,
    payload: err
  };
}

const initialState = { loading: false, success: null, error: false };

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case FETCH_USERS_LOADING:
      return { ...state, loading: true };
    case FETCH_USERS_SUCCESS:
      return {
        ...state,
        loading: false,
        success: action.payload,
        error: false
      };
    case FETCH_USERS_FAILURE:
      return { ...state, loading: false, error: action.payload, success: null };
    default:
      return state;
  }
};

const rootReducer = combineReducers({
  user: reducer
});
const store = createStore(
  rootReducer,
  applyMiddleware(logger, thunkMiddleware)
);

const fetchUsers = () => (dispatch) => {
  dispatch(fetchUsersLoading());
  axios
    .get("https://jsonplaceholder.typicode.com/users")
    .then((res) => {
      dispatch(fetchUsersSuccess(res.data.map((i) => i.id)));
    })
    .catch((e) => {
      dispatch(fetchUsersFailure(e.message));
    });
};

const unsubscribe = store.subscribe(() =>
  console.log("updated state", store.getState())
);
store.dispatch(fetchUsers());

console.log(store.getState());

```
// unsubscribe();




### React-Redux

```
const mapStateToProps=(state,oldProps)=>{
    return {
     user:state.users.user
      }
}
const mapDispatchToProps=(dispatch,oldProps)=>{
      return {
      addUser:()=>dispatch(addUser()),
      }
}
export default connect (mapStateToProps,mapDispatchToProps)(UserFile)

  // using Hooks
const dispatch=useDispatch();
dispatch(addUser());
const userReducer=useSelector((state)=>state.users.user));

```

### async
- For performing async operations we use middleware Redux-thunk

```

import {
  GET_USERS_FAILURE,
  GET_USERS_LOADING,
  GET_USERS_SUCCESS
} from "./userTypes";
import axios from "axios";

export const getUsersSuccess = (data) => {
  return {
    type: GET_USERS_SUCCESS,
    payload: data
  };
};
export const getUsersLoading = (data) => {
  return {
    type: GET_USERS_LOADING,
    payload: null
  };
};
export const getUsersFailure = (err) => {
  return {
    type: GET_USERS_FAILURE,
    payload: err
  };
};

export const getUsers = () => (dispatch) => {
  dispatch(getUsersLoading());
  axios
    .get("https://jsonplaceholder.typicode.com/users")
    .then((res) => {
      console.log(res.data);
      dispatch(getUsersSuccess(res.data));
    })
    .catch((e) => {
      dispatch(getUsersFailure(e.message));
    });
};


```

### Redux toolkit

- Configuring redux in an app seems complicated.
- In addition to redux, a lot of other packages have to be installed to get redux to do something useful.
- Redux required too much of boilerplate code.
- Redux toolkit serves as an abstraction over redux. It hides the difficult parts ensuring you have a good developer experience.


### Immer
- To write better reducer.It is used because if there are nested state ,updating that state gonna be challenging.

```
  import {produce} from 'immer'
// without immer

return {...state,address:{...state.address,street:action.payload},}

// with immer
return produce(state,(draft)=>{draft.address.street=action.payload})
```


```

const createSlice = require("@reduxjs/toolkit").createSlice;
const initialState = {
  noOfCakes: 10
};

const cakeSlice = createSlice({
  name: "cake",
  initialState,
  reducers: {
    ordered: (state, action) => {
      state.noOfCakes--;
    },
    restocked: (state, action) => {
      state.noOfCakes += action.payload;
    }
  },
  extraReducers:{
['icecream/odered']:(state,action)=>{  
state.noOfCakes--;
},
extraReducers:(builder)=>{
builder.addCase(icecreamActions.ordered,(state,action)=>{state.noOfCakes--})
}
});

module.exports = cakeSlice.reducer;
module.exports.cakeActions = cakeSlice.actions;

const configureStore = require("@reduxjs/toolkit").configureStore;
const cakeReducer = require("./features/cake/cakeSlice");
const icecreamReducer = require("./features/icecream/icecreamSlice");
const reduxLogger = require("redux-logger");

const logger = reduxLogger.createLogger();
const store = configureStore({
  reducer: {
    cake: cakeReducer,
    icecream: icecreamReducer
  },
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger)
});
module.exports = store;


const store = require("./app/store");
const cakeActions = require("./app/features/cake/cakeSlice").cakeActions;

console.log(store.getState());

const unsubscribe = store.subscribe(() => {
  console.log("updated state", store.getState());
});

store.dispatch(cakeActions.ordered());
store.dispatch(cakeActions.ordered());
store.dispatch(cakeActions.ordered());
store.dispatch(cakeActions.restocked(4));



```


### Redux tookit example

https://codesandbox.io/s/redux-toolkit-react-3fxmzn?file=/src/app/features/user/UserView.jsx
https://codesandbox.io/s/redux-toolkit-777hgp?file=/src/app/store.js
