# REDUX-Toolkit

## INTRO
- Redux Toolkit STREAMLINES, SIMPLIES and SHORTENS written code as in:
  - Setting up the store with `configureStore( )`</br>
  - Creating reducers and action creators with `createSlice( )`</br>
  - Toolkit reduces boilerplate allows mutable code for fewer errors </br>
  - Toolkit has other useable functions</br>
## "SLICES" OF STATE
- A slice is a segment of the global state responsible for a particular feature. </br>
- Slices include data, reducers, actions creators for a specific feature </br>
- To reduce written code of SLICES Redux Toolkit has `createSlice( )`
## REFRACTORING with createSlice( ) create a slice 
- In Redux action types (AT), action creators (AC), and Reducers (R) are written out seperately and in seperate files.
- `createSlice(Configuration Object argument)` ---> `A SLICE`
    - Auto generates the components (AT), (AC), (R) to manage a slice `NO NEED TO WRITE THEIR CODE` </br>
- Create CONFIGURATION OBJECT for `createSlice()` - contains 3 properties  name, initialState, reducers
- The case reducers are similar to switch(action.type) statement cases
               
        const options = {                      //configObject name can be any name
            name: 'sliceName',           //Slice's name which generates - AT & AC
            initialState: someValue,         //reducers initial state value 
            reducers: {               //Object of reducers [key(type): value(methods)] pairs
                    //When action triggered that Method describes how the state is updated
                //This SLICE can handle these action Types(1), and (2)
                actionType(1): (state, action) => {
                    return ;          //Case reducers/methods for this SLICE has
                                        // 2 parameters: state, action 
                },
                actionType(2): (state, action) => { //method or case reducer
                    return ;       //returns how state is updated
                },
             }
        }
        const todosSlice = createSlice(options);   //takes in 1 parameter
