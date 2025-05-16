<h1>
  <span class="headline">Selenium: Waits, Alerts, and Dynamic UIs</span>
  <span class="subhead">Setup</span>
</h1>

### 1: Create Your Project Folder

In this lesson, you’ll start fresh with a new project just for testing waits, alerts and dynamic uis.

Open a terminal and run:

```bash
mkdir selenium-waits-alerts-and-dynamic-uis
cd selenium-waits-alerts-and-dynamic-uis
```

### 2: Set Up a Virtual Environment

Create and activate a virtual environment inside this new folder:

```bash
python -m venv venv
```

Activate it:

**macOS/Linux:**

```bash
source venv/bin/activate
```

**Windows:**

```bash
venv\Scripts\activate
```

You should now see (venv) at the start of your terminal prompt.

### 3: Install Selenium

With the virtual environment active, install Selenium using pip:

```bash
pip install selenium
```

Check that it installed correctly:

```bash
pip show selenium
```

> ✅ If Selenium installed correctly, you’ll see output with its version and location.

### 4: Create Your Project Files

Inside your `selenium-waits-alerts-and-dynamic-uis` folder, create the following files:

- `index.html`
- `styles.css`
- `script.js`
- `test_dynamic_uis.py`

## 5. Add your starter code

For this lesson we will be using a local html file as a mock site to test our automations.

### Add the following starter code to `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>SimpleShop - Tech Deals</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <header>
      <h1>SimpleShop</h1>
      <p>Your global source for quality gadgets</p>
    </header>

    <main>
      <!-- Login Section -->
      <section class="login-section">
        <h2>Login</h2>
        <form id="login-form">
          <input id="username" type="text" placeholder="Username" required />
          <input
            id="password"
            type="password"
            placeholder="Password"
            required
          />
          <button type="submit">Log In</button>
        </form>
        <div id="spinner" class="spinner hidden"></div>
        <div id="login-success" class="hidden">Login successful!</div>
      </section>

      <!-- Offers Section -->
      <section class="product-section">
        <h2>Today's Deals</h2>
        <button id="load-offers">Load Offers</button>
        <div id="offer-spinner" class="spinner hidden"></div>
        <div class="promo-grid">
          <div class="promo-card hidden">Wireless Mouse - $24.99</div>
          <div class="promo-card hidden">Bluetooth Speaker - $49.99</div>
        </div>
      </section>

      <!-- Toast Notification -->
      <section id="toast" class="toast hidden">Welcome back!</section>

      <!-- Product Card -->
      <section class="product-section">
        <h2>Featured Items</h2>
        <div class="product-card">
          <h3>Portable Solar Power Bank</h3>
          <p>$39.99</p>
        </div>
        <div class="product-card">
          <h3>Wireless Earbuds with Touch Controls</h3>
          <p>$59.99</p>
        </div>
        <div class="product-card">
          <h3>Smart LED Light Strip (USB Powered)</h3>
          <p>$24.99</p>
        </div>
      </section>

      <!-- Checkout Button (Delayed) -->
      <section class="checkout-section">
        <button id="checkout-btn" class="hidden">Checkout</button>
      </section>

      <!-- Welcome Modal -->
      <section class="modal hidden" id="welcome-modal">
        <div class="modal-content">
          <p><strong>Welcome back to SimpleShop!</strong></p>
          <p>Don’t miss our 10% off sale on tech gadgets this week.</p>
          <button id="dismiss-welcome">Start Shopping</button>
        </div>
      </section>

      <!-- JavaScript Dialog Buttons -->
      <section class="product-section">
        <h2>Account Settings</h2>
        <button id="clear-cart">Clear Cart</button>
        <button id="delete-item">Delete Item</button>
        <button id="request-email">Request Email</button>
      </section>
    </main>
  </body>
  <script src="script.js"></script>
</html>
```

### Add the following starter code to `styles.css`:

```css
body {
  font-family: 'Segoe UI', sans-serif;
  background-color: #121212;
  color: #f0f0f0;
  margin: 0;
  padding: 0;
  line-height: 1.5;
}

header {
  background-color: #1f1f1f;
  padding: 1rem;
  text-align: center;
  border-bottom: 1px solid #333;
}

