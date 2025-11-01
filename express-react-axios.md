# Connecting React Frontend to Express API Using Axios
## Objective
Learn how to connect a React frontend application to a backend Express.js API using Axios to fetch data. This task helps you understand how to integrate frontend and backend parts of a full stack application and handle HTTP requests in React.
## Task Description
Create a simple Express.js API that returns a list of products (each with a name and price). Then, build a React frontend application with a component that uses Axios to fetch the product data from the Express API when the component mounts. Display the list of products in a user-friendly layout, showing their name and price. Ensure you handle loading states and possible errors when fetching data. Test the connection by running both the frontend and backend locally and verifying that the React app correctly displays data fetched from the Express API.
## Expected Output
See: https://s3.ap-south-1.amazonaws.com/static.bytexl.app/uploads/42vxd5kz7/content/43qjzj4cx/9.png

## Example Code

### server.js (Express Backend)
```javascript
// Import required modules
const express = require('express');
const cors = require('cors');

// Initialize Express app
const app = express();
const PORT = 5000;

// Enable CORS to allow requests from React frontend
app.use(cors());
app.use(express.json());

// Sample product data
const products = [
  { id: 1, name: 'Laptop', price: 999.99 },
  { id: 2, name: 'Smartphone', price: 699.99 },
  { id: 3, name: 'Headphones', price: 149.99 },
  { id: 4, name: 'Keyboard', price: 79.99 },
  { id: 5, name: 'Mouse', price: 49.99 }
];

// API endpoint to get all products
app.get('/api/products', (req, res) => {
  res.json(products);
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

### ProductList.js (React Frontend Component)
```javascript
// Import required modules
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import './ProductList.css';

const ProductList = () => {
  // State management for products, loading, and errors
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Fetch products when component mounts
  useEffect(() => {
    // Function to fetch products from API
    const fetchProducts = async () => {
      try {
        setLoading(true);
        // Make GET request to Express API
        const response = await axios.get('http://localhost:5000/api/products');
        setProducts(response.data);
        setError(null);
      } catch (err) {
        // Handle errors
        setError('Failed to fetch products. Please try again later.');
        console.error('Error fetching products:', err);
      } finally {
        setLoading(false);
      }
    };

    fetchProducts();
  }, []); // Empty dependency array means this runs once on mount

  // Render loading state
  if (loading) {
    return <div className="loading">Loading products...</div>;
  }

  // Render error state
  if (error) {
    return <div className="error">{error}</div>;
  }

  // Render products list
  return (
    <div className="product-list">
      <h1>Product List</h1>
      <div className="products-grid">
        {products.map((product) => (
          <div key={product.id} className="product-card">
            <h3>{product.name}</h3>
            <p className="price">${product.price.toFixed(2)}</p>
          </div>
        ))}
      </div>
    </div>
  );
};

export default ProductList;
```

### App.js (Using ProductList Component)
```javascript
// Import required modules
import React from 'react';
import ProductList from './components/ProductList';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>Express-React Product Store</h1>
      </header>
      <main>
        {/* Render the ProductList component */}
        <ProductList />
      </main>
    </div>
  );
}

export default App;
```
