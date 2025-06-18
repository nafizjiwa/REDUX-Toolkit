# REACT-REDUX-toolkit
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



