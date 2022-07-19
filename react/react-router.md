# React Router



## 基本使用

```jsx
import { Route, Routes } from 'react-router-dom';

function App() {
  return (
    <div className="App">
      <Routes>
        <Route path='/' element={<Welcome/>} />
        <Route path='/todo' element={ <Todo /> } />
      </Routes>
    </div>
  );
}
```



## 