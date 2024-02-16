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
<!-- ParentComponent.js -->
import React, { useState } from 'react';
import ChildComponent from './ChildComponent';

const ParentComponent = () => {
  const [counterFromChild, setCounterFromChild] = useState(0);

  const handleCounterChange = (newCounterValue) => {
    setCounterFromChild(newCounterValue);
  };

  return (
    <div>
      <h1>Parent Component</h1>
      <h2>Counter from Child: {counterFromChild}</h2>
      <ChildComponent onCounterChange={handleCounterChange} />
    </div>
  );
};

export default ParentComponent;
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
