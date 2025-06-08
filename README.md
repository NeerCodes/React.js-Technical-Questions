# 📘 React Interview Questions & Answers Cheat Sheet

---

## 1. Component Lifecycle & Hooks

### 🔹 What are the phases of a React component's lifecycle?
- **Mounting**, **Updating**, and **Unmounting**

### 🔹 How does `useEffect` mimic lifecycle methods?
```js
useEffect(() => {}, []) // componentDidMount  
useEffect(() => {}, [dep]) // componentDidUpdate  
useEffect(() => { return () => {}; }, []) // componentWillUnmount  
```

### 🔹 What happens if dependencies are missing in `useEffect`?
- React may skip re-renders or use stale variables. Always include all external dependencies.

### 🔹 How to perform cleanup in `useEffect`?
```js
useEffect(() => {
  const timer = setTimeout(...);
  return () => clearTimeout(timer);
}, []);
```

### 🔹 Difference between `componentDidMount` and `useEffect(() => {}, [])`?
- Both run after mount, but `useEffect` runs after the paint.

### 🔹 Why is `componentWillMount` deprecated?
- It caused bugs with async rendering and is no longer safe to use.

### 🔹 When to use `useLayoutEffect`?
- Use when you need DOM measurements **before paint** (e.g., reading element dimensions).

---

## 2. Performance Optimization

### 🔹 What is `React.memo`?
- HOC to prevent unnecessary re-renders if props haven’t changed.

### 🔹 What is `useMemo()`?
- Memoizes the result of a computation.
```js
const result = useMemo(() => computeExpensiveValue(input), [input]);
```

### 🔹 What is `useCallback()`?
- Memoizes a function to avoid re-creation on re-renders.
```js
const handleClick = useCallback(() => doSomething(), [dependencies]);
```

### 🔹 How does React reconciliation work?
- React compares new and old virtual DOM using keys to update only changed elements.

### 🔹 Controlled vs Uncontrolled Components
- **Controlled**: React state manages input values.  
- **Uncontrolled**: DOM handles input values via `ref`.

### 🔹 How to prevent unnecessary re-renders?
- Use `React.memo`, `useCallback`, `useMemo`, and avoid anonymous functions in JSX.

---

## 3. State Management

### 🔹 `useState` vs `useReducer`
- `useState` is for simple state.
- `useReducer` is better for complex or related states.

### 🔹 Manage global state without Redux?
- Use **Context API** + `useReducer` or libraries like **Zustand**, **Jotai**, etc.

### 🔹 Pros and cons of Context API?
- ✅ Simple, no extra lib  
- ❌ Can cause performance issues with frequent updates

### 🔹 What is state lifting?
- Moving shared state up to a common ancestor to coordinate between components.

### 🔹 Nested form state structure?
```js
const [formData, setFormData] = useState({ user: { name: '', email: '' } });
```

---

## 4. Advanced React

### 🔹 What is a Higher-Order Component (HOC)?
```js
const Enhanced = withLogger(MyComponent);
```

### 🔹 What is a custom hook?
```js
function useCounter() {
  const [count, setCount] = useState(0);
  return { count, increment: () => setCount(c => c + 1) };
}
```

### 🔹 What are Portals?
```js
ReactDOM.createPortal(child, container);
```

### 🔹 What is render props?
```js
<DataProvider render={data => <Chart data={data} />} />
```

### 🔹 Server-side rendering (SSR)?
- HTML is rendered on the server and sent to client before React takes over.

### 🔹 Lazy loading and code splitting?
```js
const LazyComp = React.lazy(() => import('./MyComponent'));
```

### 🔹 Concurrent Mode?
- React can pause and resume rendering, improving responsiveness.

---


---

## 🔹 What is a Portal in React?

**Definition**:  
A **Portal** allows you to render a child into a different part of the **DOM tree**, outside the parent component’s hierarchy.

### 📌 Use Case:
- Modals
- Tooltips
- Dropdowns  
> Useful when you want content to **escape overflow or z-index** boundaries of parent containers.

### 🔹 Syntax:
```jsx
import ReactDOM from 'react-dom';

ReactDOM.createPortal(child, container);
```

### 🔸 Example: Modal using Portal

**index.html**
```html
<body>
  <div id="root"></div>
  <div id="modal-root"></div> <!-- Portal target -->
</body>
```

**Modal.js**
```jsx
import React from 'react';
import ReactDOM from 'react-dom';

const Modal = ({ children }) => {
  return ReactDOM.createPortal(
    <div className="modal">{children}</div>,
    document.getElementById('modal-root')
  );
};

export default Modal;
```

**App.js**
```jsx
<Modal>
  <h2>This is rendered in a portal!</h2>
</Modal>
```

### 🧠 Why is this useful in interviews?
- Shows understanding of **DOM hierarchy manipulation**
- Portals maintain **React event bubbling** even when rendered outside the parent DOM node
- Commonly used for **modals, toasts, popovers**


## 5. React with Tools

