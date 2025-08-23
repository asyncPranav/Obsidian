
---

# What problem does RTK solve?

- In React, **local state** (`useState`) is perfect when only one component needs it.
    
- When **many components** need the same data (user, cart, theme, todos), passing props up/down becomes painful.
    
- **Redux** solves this by giving you a **single global store** for app state.
    
- **Redux Toolkit** is the **modern, official** way to write Redux—less boilerplate, safer updates, helpful defaults.
    

---

# The core pieces (in the order you’ll meet them)

## 1) **Store**

**What:** A single object that holds **all** global state for your app.  
**Why:** One place to read/update shared data.  
**Where:** `src/app/store.js`  
**How:** Created with `configureStore`.

```js
// app/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {                       // root reducer object
    counter: counterReducer        // state.counter -> managed by counterSlice
  },
});
```

**Gotcha:** You usually have **one** store per app.

---

## 2) **Provider**

**What:** A React component that makes the store available to **all** components.  
**Where:** In your entry file (`main.jsx` or `index.jsx`).  
**How:** Wrap `<App />` with `<Provider store={store}>`.

```jsx
// main.jsx
import { Provider } from 'react-redux';
import { store } from './app/store';

root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

**Gotcha:** Without `Provider`, `useSelector`/`useDispatch` won’t work.

---

## 3) **Slice**

**What:** A “section” of the store that handles one feature (e.g., `counter`, `todos`).  
**Contains:**

- `initialState` (data)
    
- `reducers` (how data changes)
    
- auto-generated **action creators**  
    **Where:** `src/features/<feature>/<feature>Slice.js`  
    **How:** Use `createSlice`.
    

```js
// features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const initialState = { value: 0 };

const counterSlice = createSlice({
  name: 'counter',     // prefix for action types
  initialState,
  reducers: {
    increment: (state) => { state.value += 1 },                  // safe "mutation"
    decrement: (state) => { state.value -= 1 },
    incrementByAmount: (state, action) => { state.value += action.payload }
  }
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions; // action creators
export default counterSlice.reducer; // the reducer for the store
```

**Why it’s nice:**

- You write “mutating” code (`state.value += 1`) but **Immer** makes immutable updates behind the scenes.
    

**Gotcha:** In a reducer, do **either** mutate the draft **or** return a new object—**not both**.  
Bad: `state.value += 1; return {...state}` → Immer error.

---

## 4) **Reducer**

**What:** A pure function that decides _how_ state changes for a given action.  
**Where:** Inside your slice (under `reducers`) or in `extraReducers`.  
**Signature:** `(state, action) => newState | mutate state`  
**Example:** `increment`, `decrement` above.

**Gotcha:** No async work here. No side-effects. Pure logic only.

---

## 5) **Action** and **Action Creator**

**Action (object):** `{ type: 'counter/increment', payload?: any }`  
**Action Creator (function):** A function that returns an action object for you.

```js
increment()        // => { type: 'counter/increment' }
incrementByAmount(5) // => { type: 'counter/incrementByAmount', payload: 5 }
```

**Where they come from:** `createSlice` generates them from your `reducers` keys.

---

## 6) **Dispatch**

**What:** A function you call to send an action to the store.  
**Where:** Inside your React component using `useDispatch()`.

```jsx
// features/counter/Counter.jsx
import { useDispatch, useSelector } from 'react-redux';
import { increment, decrement, incrementByAmount } from './counterSlice';

export default function Counter() {
  const count = useSelector((state) => state.counter.value); // read state
  const dispatch = useDispatch();                             // send actions

  return (
    <>
      <h1>{count}</h1>
      <button onClick={() => dispatch(increment())}>+1</button>
      <button onClick={() => dispatch(decrement())}>-1</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>+5</button>
    </>
  );
}
```

**Flow in your head:**  
UI click → `dispatch(action)` → Redux runs reducer → store updates → subscribed components re-render.

---

## 7) **Selector** / `useSelector`

**What:** A function that **reads** specific data from the store.  
**Why:** Keeps components focused on just the data they need → fewer re-renders.  
**How:** `useSelector((state) => state.counter.value)`

```js
const count = useSelector((state) => state.counter.value);
```

**Tip:** Create reusable selectors in the slice file for bigger apps.

---

## 8) **Immer (under the hood)**

**What:** A library RTK uses to let you write **mutable-looking** code safely.  
**Why:** Writing immutable updates is hard; Immer makes it easy.  
**Rule:** In a reducer, **mutate OR return**—don’t do both.

Bad:

```js
increment: (state) => {
  state.value += 1;
  return { ...state };   // ❌ will crash
}
```

Good:

```js
increment: (state) => { state.value += 1 } // ✅ mutate only
// OR
increment: (state) => ({ value: state.value + 1 })) // ✅ return only
```

---

## 9) **configureStore options**

```js
configureStore({
  reducer: { counter: counterReducer },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: true, // warns if you put non-serializable stuff in state/actions
      immutableCheck: true,
    }),
  devTools: process.env.NODE_ENV !== 'production', // Redux DevTools
});
```

**Terms:**

- **Middleware:** Functions that can intercept actions (for logging, async, etc.).
    
- **DevTools:** Browser extension to inspect actions/state history.
    
- **Serializable check:** Redux expects plain JS objects/arrays/numbers/strings in state and actions.
    

---

## 10) **Async with `createAsyncThunk`** (level-up)

**What:** Helper to handle async logic (fetching data) with lifecycle actions.  
It auto-creates `pending`, `fulfilled`, `rejected` actions you handle in `extraReducers`.

```js
// features/users/userSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

export const fetchUsers = createAsyncThunk(
  'users/fetch',
  async () => {
    const res = await fetch('https://jsonplaceholder.typicode.com/users');
    return await res.json();
  }
);

const userSlice = createSlice({
  name: 'users',
  initialState: { list: [], status: 'idle', error: null },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending,   (state) => { state.status = 'loading' })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.list = action.payload;
      })
      .addCase(fetchUsers.rejected,  (state, action) => {
        state.status = 'failed';
        state.error = action.error.message;
      });
  },
});

export default userSlice.reducer;
```

**UI usage:**

```jsx
const dispatch = useDispatch();
useEffect(() => { dispatch(fetchUsers()) }, [dispatch]);
```

**Terms here:**

- **extraReducers:** Handle actions **not defined** in `reducers` (e.g., async thunks, actions from other slices).
    
- **builder API:** A type-safe way to add cases one by one.
    

---

## 11) **RTK Query** (optional advanced)

A data-fetching layer built into RTK (caching, deduping, auto-loading states).  
You define endpoints instead of writing thunks manually. Great for API-heavy apps.

---

## 12) **createEntityAdapter** (optional advanced)

Helpers to manage normalized collections (e.g., lists of items) with IDs efficiently.

---

# A quick mental model (the “movie” that plays on click)

1. You click “+1”
    
2. Component calls `dispatch(increment())`
    
3. Redux finds `counterSlice.reducer`
    
4. Runs `increment` → `state.value += 1`
    
5. Immer creates new immutable state behind the scenes
    
6. Store updates → subscribed components re-render
    
7. `useSelector` reads the new `state.counter.value` → UI shows updated count
    

---

# Common mistakes (and how to avoid them)

- **Mutate + return** in the same reducer → Immer error.  
    Use one style only.
    
- **Forgetting Provider** → `useSelector`/`useDispatch` fail.
    
- **Wrong state path** in `useSelector` → `undefined`.  
    Check the key you registered in `configureStore`.
    
- **Non-serializable data** in state/actions → console warnings.  
    Keep state plain (no class instances, DOM nodes, Promises, Dates if possible)

---

