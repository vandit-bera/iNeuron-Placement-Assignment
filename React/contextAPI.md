# Context APi Minor Project

```
ThemeContext.js File


import React, { createContext, useState } from 'react';

// Create a new context
export const ThemeContext = createContext();

// Create a provider component
export const ThemeProvider = ({ children }) => {
  // Define the state
  const [theme, setTheme] = useState('light');

  // Function to toggle the theme
  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };

  // Provide the state and the toggle function to the child components
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

```

```
App.js File


import React from 'react';
import Dashboard from './Dashboard';
import { ThemeProvider } from './ThemeContext';

function App() {
  return (
    <ThemeProvider>
      <Dashboard />
    </ThemeProvider>
  );
}

export default App;

```

```
Dashboard.js File


import React, { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

const Dashboard = () => {
  // Consume the context
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <div className={`dashboard ${theme}`}>
      <h1>Dashboard</h1>
      <button onClick={toggleTheme}>
        Toggle Theme
      </button>
    </div>
  );
};

export default Dashboard;

```

```
App.css File


.dashboard {
  padding: 20px;
  background-color: #f5f5f5;
}

.dashboard.dark {
  background-color: #333;
  color: #fff;
}

```

```
index.js File


import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(<App />, document.getElementById('root'));

```

> Start the development server using a command like npm start, and you should see the dashboard with the toggle button. Clicking the button will switch the theme between light and dark.

> In this project, the ThemeContext is created using the createContext function. The ThemeProvider component provides the theme state and toggleTheme function to the child components. The Dashboard component consumes the context using the useContext hook and renders the toggle button. Clicking the button triggers the toggleTheme function, which updates the theme state and triggers a re-render, causing the styles to be updated based on the selected theme.