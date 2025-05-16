<h1>
  <span class="headline">Selenium: Waits, Alerts and Dynamic UIs</span>
  <span class="subhead">Using WebDriverWait and Expected Conditions</span>
</h1>

**Learning Objective:** Use `WebDriverWait` and expected conditions to wait for elements to become visible, clickable, or present in the DOM.

## Why precision matters in dynamic environments

You’ve already seen how **implicit waits** help Selenium keep looking for elements that might load a little late. But in real-world UIs, pages often update _asynchronously_—with elements appearing, disappearing, or becoming interactive at unpredictable times.

That’s where **explicit waits** shine: they let you pause your script **only when needed**, based on the exact state of a specific element.

## Real-world scenario: Waiting for UI readiness

Imagine a user lands on your e-commerce site and clicks **“Load Offers”** to see today’s deals. Behind the scenes:

1. A loading **spinner** appears while the system fetches new content.
2. Two **promo cards** fade in after 2–4 seconds.
3. The spinner disappears once the content is ready.

Trying to interact with these elements **too soon** will break your test. With `WebDriverWait`, you can synchronize with this kind of behavior.

## `WebDriverWait` + expected conditions

Here’s how it works:

- **Create** a wait object, specifying a max timeout.
- **Apply** an expected condition (like “element is clickable” or “element is no longer visible”).
- Selenium **polls** at short intervals and proceeds only when the condition is met—or fails after the timeout.

### Example pattern

```python
wait = WebDriverWait(driver, 10)

# Wait for a button to be clickable
load_btn = wait.until(
    EC.element_to_be_clickable((By.ID, "load-offers"))
)
```

> Think of it like “pause until this is safe to click” — not just “pause for a few seconds.”

## Common expected conditions

| Condition                         | Use when...                                    |
| --------------------------------- | ---------------------------------------------- |
| `element_to_be_clickable`         | A button or link must be ready for interaction |
| `visibility_of_element_located`   | You need to confirm something is fully shown   |
| `presence_of_element_located`     | You just want to confirm DOM presence          |
| `invisibility_of_element_located` | You need to wait for something to disappear    |

<div class="activity guided-walkthrough">
  <h2 class="title">Synchronize with a delayed offers section</h2>
  <span class="minutes">10 min</span>
</div>

We will use the **SimpleShop** mock site to practice chaining waits through a dynamic interaction sequence:

1. Wait for and click the **“Load Offers”** button.
2. Wait for the **spinner** to disappear.
3. Wait for **both promo cards** to appear.
4. Print the title of the second offer.

### Step by Step

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
driver.get("file:///path/to/your/index.html")  # Replace with your file path

wait = WebDriverWait(driver, 10)

# 1. Wait for Load Offers button to be clickable, then click it
load_btn = wait.until(EC.element_to_be_clickable((By.ID, "load-offers")))
load_btn.click()

# 2. Wait for the loading spinner to disappear
wait.until(EC.invisibility_of_element_located((By.ID, "offer-spinner")))

# 3. Wait for both promo cards to be present
cards = wait.until(EC.presence_of_all_elements_located((By.CLASS_NAME, "promo-card")))

# 4. Print the last card's text
print("Second offer:", cards[-1].text)

driver.quit()
```

## Reflect

- Why wouldn't a basic `find_element()` call be reliable in this scenario?
- Which condition best matches each step in this interaction?

## Key Takeaways

- Use `WebDriverWait` when element timing is uncertain or tied to animations, spinners, or async data.
- Choose the expected condition that matches your intent: visibility, clickability, presence, or invisibility.
- Chain waits step-by-step for multi-stage flows.