### 🔹 Fetching APIs
```js
useEffect(() => {
  fetch('/api/data')
    .then(res => res.json())
    .then(setData);
}, []);
```

### 🔹 Form Handling and Validation
- Use controlled components, `Formik`, `React Hook Form`, or validation libs like `Yup`.

### 🔹 Testing React Apps
- Use **Jest** for unit testing, **React Testing Library** for integration/UI.

### 🔹 Protected routes
```js
<Route path="/dashboard" element={auth ? <Dashboard /> : <Navigate to="/login" />} />
```

### 🔹 Error Boundaries
```js
class ErrorBoundary extends React.Component {
  static getDerivedStateFromError() { return { hasError: true }; }
  render() {
    return this.state.hasError ? <Fallback /> : this.props.children;
  }
}
```

---

## 6. Security & Best Practices

### 🔹 Common security issues?
- XSS via `dangerouslySetInnerHTML`. Always sanitize inputs.

### 🔹 Ensuring accessibility (a11y)?
- Use semantic HTML, ARIA roles, and test with screen readers.

### 🔹 Importance of keys in lists?
- Helps React identify what has changed. Use **unique, stable** keys.

---

## 7. Lifecycle Table: Class vs Hook

| Lifecycle Method         | Hook Equivalent                         |
|--------------------------|------------------------------------------|
| `componentDidMount`      | `useEffect(() => {...}, [])`             |
| `componentDidUpdate`     | `useEffect(() => {...}, [deps])`         |
| `componentWillUnmount`   | `useEffect(() => { return () => {...}; }, [])` |

---

## 8. Difference Between Class and Functional Components

| Feature | Class Component | Functional Component |
|--------|------------------|----------------------|
| Syntax | Uses `class` | Uses plain function |
| State | `this.state` | `useState()` hook |
| Lifecycle | Uses lifecycle methods | Uses `useEffect()` |
| `this` keyword | Required | Not required |
| Hooks support | ❌ | ✅ |
| Code | More verbose | Cleaner, concise |
| Performance | Slightly heavier | Lighter, faster |
| Best For | Legacy projects | Modern React apps |

### 🔹 Class Example
```js
class Counter extends React.Component {
  state = { count: 0 };
  increment = () => this.setState({ count: this.state.count + 1 });
  render() {
    return <button onClick={this.increment}>{this.state.count}</button>;
  }
}
```

### 🔹 Functional Example
```js
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```
---

## 🔹 React.memo vs useMemo vs useCallback

### 🧠 1. React.memo

**Definition**:  
`React.memo` is a **Higher-Order Component (HOC)** that prevents a functional component from re-rendering **if its props haven't changed**.

**✅ Use When**:
- Your component receives props that **don’t change frequently**.
- Component is **pure** (output depends only on props).

**📦 Example**:
```jsx
const MyButton = React.memo(({ label, onClick }) => {
  console.log("Rendering:", label);
  return <button onClick={onClick}>{label}</button>;
});
```

> With `React.memo`, `MyButton` only re-renders when `label` or `onClick` changes.

---

### 🧠 2. useMemo

**Definition**:  
`useMemo` is a **hook** that memoizes the **result of a computation**, so the value is **recalculated only when dependencies change**.

**✅ Use When**:
- The computation is **expensive**.
- You want to avoid **recalculating the same value** on every render.

**📦 Example**:
```jsx
const App = ({ number }) => {
  const expensiveValue = useMemo(() => {
    console.log("Calculating...");
    let result = 0;
    for (let i = 0; i < 1e6; i++) result += number;
    return result;
  }, [number]);

  return <div>Result: {expensiveValue}</div>;
};
```

---

### 🧠 3. useCallback

**Definition**:  
`useCallback` **memoizes a function** definition so that it is not recreated unless its **dependencies change**.

**✅ Use When**:
- You pass a function to a child component.
- You want to avoid **unnecessary re-renders** due to new function references.

**📦 Example**:
```jsx
const Parent = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Clicked");
  }, []);

  return (
    <>
      <Child onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </>
  );
};

const Child = React.memo(({ onClick }) => {
  console.log("Child rendered");
  return <button onClick={onClick}>Click Me</button>;
});
```

---

### 🔍 Differences Between `React.memo`, `useMemo`, and `useCallback`

| Feature         | `React.memo`                         | `useMemo`                                    | `useCallback`                            |
|----------------|--------------------------------------|----------------------------------------------|------------------------------------------|
| Type           | Higher-Order Component (HOC)         | Hook                                         | Hook                                     |
| Purpose        | Prevent re-render if props unchanged | Cache **computed value**                     | Cache **function reference**             |
| Used On        | Components                           | Values                                        | Functions                                |
| Example Use    | `<MyComponent />`                    | `const val = useMemo(() => ..., [deps])`     | `const fn = useCallback(() => ..., [])`  |
| Common Use Case| Avoid re-rendering child components  | Expensive calculations                       | Stable function references for children  |
