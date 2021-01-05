### Provider
React Redux provides <Provider />, which makes the Redux store available to the rest of your app:

import React from 'react'
import ReactDOM from 'react-dom'

    import { Provider } from 'react-redux'
    import store from './store'

    import App from './App'

    const rootElement = document.getElementById('root')
    ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    rootElement
    )
    

### connect
React Redux provides a `connect` function for you to connect your component to the store.

    import { connect } from 'react-redux'
    import { increment, decrement, reset } from './actionCreators'

    // const Counter = ...

    const mapStateToProps = (state /*, ownProps*/) => {
    return {
        counter: state.counter,
    }
    }

    const mapDispatchToProps = { increment, decrement, reset }

    export default connect(mapStateToProps, mapDispatchToProps)(Counter)

React Redux provides a connect function for you to read values from the Redux store (and re-read the values when the store updates).

The connect function takes two arguments, both optional:

- mapStateToProps: called every time the store state changes. It receives the entire store state, and should return an object of data this component needs.

- mapDispatchToProps: this parameter can either be a function, or an object.

  - If it’s a function, it will be called once on component creation. It will receive dispatch as an argument, and should return an object full of functions that use dispatch to dispatch actions.
  - If it’s an object full of action creators, each action creator will be turned into a prop function that automatically dispatches its action when called. Note: We recommend using this “object shorthand” form.
Normally, you’ll call connect in this way:

    const mapStateToProps = (state, ownProps) => ({
    // ... computed data from state and optionally ownProps
    })

    const mapDispatchToProps = {
    // ... normally is an object full of action creators
    }

    // `connect` returns a new function that accepts the component to wrap:
    const connectToStore = connect(
    mapStateToProps,
    mapDispatchToProps
    )
    // and that function returns the connected, wrapper component:
    const ConnectedComponent = connectToStore(Component)

    // We normally do both in one step, like this:
    connect(
    mapStateToProps,
    mapDispatchToProps
    )(Component)

#### If you call connect without providing any arguments, your component will:

- not re-render when the store changes
- receive props.dispatch that you may use to manually dispatch action
```
// ... Component
export default connect()(Component) // Component will receive `dispatch` (just like our <TodoList />!)

```

#### If you call connect with only mapStateToProps, your component will:

- subscribe to the values that mapStateToProps extracts from the store, and re-render only when those values have changed
- receive props.dispatch that you may use to manually dispatch action

        // ... Component
        const mapStateToProps = state => state.partOfState
        export default connect(mapStateToProps)(Component)

#### If you call connect with only mapDispatchToProps, your component will:

- not re-render when the store changes
- receive each of the action creators you inject with
mapDispatchToProps as props and automatically dispatch the actions upon being called

    import { addTodo } from './actionCreators'
    // ... Component
    export default connect(
    null,
    { addTodo }
    )(Component)

#### If you call connect with both mapStateToProps and mapDispatchToProps, your component will:

- subscribe to the values that mapStateToProps extracts from the store, and re-render only when those values have changed
- receive all of the action creators you inject with mapDispatchToProps as props and automatically dispatch the actions upon being called.

        import * as actionCreators from './actionCreators'
        // ... Component
        const mapStateToProps = state => state.partOfState
        export default connect(
        mapStateToProps,
        actionCreators
        )(Component)

### Connect: Extracting Data with `mapStateToProps`

As the first argument passed in to `connect`, `mapStateToProps` is used for selecting the part of the data from the store that the connected component needs. It’s frequently referred to as just `mapState` for short.

- It is called every time the store state changes.
- It receives the entire store state, and should return an object of data this component needs.

### Defining mapStateToProps

`mapStateToProps` should be defined as a function:

`function mapStateToProps(state, ownProps?)`

It should take a first argument called state, optionally a second argument called ownProps, and return a plain object containing the data that the connected component needs.

This function should be passed as the first argument to connect, and will be called every time when the Redux store state changes. If you do not wish to subscribe to the store, pass null or undefined to connect in place of mapStateToProps.

state:

The first argument to a mapStateToProps function is the entire Redux store state (the same value returned by a call to store.getState()). Because of this, the first argument is traditionally just called state. (While you can give the argument any name you want, calling it store would be incorrect - it's the "state value", not the "store instance".)

Return
Your mapStateToProps function should return a plain object that contains the data the component needs:

- Each field in the object will become a prop for your actual component
- The values in the fields will be used to determine if your component needs to re-render

### What is a Redux selector?
A `“selector”` is simply a function that accepts Redux state as an argument and returns data that is derived from that state.

Let’s start by importing `useSelector` from the `react-redux` library. `useSelector` is a function that takes the current state as an argument and returns whatever data you want from it. It’s very similiar to `mapStateToProps()` and it allows you to store the return values inside a variable within the scope of you functional components instead of passing down as props.

```
import {useSelector} from 'react-redux'
export default function UserPage() {
    const firstName = useSelectore(state => state.userFirstName)
    const lastName = useSelector(state => state.userLastName)
    return (
        <>
          <div>{firstName}</div>
          <div>{lastName}</div>
        <>
    )
}

```
