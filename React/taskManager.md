# TASK MANAGER APP  

```
App.js


import React, { useState, useEffect } from 'react';
import { BrowserRouter as Router, Switch, Route, Redirect } from 'react-router-dom';
import axios from 'axios';

const App = () => {
  const [loggedIn, setLoggedIn] = useState(false);
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState('');

  useEffect(() => {
    // Check if user is logged in
    const token = localStorage.getItem('token');
    if (token) {
      setLoggedIn(true);
      fetchTasks(token);
    }
  }, []);

  const handleLogin = async () => {
    try {
      const response = await axios.post('https://reqres.in/api/login', {
        email: 'eve.holt@reqres.in',
        password: 'cityslicka',
      });
      const { token } = response.data;
      localStorage.setItem('token', token);
      setLoggedIn(true);
      fetchTasks(token);
    } catch (error) {
      console.log('Login error:', error);
    }
  };

  const handleLogout = () => {
    localStorage.removeItem('token');
    setLoggedIn(false);
    setTasks([]);
  };

  const fetchTasks = async (token) => {
    try {
      const response = await axios.get('https://reqres.in/api/tasks', {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      });
      setTasks(response.data);
    } catch (error) {
      console.log('Fetch tasks error:', error);
    }
  };

  const handleCreateTask = async () => {
    try {
      const token = localStorage.getItem('token');
      const response = await axios.post(
        'https://reqres.in/api/tasks',
        { task: newTask },
        {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        }
      );
      const newTaskData = response.data;
      setTasks([...tasks, newTaskData]);
      setNewTask('');
    } catch (error) {
      console.log('Create task error:', error);
    }
  };

  return (
    <Router>
      <Switch>
        <Route exact path="/">
          {loggedIn ? <Redirect to="/dashboard" /> : <button onClick={handleLogin}>Login</button>}
        </Route>
        <Route exact path="/dashboard">
          {loggedIn ? (
            <div>
              <h1>Task Dashboard</h1>
              <button onClick={handleLogout}>Logout</button>
              <h2>Create Task</h2>
              <input
                type="text"
                value={newTask}
                onChange={(e) => setNewTask(e.target.value)}
              />
              <button onClick={handleCreateTask}>Add Task</button>
              <h2>Task List</h2>
              <ul>
                {tasks.map((task) => (
                  <li key={task.id}>{task.task}</li>
                ))}
              </ul>
            </div>
          ) : (
            <Redirect to="/" />
          )}
        </Route>
      </Switch>
    </Router>
  );
};

export default App;
```

```
index.css


body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  margin-bottom: 1rem;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 0.5rem;
}
```

```
index.js


import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```