---
### Once the Object is created no need to create action objects or action creator WHEN USING REDUX TOOLKIT createSlice will auto generate the Action Objects and Action Creator in the back.
---
-This action type: `(switch case)`

       const favoriteRecipesReducer = (state = initialState, action) => {
       case 'favoriteRecipes/addRecipe':
            return [...state, action.payload]
-Can be rewritten like: `(case reducer)`

       name: 'favoriteRecipes'
       initialState: []
       addRecipe: (state, action) => {
              return [...state, action.payload]
            },
## WRITING "Mutable" CODE WITH Immer
- Redux requires reducers not to mutate/change state directly </br>
- createSlice({configObject}) uses a library called `Immer` to safely “mutate” our state
- `Immer` - uses a JS object called `Proxy` to wraps the data
- 'Proxy' Allow the wrapped data to be mutated.</br>
- So functions which mutate code can be used
- Eg. Using Push -> state.push( ) because array.push( ) mutates the existing state array </br>
- Eg. Using Find -> state.find( ) because array.find( ) creates a new state array</br>

... The code logic can go from `immutable` </br>

    reducers: {
       addRecipe: (state, action) => {
         return [...state, action.payload]   
       },        //HERE PREVIOUSE STATE MUST BE COPIED INTO NEW STATE
    }
... To `Mutable` </br>

    reducers: {
        addRecipe: (state, action) => {
         state.push(action.payload)    //NO RETURN REQUIRED
        },        //THE NEW STATE DOES NOT HAVE PREVIOUS STATE
    }
## THE RETURNED OBJECT & AUTO-GENERATED ACTIONS FROM `createSlice( )`
- `createSlice( { name, initialState, reducers } )` SLICE --> returns an object { name, reducer, actions creators }</br>
- Generated Action Creators - accept 1 arguement (action.payload)
                            - are based on reducer functions</br>

                  const sliceName = {                sliceName/type1 sliceName/type2  sliceName/type3
                          name: sliceName,                         //prefix for generated action types
                          reducer: (state, action) => newState,      // Case reducer function
                          actions: {                             //Object of auto Generated action creators
                             actionCreator1: (payload) => (     --> action creator accepts 1 arg
                                    { type:'sliceName/actionCreatorName' , payload }
                              ), ...
                            },
                          caseReducers: {
                            actionType: (state, action) => newState
                          }
                      }
###### To Access this action from the return object { type: 'todos/addTodo', payload: 'walk dog' } </br>
  - As above the auto generated ~action creators~ accept one argument `payload`
  - {sliceName}Slice.actions.action(`payload`)</br>
  - todosSlice.actions.addTodo(`walk dog`) </br>

          export const { actionCreator1, ... } = 

## THE RETURN OBJECTS AND REDUCERS from 'createslice( )`

      "SLICE REDUCER"  
      {TheSlicesName}Slice.reducer --> represents ALL case reducers and actions of the SLICE
      todosSlice.reducer --> with actions addTodo and toggleTodo
|When an Aciton is Dispatch |Of Type: 'sliceName/action'|  |
|---|---|---|
||All Slice's | Check the action type using their Reducers |
|||If they have a match with their case reducers |
||| {sliceName}Slice.reducer( )|
|Is there a Match to case reducer?|MATCH:||
||YES| case reducer function is executed |
||NO | the current state is returned |
| EG. DIPATCHED ACTION TYPE |'todos/addTodo'| |
|| The Slice 'todosSlice' | Executes the reducer todosSlice.reducer() |
||The reducer matches the action to it's case reducers |todos.actions|
|||Finds 'todos/addTodo'|
|||Runs its code to update state|
- `ducks PATTERN` suggest exporting the reducer and action creators as NAMED EXPORTS.
- EXPORTING integrates the reducer to the store as slice of state.

          export const { AC1, AC2 } = [TheSlicesName]Slice.action;      //ACTION CREATORS (AC)
          export default [slice'sName]Slice.reducer                     //REDUCER default export
         
          export const { addTodo, toggleTodo } = todosSlice.actions
          export default todosSlice.reducer
###### TO ACCESS:
      -REDUCER ---> `todoSlice.reducer`
      -ACTION CREATORS ---> `addTodo.actions`
 
## CONVERTING THE STORE TO USE `configureStore( )`
- It wraps around createStore( ) and combineReducers( ) to simplify store setup.

      import { configureStore } from '@redux/toolkit
      const store = configureStore( { OBJECT } )
- OBJECT = 1. Has a reducer property or
         = 2. Object of slice reducers
         = Combine to create a root reducer 
###### - Reducer Property example

           const store = configure(                                          export const store = configure(
                   { reducer: {                                                   { reducer: {
                       todos: todosReducer,                                           slice1: reducer1,
                       filters: filtersReducer                                        slice2: reducer2,
                      }     // Define a state field                                  }
                   }        // named `todos`, handled by `todosReducer`                })                  
- Reducer combines all slice reducers into root reducer function no need for `combineReducers( )`
- The root reducer then creates a store no need for `createStore( )`

</br>
</br>

|REVIEW NOTES|
|---|
|RTX helps simplifies Redux logic with functions|
|`createSlice( confguration object param )` function ----> OBJECT PROPERTIES: name, initialState, reducers|
|createSlice() auto generates action creators and slice reducers|
|A case reducer: A method to update state when an action is dipatched|
|Code can be mutated in case reducer using `Immer`|
|`createSlice()` returns an object ----> OBJECT PROPERTIES: name, reducer, actions, caseReducers.|
|When exporting action creators and reducers use "duck" pattern.|
|`configureStore( )` function sets up store. Wrap around `createStore( )` and `combineReducers( )`|
|When slices are separate files, Export the action creators as named exports and the reducer as a default export.|
|export const { myActionCreator } = mySlice.actions;|
|export default mySlice.reducer;|

# DIFFERENCE BETWEEN createStore( ) and configureStore( )
- In Redux, createStore and configureStore set up the store to hold state.
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







