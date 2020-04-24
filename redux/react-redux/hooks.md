React Redux now offers a set of hook APIs as an alternative to the existing connect() Higher Order Component. These APIs allow you to subscribe to the Redux store and dispatch actions, without having to wrap your components in connect().

⇒ 그니까, 결국 connect 로 하나하나 연결하는 대신 사용할 수 있도록 한다는거네.

[https://react-redux.js.org/api/hooks](https://react-redux.js.org/api/hooks)

# useDispatch

    const dispatch = useDispatch()

This hook returns a reference to the dispatch function from the Redux store. You may use it to dispatch actions as needed.

    // Basic Usage
    import React from 'react'
    import { useDispatch } from 'react-redux'
    
    export const CounterComponent = ({ value }) => {
      const dispatch = useDispatch()
    
      return (
        <div>
          <span>{value}</span>
          <button onClick={() => dispatch({ type: 'increment-counter' })}>
            Increment counter
          </button>
        </div>
      )
    }

# useSelector

    const result: any = useSelector(selector: Function, equalityFn?: Function)

Allows you to extract data from the Redux store state, using a selector function.

    // Basic Usage
    import React from 'react'
    import { useSelector } from 'react-redux'
    
    export const CounterComponent = () => {
      const counter = useSelector(state => state.counter)
      return <div>{counter}</div>
    }
    
    //Using props via closure to determine what to extract:
    
    import React from 'react'
    import { useSelector } from 'react-redux'
    
    export const TodoListItem = props => {
      const todo = useSelector(state => state.todos[props.id])
      return <div>{todo.text}</div>
    }
