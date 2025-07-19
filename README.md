# REDUX-Toolkit

## INTRO
- Redux Toolkit SIMPLIES:
  - setting up the store with `configureStore( )`</br>
  - Creating reducers and aciton with `createSlice( )`</br>
  - Performs immutable update so fewer errors </br>
- Toolkit also uses other functions to help construct a Redux application </br>
## "SLICES" OF STATE
- A slice is a segment of the global state the focuses on a particular feature. </br>
- Includes data, reducers, actions and selectors for a specific functionality </br>
- Each slice of state has a reducer which manages that slice </br>
## REFRACTORING create a slice with createSlice( )
- In Redux we must write action types (AT), action creators (AC), and reducers seperately
- `createSlice(Object argument)`
      - It auto generates action types, action creators, and reducers so we don't need to write them </br>
      
                               
        const options = {                      //configObject any name
            name: 'sliceName',                //name of the slice
                                              //Used to generate the AT and AC
            initialState: someValue,         //reducers initialize state value 
            reducers: {                      //an object of key: value pairs
                //The slice's case reducers have 2 parameters: state, action 
                //actionType: A Methods with directions how to change this actions state
                method1: (state, action) => {
                    return ;
                },
                method2: (state, action) => {
                    return ;
                },
             }
        }
        const todosSlice = createSlice(options);   //takes in 1 parameter

## WRITING "Mutable" CODE WITH Immer
- Redux requires not mutating/changing state directly but coping with (...spread).</br>
- A createSlice({configObject}) library called `Immer` uses a `Proxy` an object to wrap the data this allows Mutation of the code.</br>
- Eg. Using Push - state.push( ) because array.push( ) mutates the existing array </br>
- Eg. Using Find - state.find( ) because array.find( ) creates a new array</br>

... The code logic can go from immutable </br>

    reducers: {
       addRecipe: (state, action) => {
         return [...state, action.payload]
       },
    }
... To Mutable </br>

    reducers: {
        addRecipe: (state, action) => {
          return state.addRecipe.push(action.payload)
        },
    }
## `createSlice( )` RETURNS AN OBJECT & AUTO-GENERATED ACTIONS
- createSlice({name,initialState,reducers}) automatically creates action creators</br>
- `createSlice({object})` --> RETURNS AN OBJECT with SLICE REDUCERS AND ACTION CREATORS: sliceName.reducer and sliceName.actions.</br>

                    sliceName = {                sliceName/type1 sliceName/type2  sliceName/type3
                          name: sliceName,                         //prefix for generated action types
                          reducer: (state, action) => newState,      // Case reducer function
                          actions: {                             //Object of auto Generated action creators
                             actionCreatorType1: (payload) => ({type:'sliceName/actionType1Name' , payload}),
                             actionCreatorType2: (payload) => ({type:'sliceName/actionType2Name' , payload})
                            },
                          caseReducers: {
                            actionType: (state, action) => newState
                          }
                      }
                              
##### >>>>>>>>>>>>>> console.log({sliceName}Slice.actions.action('payload') </br>
##### >>>>>>>>>>>>>> console.log({todos}Slice.actions.addTodo('walk dog')) </br>
##### >>>>>>>>>>>>>> // {type: 'todos/addTodo', payload: 'walk dog'} </br>
- The generated action creators names are based on reducer functions names
- createSlice() generates action creators and reducers 
- EXPORT action creators TO USE IN OTHER FILES:

          export const { actionCreator1, actionCreator2 } = {TheReduxSlicesName}Slice.actions
            - ACTION CREATORS ARE ACCESSED BY PROPERTY addTodo.actions
          export const { addTodo, toggleTodo } = todosSlice.actions

## RETURN OBJECTS AND REDUCERS
- The Reducer within the returned object of createSlice( )</br>

      "SLICE REDUCER"  
      {TheReduxSlicesName}Slice.reducer --> represents collection of ALL case reducers
      
|Dispatch an Action of Type ---> |'sliceName/action'|  |
|---|---|---|
|| {sliceName}Slice | Uses reducer {sliceName}Slice.reducer( ) |
||| checks if dispatched action's type aligns with any case reducers |
|| Find Match? |   |
|| YES MATCH| the matching case reducer function is executed |
|| NO MATCH | the current state is returned |
| DIPATCH ACTION TYPE |'todos/addTodo'| |
|| the Slice 'todosSlice' | employs reducer todosSlice.reducer() |
||to check if action aligns with any case reducers in |todos.actions|
|||Finds 'todos/addTodo'|

- `ducks` pattern suggest exporting action creators seperate from the reducer.
- EXPORT so it can integrate into global store and used as slice of state.

        export const { AC1, AC2 } = todosSlice.actions;      //ACTION CREATORS (AC)
        export default sliceNameSlice.reducer                     //REDUCER
         - REDUCER IS ACCESSED BY todoSlice.reducer
        export default todosSlice.reducer
 
## CONVERTING THE STORE TO USE `configureStore( )`
- `configureStore( )` setup store by wrapping around createStore( ) and combineReducers( )

      import { configureStore } from '@redux/toolkit
      const store = configureStore({INPUT OBJECT})
- INPUT OBJECT = A reducer property which defines a function, or An object of slice reducers both to create a root reducer.
            
            Input Object is Reducer Property
           const store = configure(                                          export const store = configure(
                           { reducer: {                                                   { reducer: {
                              todos: todosReducer,                                           slice1: reducer1,
                              filters: filtersReducer                                        slice2: reducer2,
                             }     // Define a state field                                  }
                                  // named `todos`, handled by `todosReducer`                                      
               }                                                                             })
- Reducer combines all slice reducers into root reducer function no need for `combineReducers( )`
- The root reducer creates a store no need for `createStore( )`

</br>
</br>

|REVIEW NOTES|
|---|
|Helps simplify and refactor Redux logic with functions and methods|
|`createSlice(param=options)` function has a configuration object parameter----> OBJECT PROPERTIES: name, initialState, reducers|
|A case reducer: A method to update state if a specific action is dipatched|
|Code can be mutated in case reducer|
|`createSlice()` returns an object ----> OBJECT PROPERTIES: name, reducer, actions, caseReducers.|
|When exporting action creators and reducers use "duck" pattern.|
|`configureStore( )` function sets up store. Wrap around `createStore( )` and `combineReducers( )`|
|When slices are separate files, Export the action creators as named exports and the reducer as a default export.|
|createSlice() generates action creators and slice reducers to access them:|
|export const { myActionCreator } = mySlice.actions;|
|export default mySlice.reducer;|

# DIFFERENCE BETWEEN createStore( ) and configureStore( )


In Redux, createStore and configureStore set up the store for your state to live in.
Compare:

### 🏗️ createStore (Classic Redux)
- In the redux library.
- Requires manual setup using createStore() and combineReducers().
- Middleware (like redux-thunk) is added with applyMiddleware().

        import { createStore, combineReducers, applyMiddleware } from 'redux';
        import thunk from 'redux-thunk';
        
        const rootReducer = combineReducers({
          // your reducers
        });
        
        const store = createStore(rootReducer, applyMiddleware(thunk));

### 🚀 configureStore (Redux Toolkit)
- imported with `@reduxjs/toolkit`
- configureStore( ) Automatically sets up the store
- No need for combineReducers() and applyMiddleware() Built-in support for redux-thunk 
- Accepts an object with reducer, where you define slice reducers.

      import { configureStore } from '@reduxjs/toolkit';
      import todosReducer from './todosSlice';
      
      const store = configureStore({
        reducer: {
          todos: todosReducer,
          // other slices
        }
      });







