# React + Vite


# Usar hooks mais eficientes em react

Expanding on the anecdote application and integrating more advanced React hooks like `useReducer`and `useMemo`can enhance your understanding and provide a more robust approach to state management and optimizaion. Here's how you can refactor the application using these hooks:

## using `useReducer`

The `useReducer`hookn is typically preferred over ` useState` when you have more complex state logic that involves multiple sub-values or when the next state depends on the previous one. It's also handy when mananging in deeply nested components.

#### Refactoring with `useReducer`

##### 1. ** Define the reducer function: ** This function will handle the state transitions based on dispatched actions.

#### 2 Setup the useReducer hook

replace the useState for managing votes and selected anecdote.

### Using `useMemo`

The `useMemo`hook is usefol for memoizing expensive calculations. If determining the anecdote with most votes become computationally expensive (especially as list grows), `useMemo`can optimize this by recalcuting only when the votes array changes.

### Updated App Component with `useReducer`and `useMemo`

```
import { useReducer, useMemo } from 'react';

const anecdotes = [
  'If it hurts, do it more often.',
  'Adding manpower to a late software project makes it later!',
  // Add all other anecdotes here...
];

const initialState = {
  selected: 0,
  votes: new Array(anecdotes.length).fill(0)
};

function reducer(state, action) {
  switch (action.type) {
    case 'VOTE':
      const newVotes = [...state.votes];
      newVotes[state.selected] += 1;
      return { ...state, votes: newVotes };
    case 'NEXT_ANECDOTE':
      return { ...state, selected: Math.floor(Math.random() * anecdotes.length) };
    default:
      return state;
  }
}

const App = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  const maxVotesIndex = useMemo(() => {
    const max = Math.max(...state.votes);
    return state.votes.indexOf(max);
  }, [state.votes]);

  return (
    <div>
      <h1>Anecdote of the day</h1>
      <div>
        {anecdotes[state.selected]}
        <br />
        has {state.votes[state.selected]} votes
      </div>
      <button onClick={() => dispatch({ type: 'VOTE' })}>Vote</button>
      <button onClick={() => dispatch({ type: 'NEXT_ANECDOTE' })}>Next anecdote</button>
  
      <h1>Anecdote with most votes</h1>
      {state.votes[maxVotesIndex] > 0 ? (
        <div>
          {anecdotes[maxVotesIndex]}
          <br />
          has {state.votes[maxVotesIndex]} votes
        </div>
      ) : (
        <div>No votes yet.</div>
      )}
    </div>
  );
}

export default App;

```

## Key concepts and Changes

### . Reducer function

handles actions like voting and changing anecdotes. It takes the current state and an action, then returns the new state.

. useReducer Hook: Manages complex state logic more predictably.

. ùseMemo Hook`: Optimizes performance by memoizing the calculation of the anecdote with the most votes, preventing unnecessary recalculations.

This approach uses `useReducer`for state management , wich, makes the state transuitions predictable and centralized, and `useMemo`ensures that the application is eficient, recaulculating the mos voted anecdote only whem necessary. These hooks provide robust tools for building complex and performant React applications.
