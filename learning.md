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

The expression `Math.floor(Math.random() *random() * enecdotes.length)`is used to generate a random index for selecting an item from an array in this case an array of anecdotes.

## Components of the expression

### 1.`M̀ath.random`:

##### This function generates a random floating-point number between 0 and 1. That is,  it could be any number from 0 up to, but not including.

### 2. `Mathrandom() * anecdotes.length`:

##### This part scales the random number up to the length of the `anecdotes`array. For instance, if there are 8 anecdotes, `Math.random() `might initially generate 0.5. multiplying 0.5 by 8 results in 4.0.

##### The result here is a floating-point number ranging from 0 up to, nut not including, the length of the anecdote array. if `anecdotes.length`is 8 , then possiblle results of M̀ath.random() * anecdotes.length `might be numbers like 0.0 , 3.7, 7.9999, etc.

### 3. `M̀ath.floor(Math.random() * anecdotes.length)`

##### is used to round down the floating-point number to the nearest whole numbers.

##### Continuing the earlier example, if the `Math.random * anecdotes.length`resulted 4.0 , or 3.7 or 7,9999, then `Math.floor()`would convert 4,3 and 7 respectively.

## Pratical Use in the contect

By using this expression,n  you effectevily generate a random index that can be used to access an element of the `ànecdotes` array. This random index is used to display a random anecdote from the array whe, for example, a "Next anecdote" button is clicked in a React application. The use if `MAth.flloor()`ensures the index is a valid integer that can be used to access the array without errors.

In summary, ` Math.floor(Math.random() * anecdotes.length)`is a commom and efficient way to select a random item from an array in JavaScript, leveragin the randomness of ` Math.random()`and the integer conversion of `Math.floor()`.

### Understanding `useReducer` and `dispatch`

The `useReducer` hook is an alternative to `useState` for managing more complex state logic in React components. It's particularly useful when the next state depends on the previous one or when you need to handle multiple state transitions.

Here’s a quick rundown of how `useReducer` works:

* **Reducer Function**: This is a function you define that takes the current state and an action, then returns a new state based on that action. It's similar to how reducers work in Redux, if you're familiar with that library.
* **Dispatch Function**: This is a function that React provides when you use `useReducer`. You call `dispatch` to trigger an update to the state. When you call `dispatch` with an action, React passes that action to your reducer function to compute the new state.

### Usage in Your Component

In your component, the `useReducer` is set up to handle actions like changing anecdotes and voting. Each button click dispatches an action that tells the reducer how to change the state:

1. **Vote Button**:
   <pre><div class="dark bg-gray-950 rounded-md border-[0.5px] border-token-border-medium"><div class="flex items-center relative text-token-text-secondary bg-token-main-surface-secondary px-4 py-2 text-xs font-sans justify-between rounded-t-md"><span>jsx</span><div class="flex items-center"><span class="" data-state="closed"></span></div></div></div></pre>

* <pre><div class="dark bg-gray-950 rounded-md border-[0.5px] border-token-border-medium"><div class="overflow-y-auto p-4 text-left undefined" dir="ltr"><code class="!whitespace-pre hljs language-jsx"><button onClick={() => dispatch({ type: 'VOTE' })}>Vote</button>
  </code></div></div></pre>

  * **Action**: When the "Vote" button is clicked, the `dispatch` function is invoked with the action `{ type: 'VOTE' }`.
  * **Reducer Handling**: Your reducer function listens for an action of type `'VOTE'` and knows to increment the vote count for the currently selected anecdote.
* **Next Anecdote Button**:

  <pre><div class="dark bg-gray-950 rounded-md border-[0.5px] border-token-border-medium"><div class="flex items-center relative text-token-text-secondary bg-token-main-surface-secondary px-4 py-2 text-xs font-sans justify-between rounded-t-md"><span>jsx</span><div class="flex items-center"><span class="" data-state="closed"></span></div></div></div></pre>

1. <pre><div class="dark bg-gray-950 rounded-md border-[0.5px] border-token-border-medium"><div class="overflow-y-auto p-4 text-left undefined" dir="ltr"><code class="!whitespace-pre hljs language-jsx"><button onClick={() => dispatch({ type: 'NEXT_ANECDOTE' })}>Next anecdote</button>
   </code></div></div></pre>

   * **Action**: When the "Next anecdote" button is clicked, the `dispatch` function is triggered with the action `{ type: 'NEXT_ANECDOTE' }`.
   * **Reducer Handling**: For an action of type `'NEXT_ANECDOTE'`, the reducer function changes the `selected` state to a new random index, which corresponds to displaying a new anecdote.

### Why Use `dispatch`?

Using `dispatch` in this way has several benefits:

* **Encapsulation**: Actions encapsulate all the information needed to perform a state update, making the state changes predictable and traceable.
* **Simplicity**: It separates the logic of state updates from the UI, making your component easier to maintain and reason about, as the UI just dispatches actions and lets the reducer handle the logic.
* **Scalability**: This pattern can easily be scaled as the complexity of state management grows, without cluttering your components with complex state logic.

In summary, each button triggers a `dispatch` call that sends a specific action to your reducer. The reducer then decides how to update the state based on this action, resulting in the state changes that reflect in your component's UI. This pattern helps manage state changes in a structured and maintainable way, particularly suitable for complex interactions in larger applications.


# initial sState

### First you define the initialState for your component:

```
const initialSatate = {
selected: 0,
votes: new Array(anecdotes.length).fill(0)
};
```

This state object includes two properties

##### `slected:`then index of the currently displayed anecdote

` votes`an array initialized with zeros, where each element corresponds to the number of votes for each anecdote. The length of this array matches the number of anecdotes.

## Reducer function

The reducer funtion handles state transitions based on actions dispatched from the UI. Here's how the vote updating works inside your ` reducer`

```
fubction reducer(state, action) {
switch (action.type) { 
  case 'VOTE':
    const newVotes = [...state.votes]; // Step 1: Copy the votes array from the current state
    newVotes[state.selected] += 1; // Step 2: Increment tthe vote count for the currently selected anecdote
    return { ...state, votes: newVotes }; //Step 3: return the new state with the updated votes array.
```

### Step-by-Step Breakdown

#### 1. Copying the Votes Array: ` const newVotes = [...state.votes];`

###### This step involves creating a new copy of the `votes`array from the current state. It uses the spread  operator ` [...array]`to ensure that modifications to the `newVotes`array do not directly mutate the `votes `array in the state. This is crucial for maitaining immutability in React state management, which helps React optimize re-rendering.

##### incrementing the Vote ` neVotes[state.selected] +=1;

###### .       The funtion returns a new State object. It spreads the current state to maintain other stat values (like `selected`) and updates the ` votes`properly with the  `  newVotes`array. This updated state is then used by React to update the component and re-render if necessary.

### How it works in the Component

in the component, whenever the "Vote" button is clicked, the following happens:

- The button's `onClick`handler dispatches a `VOTE`action: `dispatch({ type: 'VOTE' })`.
- React passes this action to the reducer function.
- The reducer updates the state based on the logic defined in the `VOTE` case..
- The component receives the updated state and re-renders to reflect the incremented cote on the UI.

This implementation effectively manages the state changes for voting on anecdotes, leveraging React's `useReducer`for more predictable state transitions ins complex state scenarios.
