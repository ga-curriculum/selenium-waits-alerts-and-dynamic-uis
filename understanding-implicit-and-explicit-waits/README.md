<h1>
  <span class="headline">Selenium: Waits, Alerts, and Dynamic UIs</span>
  <span class="subhead">Understanding Implicit and Explicit Waits</span>
</h1>

**Learning objective:** Explain the difference between implicit and explicit waits in Selenium and when to use each.

## Why synchronization matters in real-world browser automation

When you first start automating tests with Selenium, everything can feel straightforward. You write a script to click a button or fill a form, and it "just works." But as you interact with more complex, dynamic web applications, things may begin to surprise you:

- The **"Submit"** button does not show up until after new data is loaded from the server.
- An animated menu needs time to slide onto the screen before you can interact with it.
- A confirmation message appears a few seconds after you finish a task, rather than immediately.

If Selenium tries to interact with any of these elements before they are available, you get errors like _Element Not Found_.

Modern websites often load, display, or update parts of the page dynamically. To create reliable Selenium scripts, you'll use **waits** to help your code pause, check, and only act when the page is actually ready.

## Two ways Selenium waits: `implicit wait` vs `explicit wait`

Selenium gives you two main tools for synchronizing automation with page behavior:

- **Implicit wait**
- **Explicit wait**

Let's explore each one—and learn when to use which.

## What is an `implicit` wait?

An _implicit wait_ is a default timeout you set for _all_ element searches in your Selenium session.

Once activated, Selenium will keep trying to find any element for up to the time limit you specify, polling the page at short intervals. This way, you don't need to worry if an element appears a bit later than expected—Selenium will keep looking until either the element shows up or the time runs out.

> 📚 The _Document Object Model (DOM)_ is the tree structure of objects that make up a web page. Selenium searches for elements within the DOM.

### Example: Setting an implicit wait

Let's see how to add an implicit wait in Python using Selenium:

```python
from selenium import webdriver

driver = webdriver.Chrome()
driver.implicitly_wait(10) # Wait up to 10 seconds for any element to appear
driver.get('https://example.com')

# Now every find_element will wait up to 10 seconds by default

search_box = driver.find_element('css selector', '#search')
search_box.send_keys('Selenium waits')
```

If the element shows up within 10 seconds, Selenium will find it and continue. If it doesn't, your code will raise an error.

> 🏆 **Best practice:** Use implicit waits when you want a quick, global solution for slow-loading elements—but only if most elements on your page appear in about the same amount of time.

### Implicit wait in action: scenario

- You are testing an internal dashboard that usually loads all its basic data within 3–5 seconds.
- You set an implicit wait for 5 seconds. Now, every element lookup in your script is a little more robust to short delays.

But what if some page features need _much more_ time to appear, or specific actions require unique timing? This is where **explicit waits** help.

## What is an `explicit` wait?

An _explicit wait_ lets you pause your script until a particular _condition_ is true for a specific element. Conditions can include:

- The element is present in the DOM.
- The element is visible on the page.
- The element is clickable (not just present, but ready for interaction).

You choose what to wait for, and for how long.

### Example: Using an explicit wait

Here's how to wait (up to 15 seconds) until a button with the id _submit_ is clickable:

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
driver.get('https://example.com')

wait = WebDriverWait(driver, 15)
submit_button = wait.until(
EC.element_to_be_clickable((By.CSS_SELECTOR, '#submit'))
)
submit_button.click()
```

If the button appears and is ready after 4 seconds, your script continues at that point. The wait ends as soon as the criteria are met—saving time and making your tests faster and more reliable.

### When to use explicit waits

- You want different wait times for different steps.
- You need to hold off until a particular animation, dialog, or data element is ready.
- Parts of the page load at unpredictable speeds, depending on user actions or network delays.

## Comparing implicit and explicit waits: Which should you use?

| Feature           | Implicit wait                   | Explicit wait                     |
| ----------------- | ------------------------------- | --------------------------------- |
| **Scope**         | All element lookups             | Specific actions/conditions       |
| **Customization** | One global time limit           | Custom time and condition         |
| **Flexibility**   | Waits for presence only         | Wait for clickable, visible, etc. |
| **Syntax**        | Simple, single method           | Requires more code                |
| **When to use**   | For similar, predictable delays | For unique/dynamic behaviors      |

> ⚠️ **Do not mix implicit and explicit waits.** Combining them in one script often causes unpredictable results and makes debugging harder. Choose one strategy per context.

<div class="activity guided-walkthrough">
  <h2 class="title">Try Both Implicit and Explicit Waits with a Real Modal</h2>
  <span class="minutes">10 min</span>
</div>

### Scenario

We will use the **SimpleShop** mock site to demonstrate interacting with a **welcome modal** that appears automatically **3 seconds after the page loads**. Your goal is to write two versions of a Selenium script:

1. One using an **implicit wait**
2. One using an **explicit wait**

...and compare which one is more reliable and when.

#### 1. **Test with an implicit wait**

- Add an implicit wait of 2 seconds.
- Immediately try to find and click the modal’s dismiss button (`Start Shopping`), which may or may not be visible in time.

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

driver = webdriver.Chrome()
driver.implicitly_wait(2)  # Global wait
driver.get("file:///Users/username/path/to/example_file.html")  # Replace with the full absolute path to your local HTML file

# Try to click the dismiss button as soon as the modal may appear
dismiss_button = driver.find_element(By.ID, "dismiss-welcome")
dismiss_button.click()

print("Modal dismissed using implicit wait.")
driver.quit()
```

**Reflect:**

- Did the script run reliably every time?
- What happened if the modal hadn’t appeared by the time Selenium searched for the button?

#### 2. **Now test with an explicit wait**

- Remove the implicit wait.
- Use `WebDriverWait` and `expected_conditions` to pause until the button becomes clickable.

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
driver.get("file:///Users/username/path/to/example_file.html")  # Replace with the full absolute path to your local HTML file

# Wait up to 6 seconds for the modal dismiss button to become clickable
wait = WebDriverWait(driver, 6)
dismiss_button = wait.until(
    EC.element_to_be_clickable((By.ID, "dismiss-welcome"))
)
dismiss_button.click()

print("Modal dismissed using explicit wait.")
driver.quit()
```

**Reflect:**

- Was this more reliable?
- What advantages did you notice compared to the implicit wait version?

### Stretch challenge: Real-world adaptation

For those looking for an extra challenge, choose a real website you use regularly (one that requires login is not necessary). Identify a UI element that appears only after a short delay, or after you take an action. Try automating a wait for that element—first with an implicit wait, then with an explicit wait. Which approach works better, and why?

## Key takeaways

- Use implicit waits as a convenient, "set it and forget it" solution for predictable environments.
- Use explicit waits when you need flexibility and precision for dynamic elements or events.
- Avoid mixing both wait strategies in one script to prevent unpredictable results.
- Practicing both approaches on real-world pages will improve your confidence—and help you write Selenium scripts that are as adaptable as real users.
