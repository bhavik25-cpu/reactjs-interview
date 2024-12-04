
https://github.com/sudheerj/reactjs-interview-questions

https://github.com/Devinterview-io/react-interview-questions

 1. create a counter child component in React and pass the counter value to the parent component:
```javascript
<!-- App.jsx -->
import React from 'react';
import ParentComponent from './components/ParentComponent';

const App = () => {
  return (
    <div>
     <h1>Counter App</h1>
      <ParentComponent />
    </div>
  );
};

export default App;|
```

```javascript
<!--  ParentComponent.js -->
import React, { useState } from 'react';
import ChildComponent from './ChildComponent';

const ParentComponent = () => {
  const [totalCount, setTotalCount] = useState(0);

  const handleCountChange = (newCount) => {
    setTotalCount(totalCount + newCount);
  };

  return (
    <div>
      <h1>Parent Component</h1>
      <p>Total Count: {totalCount}</p>
      <ChildComponent onCountChange={handleCountChange} />
    </div>
  );
};

export default ParentComponent;
```

```javascript
import React from 'react';

const ChildComponent = ({ onCountChange }) => {
  const handleButtonClick = () => {
    // Increase count by 1 and pass it to the parent component
    onCountChange(1);
  };

  return (
    <div>
      <button onClick={handleButtonClick}>Increase Count</button>
    </div>
  );
};

export default ChildComponent;

```
__________________________________________________________________________________________________________________


2.search arrays
```javascript
<!-- app.jsx -->
import React, { useState } from 'react';

function App() {
  const [searchTerm, setSearchTerm] = useState('');
  const [matchedName, setMatchedName] = useState('');
  const [error, setError] = useState('');

  const names = [
    'Jeff Bezos',
    'Bill Gates',
    'Warren Buffett',
    'Bernard Arnault',
    'Carlos Slim Helu',
    'Amancio Ortega',
    'Larry Ellison',
    'Mark Zuckerberg',
    'Michael Bloomberg',
    'Larry Page'
  ];

  const handleSearch = () => {
    const foundName = names.find(name =>
      name.toLowerCase().includes(searchTerm.toLowerCase())
    );
    if (foundName) { 
      setMatchedName(foundName);
      setError('');
    } else {
      setMatchedName('');
      setError('Name not found');
    }
  };

  return (
    <div>
      <input
        type="text"
        placeholder="Search name"
        value={searchTerm}
        onChange={e => setSearchTerm(e.target.value)}
      />
      <button onClick={handleSearch}>Search</button>
      {matchedName && <p>Matched Name: {matchedName}</p>}
      {error && <p>{error}</p>}
    </div>
  );
}

export default App;
```



__________________________________________________________________________________________________________________

3.counter
```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <h1>Counter</h1>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
}

export default Counter;
```