h1 {
  margin: 0;
  font-size: 2rem;
}

main {
  padding: 2rem;
}

input,
button {
  font-size: 1rem;
  border-radius: 4px;
  border: none;
}

input {
  width: 100%;
  padding: 0.6rem;
  margin: 0.5rem 0;
  background-color: #2c2c2c;
  color: white;
}

button {
  padding: 0.6rem 1.2rem;
  margin-top: 1rem;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.hidden {
  display: none !important;
}

.spinner {
  width: 32px;
  height: 32px;
  border: 4px solid #444;
  border-top: 4px solid #fff;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 20px auto;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.toast {
  position: fixed;
  top: 1rem;
  right: 1rem;
  background: #28a745;
  color: white;
  padding: 1rem;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.4);
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.modal-content {
  background: #1e1e1e;
  padding: 2rem;
  border-radius: 6px;
  color: white;
  text-align: center;
  box-shadow: 0 0 20px rgba(255, 255, 255, 0.05);
}

.product-card,
.promo-card {
  background-color: #1e1e1e;
  padding: 1.5rem;
  margin: 1rem 0;
  border-radius: 6px;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.05);
}

.promo-grid {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 1.5rem;
}
```

### Add the following starter code to `script.js`:

```js
document.addEventListener('DOMContentLoaded', () => {
  // Welcome Modal
  const modal = document.getElementById('welcome-modal');
  const dismissBtn = document.getElementById('dismiss-welcome');
  setTimeout(() => {
    modal.classList.remove('hidden');
  }, 3000);
  dismissBtn.addEventListener('click', () => {
    modal.classList.add('hidden');
  });

  // Login + Toast
  const loginForm = document.getElementById('login-form');
  loginForm.addEventListener('submit', (e) => {
    e.preventDefault();
    document.getElementById('spinner').classList.remove('hidden');
    setTimeout(() => {
      document.getElementById('spinner').classList.add('hidden');
      document.getElementById('login-success').classList.remove('hidden');
      document.getElementById('toast').classList.remove('hidden');
      setTimeout(() => {
        document.getElementById('toast').classList.add('hidden');
      }, 3000);
    }, 2000);
  });

  // Load Offers
  const loadBtn = document.getElementById('load-offers');
  const offerSpinner = document.getElementById('offer-spinner');
  const promoCards = document.querySelectorAll('.promo-card');
  loadBtn.addEventListener('click', () => {
    offerSpinner.classList.remove('hidden');
    setTimeout(() => {
      offerSpinner.classList.add('hidden');
      promoCards.forEach((card) => card.classList.remove('hidden'));
    }, 3000);
  });

  // Checkout
  setTimeout(() => {
    document.getElementById('checkout-btn').classList.remove('hidden');
  }, 3000);

  // JavaScript Dialogs
  document.getElementById('clear-cart').addEventListener('click', () => {
    alert('Are you sure you want to clear your cart?');
  });

  document.getElementById('delete-item').addEventListener('click', () => {
    const confirmed = confirm('Are you sure you want to delete this item?');
    console.log(confirmed ? 'Item deleted.' : 'Canceled.');
  });

  document.getElementById('request-email').addEventListener('click', () => {
    const email = prompt('Enter your email to continue:');
    console.log(email);
  });
});
```

### Add the following starter code to `test_dynamic_uis.py`:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service

# Replace with your actual path to chromedriver
chromedriver_path = "/Users/username/Downloads/chromedriver"
service = Service(executable_path=chromedriver_path)
driver = webdriver.Chrome(service=service)

# Replace with the full path to your index.html file
# Example (macOS): "file:///Users/yourname/Desktop/selenium-interacting-with-elements/index.html"
# Example (Windows): "file:///C:/Users/yourname/Desktop/selenium-interacting-with-elements/index.html"
driver.get("file:///absolute/path/to/index.html")


# Automate browser actions here

driver.quit()
```

> 💡 Tip: If ChromeDriver is already in your system PATH, you can skip the `Service` setup and just write:
>
> ```python
> driver = webdriver.Chrome()
> ```

## Setup Complete!

Once everything is set up, you should be able to:

- Open `index.html` in Chrome
- Run `test_dynamic_uis.py`
- See a browser open and then close with no errors

If you do, you're ready to move on!
