Task 1: Todo List App
--------------------- 
1. Task Description
Create a complete Todo List application with all essential CRUD operations.

2. Short Description
Build a React application that allows users to add, view, mark as complete, and delete todo items. Include filtering functionality to show all, active, or completed todos.

3. Solution

```
import React, { useState } from 'react';

function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState('');
  const [filter, setFilter] = useState('all'); // all, active, completed

  const addTodo = () => {
    if (inputValue.trim() !== '') {
      setTodos([
        ...todos,
        {
          id: Date.now(),
          text: inputValue,
          completed: false
        }
      ]);
      setInputValue('');
    }
  };

  const toggleTodo = (id) => {
    setTodos(
      todos.map(todo =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };

  const deleteTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  const filteredTodos = todos.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });

  const completedCount = todos.filter(todo => todo.completed).length;
  const activeCount = todos.length - completedCount;

  return (
    <div style={{ maxWidth: '500px', margin: '0 auto', padding: '20px' }}>
      <h1>Todo List</h1>
      
      {/* Add Todo Form */}
      <div style={{ display: 'flex', marginBottom: '20px' }}>
        <input
          type="text"
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          onKeyPress={(e) => e.key === 'Enter' && addTodo()}
          placeholder="Add a new todo..."
          style={{ flex: 1, padding: '10px', marginRight: '10px' }}
        />
        <button onClick={addTodo} style={{ padding: '10px 20px' }}>
          Add
        </button>
      </div>

      {/* Filter Buttons */}
      <div style={{ marginBottom: '20px' }}>
        <button 
          onClick={() => setFilter('all')}
          style={{ marginRight: '10px', padding: '5px 10px' }}
        >
          All ({todos.length})
        </button>
        <button 
          onClick={() => setFilter('active')}
          style={{ marginRight: '10px', padding: '5px 10px' }}
        >
          Active ({activeCount})
        </button>
        <button 
          onClick={() => setFilter('completed')}
          style={{ padding: '5px 10px' }}
        >
          Completed ({completedCount})
        </button>
      </div>

      {/* Todo List */}
      <ul style={{ listStyle: 'none', padding: 0 }}>
        {filteredTodos.map(todo => (
          <li 
            key={todo.id}
            style={{
              display: 'flex',
              justifyContent: 'space-between',
              alignItems: 'center',
              padding: '10px',
              borderBottom: '1px solid #eee',
              textDecoration: todo.completed ? 'line-through' : 'none'
            }}
          >
            <div>
              <input
                type="checkbox"
                checked={todo.completed}
                onChange={() => toggleTodo(todo.id)}
                style={{ marginRight: '10px' }}
              />
              <span>{todo.text}</span>
            </div>
            <button 
              onClick={() => deleteTodo(todo.id)}
              style={{ 
                padding: '5px 10px', 
                backgroundColor: '#ff4444', 
                color: 'white', 
                border: 'none', 
                borderRadius: '3px',
                cursor: 'pointer'
              }}
            >
              Delete
            </button>
          </li>
        ))}
      </ul>

      {todos.length === 0 && (
        <p style={{ textAlign: 'center', color: '#666' }}>
          No todos yet. Add some tasks!
        </p>
      )}
    </div>
  );
}

export default TodoApp;
```
