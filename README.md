# REACT-REDUX-toolkit

- Contains functions to simplify and refactor Redux logic
- createSlice() function has a configuration object called options
- The object properties: name, initialState, reducers
- A case reducer is a method that updates state when an action is dipatched
- Code can be mutated in case reducer
- `createSlice()` returns an object with properties: name, reducer, actions, caseReducers.
- When exporting action crator and reducers use "duck" pattern.
- configureStore() function sets up store. Wrap around createStore() and combineReducers()
