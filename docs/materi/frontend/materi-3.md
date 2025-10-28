---
title: Materi 3
sidebar_position: 3
---

# Intro to React Js (Installation, Functional Component, Props, Lifecycle)

# Introduction to React.js - Complete Beginner's Guide

## What is React?

React is a powerful JavaScript library developed by Facebook for building user interfaces, particularly web applications. Think of React as a tool that helps you create interactive websites by breaking them down into reusable pieces called **components**.

### Why React?

- **Component-Based**: Build encapsulated components that manage their own state
- **Reusable**: Write once, use anywhere in your application
- **Fast**: Virtual DOM makes updates efficient
- **Popular**: Huge community and job market demand

---

## 1. Installation & Setup

### Prerequisites

Before starting with React, make sure you have:

- **Node.js** (version 14 or higher) - Download from [nodejs.org](https://nodejs.org)
- Basic knowledge of **HTML**, **CSS**, and **JavaScript**

### Method 1: Create React App (Recommended for Beginners)

```bash
# Install Create React App globally
npm install -g create-react-app

# Create a new React project
npx create-react-app my-first-app

# Navigate to your project
cd my-first-app

# Start the development server
npm start
```

### Method 2: Vite (Faster Alternative)

```bash
# Create new project with Vite
npm create vite@latest my-react-app -- --template react

# Navigate and install dependencies
cd my-react-app
npm install

# Start development server
npm run dev
```

### Project Structure Overview

```
my-first-app/
├── public/
│   └── index.html
├── src/
│   ├── App.js
│   ├── App.css
│   ├── index.js
│   └── index.css
├── package.json
└── README.md
```

---

## 2. Functional Components

### What are Components?

Components are like JavaScript functions that return HTML (JSX). They're the building blocks of React applications.

### Basic Functional Component

```jsx
// Simple component
function Welcome() {
  return <h1>Hello, World!</h1>;
}

// Arrow function version (RECOMMENDED ONE by Convention)
const Welcome = () => {
  return <h1>Hello, World!</h1>;
};

// Export to use in other files
export default Welcome;
```

### JSX - JavaScript XML

JSX lets you write HTML-like code in JavaScript:

```jsx
function MyComponent() {
  const name = "React";
  const isLearning = true;

  return (
    <div>
      <h1>Learning {name}</h1>
      <p>{isLearning ? "You're doing great!" : "Keep trying!"}</p>
      <button onClick={() => alert("Hello!")}>Click me</button>
    </div>
  );
}
```

### JSX Rules

1. **Single Parent Element**: Wrap multiple elements in a `<div>` or React Fragment `<></>`
2. **JavaScript Expressions**: Use `{}` to embed JavaScript
3. **camelCase Attributes**: `className` instead of `class`, `onClick` instead of `onclick`
4. **Self-Closing Tags**: `<img />`, `<br />`, `<hr />`

### Example: User Card Component

```jsx
function UserCard() {
  const user = {
    name: "John Doe",
    age: 25,
    location: "New York",
  };

  return (
    <div className="user-card">
      <h2>{user.name}</h2>
      <p>Age: {user.age}</p>
      <p>Location: {user.location}</p>
    </div>
  );
}
```

---

## 3. Props (Properties)

### What are Props?

Props are how you pass data from parent components to child components. Think of them as function parameters.

### Basic Props Usage

```jsx
// Child component that receives props
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Parent component that passes props
function App() {
  return (
    <div>
      <Greeting name="Alice" />
      <Greeting name="Bob" />
      <Greeting name="Charlie" />
    </div>
  );
}
```

### Props Destructuring (Cleaner Syntax)

```jsx
// Instead of props.name, props.age
function UserProfile({ name, age, email }) {
  return (
    <div className="profile">
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
    </div>
  );
}

// Usage
function App() {
  return <UserProfile name="Sarah Wilson" age={28} email="sarah@example.com" />;
}
```

### Different Types of Props

```jsx
function ProductCard({
  title, // string
  price, // number
  isOnSale, // boolean
  tags, // array
  onBuy, // function
}) {
  return (
    <div className="product">
      <h3>{title}</h3>
      <p>${price}</p>
      {isOnSale && <span className="sale-badge">ON SALE!</span>}
      <div>
        {tags.map((tag, index) => (
          <span key={index} className="tag">
            {tag}
          </span>
        ))}
      </div>
      <button onClick={onBuy}>Buy Now</button>
    </div>
  );
}

// Usage
function App() {
  const handlePurchase = () => {
    alert("Added to cart!");
  };

  return (
    <ProductCard
      title="Wireless Headphones"
      price={99.99}
      isOnSale={true}
      tags={["electronics", "audio", "wireless"]}
      onBuy={handlePurchase}
    />
  );
}
```

### Default Props

```jsx
function Button({ text = "Click me", color = "blue", onClick }) {
  return (
    <button style={{ backgroundColor: color }} onClick={onClick}>
      {text}
    </button>
  );
}
```

---

## 4. Component Lifecycle & Hooks

### Introduction to Hooks

Hooks are functions that let you "hook into" React features. They always start with `use`.

### useState Hook - Managing State

```jsx
import { useState } from "react";

function Counter() {
  // Declare state variable with initial value
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

### Multiple State Variables

```jsx
function UserForm() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleSubmit = (e) => {
    e.preventDefault();
    setIsSubmitting(true);

    // Simulate API call
    setTimeout(() => {
      alert(`Hello ${name}! Email: ${email}`);
      setIsSubmitting(false);
      setName("");
      setEmail("");
    }, 1000);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Your name"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <input
        type="email"
        placeholder="Your email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Submitting..." : "Submit"}
      </button>
    </form>
  );
}
```

### useEffect Hook - Side Effects

```jsx
import { useState, useEffect } from "react";

