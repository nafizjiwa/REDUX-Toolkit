# REACT-REDUX-toolkit

## INTRO
- Helps simplify the store setup </br>
- no need to add additonal packages</br>
- less boierplate and fewer errors from immutable updates </br>
- Toolkit contains functions to construct a Redux application </br>
- Most important methods are createSlice( ) and configureStore( ) </br>
## "SLICES" OF STATE
- A slice is a segment of the global state the focuses on a particular feature. </br>
- Includes data, reducers, actions and selectors for a specific functionality </br>
- Each slice of state's reducer is responsible to manage its own slice </br>
## REFRACTORING with createSlice( )
- `createSlice(configObject)` - generates action types, action creators, reducers with 1 parameter </br>

Configuration Object = { </br>
------- name: name of slice,   </br>
------- initialState: reducers initial state value   </br>
------- reducers: { an object of key: value pairs    </br>
------------ action type: a method with directions how to change state for this action   </br>
------- } </br>
}</br>
                                           
        const options = {
            name: 'sliceName',
            initialState: someValue,
            reducers: {
              // case reducers methods for the slice.
              // reducers 2 parameters state, action
                method1: (state, action) => {
                    return ;
                },
                method2: (state, action) => {
                    return ;
                },
             }
        }
        const todosSlice = createSlice(options);

## WRITING "Mutable" CODE WITH Immer
- Redux requires not mutating/changing state directly but coping with (...spread).
- A createSlice(configObject) library called `Immer` uses a `Proxy` an object to wrap the data this allows Mutation of the code.
- Eg. Using Push - state.push( ) because array.push( ) mutates the existing array
- Eg. Using Find - state.find( ) because array.find( ) creates a new array

... The code logic can go from immutable 

    reducers: {
       addRecipe: (state, action) => {
         return [...state, action.payload]
       },
    }
... To Mutable

    reducers: {
        addRecipe: (state, action) => {
          return state.addRecipe.push(action.payload)
        },
    }
## RETURNED OBJECTS AND AUTO-GENERATED ACTIONS





|REVIEW NOTES|
|---|
|- Helps simplify and refactor Redux logic with functions and methods|
|- `createSlice(param=options)` function has a configuration object parameter----> OBJECT PROPERTIES: name, initialState, reducers|
|- A case reducer: A method to update state if a specific action is dipatched|
|- Code can be mutated in case reducer|
|- `createSlice()` returns an object ----> OBJECT PROPERTIES: name, reducer, actions, caseReducers.|
|- When exporting action creators and reducers use "duck" pattern.|
|- `configureStore( )` function sets up store. Wrap around `createStore( )` and `combineReducers( )`|
|=========================================================================================|



