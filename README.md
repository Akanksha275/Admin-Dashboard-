# Admin-Dashboard-
Creating a Admin Dashboard for business use 
Creating a full-featured admin dashboard with backend code and a database server is a complex task that typically requires multiple files, technologies, and significant development time. Below, I'll provide you with a simplified example using Node.js, Express.js for the backend, and MongoDB as the database. This example focuses on managing products in the admin dashboard. To run this example, you need to have Node.js and MongoDB installed.

**1. Install Dependencies:**

Before starting, make sure you have Node.js and MongoDB installed. You can install Node.js from [nodejs.org](https://nodejs.org/) and MongoDB from [mongodb.com](https://www.mongodb.com/try/download/community).

**2. Create a new directory for your project and navigate to it in your terminal.**

**3. Set up Your Project:**

Run the following commands in your project directory:

```bash
npm init -y
npm install express mongoose body-parser
```

**4. Create the Backend:**

Create a file named `server.js` for your backend code:

```javascript
// server.js
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();

// Connect to MongoDB
mongoose.connect('mongodb://localhost/admin-dashboard', { useNewUrlParser: true, useUnifiedTopology: true })
    .then(() => console.log('Connected to MongoDB'))
    .catch(err => console.error('Failed to connect to MongoDB', err));

// Create a product schema
const productSchema = new mongoose.Schema({
    name: String,
    price: Number,
});

const Product = mongoose.model('Product', productSchema);

app.use(bodyParser.json());

// Define API routes

// Get all products
app.get('/api/products', async (req, res) => {
    const products = await Product.find();
    res.send(products);
});

// Create a new product
app.post('/api/products', async (req, res) => {
    const { name, price } = req.body;
    const product = new Product({ name, price });
    await product.save();
    res.send(product);
});

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});
```

**5. Create the Frontend:**

Create an `index.html` file for your frontend:

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Admin Dashboard</title>
</head>
<body>
    <header>
        <h1>Admin Dashboard</h1>
    </header>
    <main>
        <section id="products">
            <h2>Manage Products</h2>
            <ul id="productList"></ul>
            <form id="addProductForm">
                <input type="text" id="productName" placeholder="Product Name">
                <input type="number" id="productPrice" placeholder="Product Price">
                <button type="submit">Add Product</button>
            </form>
        </section>
    </main>
    <script src="script.js"></script>
</body>
</html>
```

**6. Create the Frontend JavaScript:**

Create a `script.js` file for your frontend JavaScript:

```javascript
// script.js
document.addEventListener('DOMContentLoaded', async () => {
    const productList = document.getElementById('productList');
    const addProductForm = document.getElementById('addProductForm');
    const productNameInput = document.getElementById('productName');
    const productPriceInput = document.getElementById('productPrice');

    // Function to fetch and display products
    async function displayProducts() {
        const response = await fetch('/api/products');
        const products = await response.json();
        productList.innerHTML = '';
        products.forEach(product => {
            const listItem = document.createElement('li');
            listItem.innerHTML = `${product.name} - $${product.price}`;
            productList.appendChild(listItem);
        });
    }

    // Event listener to add a new product
    addProductForm.addEventListener('submit', async (e) => {
        e.preventDefault();
        const name = productNameInput.value;
        const price = parseFloat(productPriceInput.value);
        if (name && !isNaN(price)) {
            const response = await fetch('/api/products', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ name, price }),
            });
            if (response.status === 200) {
                productNameInput.value = '';
                productPriceInput.value = '';
                displayProducts();
            }
        }
    });

    // Initial product display
    displayProducts();
});
```

**7. Start the Application:**

Run the following command in your terminal to start the application:

```bash
node server.js
```

admin dashboard will be accessible at `http://localhost:3000`. 