______________________________________________________________________________________________________________________________________________
Product Grid operations:
1. create frontend app.
2. create one table with multiple column like Title, Description, Price, 
Rating, Stock, Brand, Category.
3. Integrate this API (https://dummyjson.com/products) with table to 
render the data
4. We can allow user to do the filter and sorting for all column and 
pagination (all operation from front-end side)
5. Title field clickable and show the popup msg on title click ex: ( The 
product title is {Title} 


UserData.jsx

```javascript
// UserData.jsx
const UserData = ({ products }) => {
    return (
        <>
            {Array.isArray(products) && products.map((product) => {
                const { id, title, description, price, rating, brand, category } = product;
                return (
                    <tr key={id}>
                        <td>{id}</td>
                        <td>{title}</td>
                        <td>{description}</td>
                        <td>{price}</td>
                        <td>{rating}</td>
                        <td>{brand}</td>
                        <td>{category}</td>
                    </tr>
                );
            })}
        </>
    );
};

export default UserData;

```

App.jsx
```javascript
import { useEffect, useState } from "react";
import UserData from "./components/UserData.jsx";

const API = "https://dummyjson.com/products";

const App = () => {
    const [products, setProducts] = useState([]);
    const [filteredProducts, setFilteredProducts] = useState([]);
    const [filterText, setFilterText] = useState("");
    const [sortConfig, setSortConfig] = useState({ key: "", direction: "" });
    const [currentPage, setCurrentPage] = useState(1);
    const [itemsPerPage] = useState(10);
    const [popupMessage, setPopupMessage] = useState("");

    const fetchProducts = async (url) => {
        try {
            const res = await fetch(url);
            const data = await res.json();
            if (data && data.products && data.products.length > 0) {
                setProducts(data.products);
                setFilteredProducts(data.products);
            }
        } catch (error) {
            console.error("Error fetching products:", error);
        }
    };

    useEffect(() => {
        fetchProducts(API);
    }, []);

    const handleFilterChange = (e) => {
        setFilterText(e.target.value);
        const filteredData = products.filter((product) =>
            Object.values(product).some(
                (value) =>
                    typeof value === "string" &&
                    value.toLowerCase().includes(e.target.value.toLowerCase())
            )
        );
        setFilteredProducts(filteredData);
        setCurrentPage(1);
    };

    const handleSort = (key) => {
        let direction = "ascending";
        if (sortConfig.key === key && sortConfig.direction === "ascending") {
            direction = "descending";
        }
        setSortConfig({ key, direction });
    };

    const sortedProducts = () => {
        const sortableProducts = [...filteredProducts];
        if (sortConfig.key !== "") {
            sortableProducts.sort((a, b) => {
                if (a[sortConfig.key] < b[sortConfig.key]) {
                    return sortConfig.direction === "ascending" ? -1 : 1;
                }
                if (a[sortConfig.key] > b[sortConfig.key]) {
                    return sortConfig.direction === "ascending" ? 1 : -1;
                }
                return 0;
            });
        }
        return sortableProducts;
    };

    const indexOfLastItem = currentPage * itemsPerPage;
    const indexOfFirstItem = indexOfLastItem - itemsPerPage;
    const currentItems = sortedProducts().slice(indexOfFirstItem, indexOfLastItem);

    const paginate = (pageNumber) => {
        setCurrentPage(pageNumber);
    };

    const handleTitleClick = (title) => {
        setPopupMessage(`The product title is: ${title}`);
    };

    return (
        <>
            <div className="filter-container">
                <input
                    type="text"
                    placeholder="Filter products..."
                    value={filterText}
                    onChange={handleFilterChange}
                />
            </div>
            <div className="table-container">
                <table className="medium-table">
                    <thead>
                        <tr>
                            <th onClick={() => handleSort("id")}>ID</th>
                            <th onClick={() => handleSort("title")}>Title</th>
                            <th onClick={() => handleSort("description")}>Description</th>
                            <th onClick={() => handleSort("price")}>Price</th>
                            <th onClick={() => handleSort("rating")}>Rating</th>
                            <th onClick={() => handleSort("brand")}>Brand</th>
                            <th onClick={() => handleSort("category")}>Category</th>
                        </tr>
                    </thead>
                    <tbody>
                        {currentItems.map((item) => (
                            <tr key={item.id}>
                                <td>{item.id}</td>
                                <td onClick={() => handleTitleClick(item.title)}>{item.title}</td>
                                <td>{item.description}</td>
                                <td>{item.price}</td>
                                <td>{item.rating}</td>
                                <td>{item.brand}</td>
                                <td>{item.category}</td>
                            </tr>
                        ))}
                    </tbody>
                </table>
            </div>
            <div className="pagination-container">
                <Pagination
                    itemsPerPage={itemsPerPage}
                    totalItems={filteredProducts.length}
                    paginate={paginate}
                    currentPage={currentPage}
                />
            </div>
            {popupMessage && (
                <div className="popup">
                    <p>{popupMessage}</p>
                    <button onClick={() => setPopupMessage("")}>Close</button>
                </div>
            )}
        </>
    );
};

export default App;

const Pagination = ({ itemsPerPage, totalItems, paginate, currentPage }) => {
    const pageNumbers = [];

    for (let i = 1; i <= Math.ceil(totalItems / itemsPerPage); i++) {
        pageNumbers.push(i);
    }

    return (
        <nav>
            <ul className="pagination">
                {pageNumbers.map((number) => (
                    <li key={number} className="page-item">
                        <button
                            onClick={() => paginate(number)}
                            className={`page-link${currentPage === number ? " active" : ""}`}
                        >
                            {number}
                        </button>
                    </li>
                ))}
            </ul>
        </nav>
    );
};
```

index.css
```javascript

.filter-container {
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
}
.medium-table {
  width: 70%;
  margin: 0 auto;
}

.table-container {
  overflow-x: auto;
  max-height: 500px; /* Adjust height as needed */
}

.filter-container {
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
}

.pagination-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.pagination {
  display: flex;
  list-style: none;
  padding: 0;
}

.pagination li {
  margin-right: 5px;
}

.page-link {
  padding: 5px 10px;
  cursor: pointer;
  border: 1px solid #ccc;
  background-color: #fff;
}

.page-link.active {
  background-color: #007bff;
  color: #fff;
  border-color: #007bff;
}
.popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  padding: 20px;
  border: 1px solid #ccc;
  z-index: 999;
}

.popup p {
  margin-bottom: 10px;
}

```


![image](https://github.com/bhavik25-cpu/reactjs-interview/assets/82199211/b77e5fdc-ac42-4eb3-b167-52a35417c302)


____________________________________________________________________________________________________________________________________
https://jsonplaceholder.typicode.com/users

```javascript

import React, { useState, useEffect } from "react";

function UserList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((data) => setUsers(data))
      .catch((error) => console.error("Error fetching users:", error));
  }, []);

  return (
    <div>
      <h1>User List</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            <strong>{user.name}</strong> - {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;
```


_________________________________________________________________________________________________
https://dummy.restapiexample.com/api/v1/employees
employee_salary > 20000
To fetch data from the provided API and filter employees with a salary greater than 20000, you can modify the useEffect hook in your UserList component like this:
```javascript
import React, { useState, useEffect } from "react";

function UserList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://dummy.restapiexample.com/api/v1/employees")
      .then((response) => response.json())
      .then((data) => {
        // Filter employees with a salary greater than 20000
        const filteredUsers = data.data.filter((user) => user.employee_salary > 20000);
        setUsers(filteredUsers);
      })
      .catch((error) => console.error("Error fetching users:", error));
  }, []);

  return (
    <div>
      <h1>User List</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            <strong>{user.employee_name}</strong> - {user.employee_salary}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;

```
_____________________________________________________________________________________________

https://jsonplaceholder.typicode.com/posts
use this api and write a react js code  
title of the first char all 
sorted  
```javascript

import React, { useState, useEffect } from 'react';

function App() {
  const [sortedPosts, setSortedPosts] = useState([]);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => response.json())
      .then(data => {
        const sortedData = data.sort((a, b) => {
          const titleA = a.title.charAt(0).toLowerCase();
          const titleB = b.title.charAt(0).toLowerCase();
          if (titleA < titleB) return -1;
          if (titleA > titleB) return 1;
          return 0;
        });
        setSortedPosts(sortedData);
      })
      .catch(error => console.error('Error fetching data: ', error));
  }, []);

  return (
    <div>
      <h1>Sorted Posts in JSON Format</h1>
      <pre>{JSON.stringify(sortedPosts, null, 2)}</pre>
    </div>
  );
}

export default App;


```

__________________________________________________________________

Create a typeahead (searchable select) component in react. When searching, it should populate the results in a dropdown popup just below the input field. Use ApI https://api.jikan.moe/v4/anime?q={searchText} for populating results. Replace searchText with the input. It will return a list of anime episodes. Show title and image of the anime in the options. Once selected, the dropdown should close and selection should be displayed in the input field. Optimise  the API calls so that APIs are only fired when user stops typing.

```javascript

import React, { useState, useEffect } from 'react';

const Typeahead = () => {
  const [searchText, setSearchText] = useState('');
  const [results, setResults] = useState([]);
  const [selected, setSelected] = useState('');
  const [isOpen, setIsOpen] = useState(false);
  const [typingTimeout, setTypingTimeout] = useState(null);

  // Fetch anime from API
  const fetchAnime = async (query) => {
    if (query.trim() === '') {
      setResults([]);
      return;
    }
    const response = await fetch(`https://api.jikan.moe/v4/anime?q=${query}`);
    const data = await response.json();
    setResults(data.data || []);
  };

  // Debounced search function
  const handleSearch = (e) => {
    const input = e.target.value;
    setSearchText(input);
    setIsOpen(true);

    if (typingTimeout) {
      clearTimeout(typingTimeout);
    }

    setTypingTimeout(
      setTimeout(() => {
        fetchAnime(input);
      }, 500) // Debounce for 500ms
    );
  };

  // Handle selecting an option
  const handleSelect = (title) => {
    setSelected(title);
    setSearchText(title);
    setIsOpen(false);
  };

  return (
    <div>
      <input
        type="text"
        value={selected || searchText}
        onChange={handleSearch}
        placeholder="Search anime..."
      />
      {isOpen && results.length > 0 && (
        <ul style={{ border: '1px solid black', listStyle: 'none', padding: '0' }}>
          {results.map((anime) => (
            <li
              key={anime.mal_id}
              onClick={() => handleSelect(anime.title)}
              style={{ cursor: 'pointer', padding: '5px', display: 'flex', alignItems: 'center' }}
            >
              <img src={anime.images.jpg.image_url} alt={anime.title} width="50" />
              <span style={{ marginLeft: '10px' }}>{anime.title}</span>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
};

export default Typeahead;
```
________________________________________________________________________________

dropdown
```javascript

import React from 'react';

const App = () => {
  return (
    <div>
      <h1>Hard-Coded Dropdown Example</h1>
      <select>
        <option value="">Select an option</option>
        <option value="option1">Option 1</option>
        <option value="option2">Option 2</option>
        <option value="option3">Option 3</option>
      </select>
    </div>
  );
};

export default App;
```




_____________________________________________________________________________
list of items with delete
```javascript

import React, { useState } from 'react';

const TodoApp = () => {
  const [items, setItems] = useState([]);
  const [input, setInput] = useState('');

  const addItem = () => {
    if (input.trim()) setItems([...items, input]);
    setInput('');
  };

  const deleteItem = (index) => setItems(items.filter((_, i) => i !== index));

  return (
    <div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <button onClick={addItem}>Add</button>
      <ul>
        {items.map((item, i) => (
          <li key={i} onClick={() => deleteItem(i)}>{item}</li>
        ))}
      </ul>
    </div>
  );
};

export default TodoApp;

```


______________________________________________________________

Imagine you're building a countdown timer component in React application. How would y design this component to accura count down from a specified tim update the UI in real-time, and trigger specific actions when the timer reaches zero? Describe the component structure and any sta management techniques
```javascript

import React, { useState, useEffect } from 'react';

const CountdownTimer = ({ initialTime = 60, onComplete }) => {
  const [timeLeft, setTimeLeft] = useState(initialTime);
  const [isActive, setIsActive] = useState(false);

  useEffect(() => {
    if (isNaN(initialTime) || initialTime < 0) setTimeLeft(60); // fallback if initialTime is invalid

    let interval;
    if (isActive && timeLeft > 0) {
      interval = setInterval(() => setTimeLeft((prev) => prev - 1), 1000);
    } else if (timeLeft === 0) {
      clearInterval(interval);
      onComplete && onComplete();
    }
    return () => clearInterval(interval);
  }, [isActive, timeLeft, onComplete, initialTime]);

  const toggleTimer = () => setIsActive(!isActive);
  const resetTimer = () => { setTimeLeft(initialTime); setIsActive(false); };
  
  const formatTime = () => 
    `${String(Math.floor(timeLeft / 60)).padStart(2, '0')}:${String(timeLeft % 60).padStart(2, '0')}`;

  return (
    <div>
      <h2>{formatTime()}</h2>
      <button onClick={toggleTimer}>{isActive ? 'Pause' : 'Start'}</button>
      <button onClick={resetTimer}>Reset</button>
    </div>
  );
};

export default CountdownTimer;
```

________________________________________________________________


React project where you're managing a list of tasks with functionality to add, toggle, and delete tasks
```javascript

import React, { useState } from 'react';

const App = () => {
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState("");

  const handleNewTaskChange = (event) => {
    setNewTask(event.target.value);
  };

  const handleAddTask = () => {
    if (newTask.trim() !== "") {
      const newTaskObj = {
        id: Date.now(),
        text: newTask,
        completed: false
      };
      setTasks([...tasks, newTaskObj]);
      setNewTask(""); // Clear input field
    }
  };

  const handleToggleTask = (taskId) => {
    setTasks(tasks.map((task) =>
      task.id === taskId ? { ...task, completed: !task.completed } : task
    ));
  };

  const handleDeleteTask = (taskId) => {
    setTasks(tasks.filter((task) => task.id !== taskId));
  };

  return (
    <div className="App">
      <input
        type="text"
        value={newTask}
        onChange={handleNewTaskChange}
        placeholder="Add a new task"
      />
      <button onClick={handleAddTask}>Add Task</button>

      {tasks.length === 0 ? (
        <p>No tasks to display.</p>
      ) : (
        <ul>
          {tasks.map((task) => (
            <li key={task.id} className="flex-container">
              <div className="flex-item" style={{ textDecoration: task.completed ? "line-through" : "none" }}>
                {task.text}
              </div>
              <button
                className="flex-item1"
                onClick={() => handleToggleTask(task.id)}
              >
                {task.completed ? "Mark Incomplete" : "Mark Complete"}
              </button>
              <button
                className="flex-item1"
                onClick={() => handleDeleteTask(task.id)}
              >
                Delete
              </button>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
};

export default App;

```
______________________________________________________________________

// Print the title of each task
```javascript

const tasks = [
  {
    title: "Complete the React tutorial",
    body: "Learn about components, state, and props.",
  },
  {
    title: "Finish the JavaScript project",
    body: "Implement the remaining features and fix any bugs.",
  },
  {
    title: "Read a book",
    body: "Choose a book from your reading list and start reading.",
  },
  {
    title: "Go for a walk",
    body: "Take a break from work and enjoy some fresh air.",
  },
  {
    title: "Learn a new skill",
    body: "Explore a new hobby or learn a new programming language.",
  },
];

// Print the title of each task
tasks.forEach((task) => {
  console.log(task.title);
});

```

