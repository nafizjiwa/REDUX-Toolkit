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
### Once the Object is created no need to create action objects or action creator
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
- createSlice({name,initialState,reducers}) automatically creates action creators</br>
- `createSlice({object})`--> RETURNS AN OBJECT with SLICE REDUCERS AND ACTION CREATORS:
||
||
    sliceName.reducer and sliceName.actions.</br>

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
           
          export const { addTodo, toggleTodo } = todosSlice.actions
 - ACTION CREATORS ARE ACCESSED BY PROPERTY `addTodo.actions`

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
         
        export const { addTodo, toggleTodo } = todosSlice.actions
        export default todosSlice.reducer
- REDUCER IS ACCESSED BY `todoSlice.reducer`
 
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