function Timer() {
  const [seconds, setSeconds] = useState(0);
  const [isRunning, setIsRunning] = useState(false);

  // Effect runs after every render
  useEffect(() => {
    let interval = null;

    if (isRunning) {
      interval = setInterval(() => {
        setSeconds((seconds) => seconds + 1);
      }, 1000);
    }

    // Cleanup function (runs before next effect or unmount)
    return () => {
      if (interval) clearInterval(interval);
    };
  }, [isRunning]); // Dependency array - effect runs when isRunning changes

  return (
    <div>
      <h2>Timer: {seconds}s</h2>
      <button onClick={() => setIsRunning(!isRunning)}>
        {isRunning ? "Pause" : "Start"}
      </button>
      <button onClick={() => setSeconds(0)}>Reset</button>
    </div>
  );
}
```

### Common useEffect Patterns

#### 1. Component Did Mount (runs once)

```jsx
useEffect(() => {
  // Fetch data when component mounts
  fetchUserData();
}, []); // Empty dependency array = runs once
```

#### 2. Component Did Update (runs on specific changes)

```jsx
useEffect(() => {
  // Save to localStorage when count changes
  localStorage.setItem("count", count);
}, [count]); // Runs when count changes
```

#### 3. Component Will Unmount (cleanup)

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("Timer tick");
  }, 1000);

  return () => {
    clearInterval(timer); // Cleanup on unmount
  };
}, []);
```

---

## 5. Practical Examples

### Todo List Application

```jsx
import { useState } from "react";

function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState("");

  const addTodo = () => {
    if (inputValue.trim()) {
      setTodos([
        ...todos,
        {
          id: Date.now(),
          text: inputValue,
          completed: false,
        },
      ]);
      setInputValue("");
    }
  };

  const toggleTodo = (id) => {
    setTodos(
      todos.map((todo) =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };

  const deleteTodo = (id) => {
    setTodos(todos.filter((todo) => todo.id !== id));
  };

  return (
    <div className="todo-app">
      <h1>Todo List</h1>

      <div className="add-todo">
        <input
          type="text"
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          placeholder="Add a new todo..."
          onKeyPress={(e) => e.key === "Enter" && addTodo()}
        />
        <button onClick={addTodo}>Add</button>
      </div>

      <div className="todo-list">
        {todos.map((todo) => (
          <div key={todo.id} className="todo-item">
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            <span
              style={{
                textDecoration: todo.completed ? "line-through" : "none",
              }}
            >
              {todo.text}
            </span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </div>
        ))}
      </div>
    </div>
  );
}
```

---

## 6. Best Practices for Beginners

### 1. Component Organization

- Keep components small and focused
- One component per file
- Use descriptive names

### 2. Props Best Practices

- Destructure props for cleaner code
- Provide default values when needed
- Validate props with PropTypes (optional)

### 3. State Management

- Keep state as simple as possible
- Don't duplicate data in state
- Update state immutably

### 4. Common Patterns

```jsx
// Conditional rendering
{isLoggedIn ? <Dashboard /> : <Login />}

// List rendering
{items.map(item => <Item key={item.id} data={item} />)}

// Event handling
<button onClick={(e) => handleClick(e, item.id)}>
```

---

## 7. Next Steps

After mastering these concepts, explore:

1. **React Router** - Navigation between pages
2. **Context API** - Global state management
3. **Custom Hooks** - Reusable logic
4. **Testing** - Jest and React Testing Library
5. **Styling** - CSS Modules, Styled Components
6. **State Management** - Redux, Zustand

---

## Quick Reference

### Essential Hooks

- `useState()` - Manage component state
- `useEffect()` - Handle side effects
- `useContext()` - Access React context
- `useReducer()` - Complex state logic

### Common Patterns

```jsx
// State update
const [state, setState] = useState(initialValue);

// Effect with cleanup
useEffect(() => {
  // setup
  return () => {
    // cleanup
  };
}, [dependencies]);

// Event handler
const handleClick = (e) => {
  e.preventDefault();
  // handle event
};
```

---

## Resources for Further Learning

- **Official React Documentation**: [reactjs.org](https://reactjs.org)
- **React DevTools**: Browser extension for debugging
- **Create React App**: [create-react-app.dev](https://create-react-app.dev)
- **freeCodeCamp React Course**: Free comprehensive tutorial
