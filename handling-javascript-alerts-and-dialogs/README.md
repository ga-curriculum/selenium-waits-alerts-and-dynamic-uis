<h1>
  <span class="headline">Selenium: Waits, Alerts, and Dynamic UIs</span>
  <span class="subhead">Handling JavaScript Alerts and Dialogs</span>
</h1>

**Learning objective:** Handle JavaScript browser alerts, confirms, and prompts using `driver.switch_to.alert`.

## What are browser dialogs?

So far, you’ve worked with elements like buttons and modals that Selenium can interact with using CSS selectors. But now you’ll learn about a different kind of popup: **browser dialogs**.

These dialogs—**alerts**, **confirms**, and **prompts**—are created by the browser, not by the page’s HTML. You can’t select them with CSS or inspect them in DevTools. Instead, you’ll use Selenium’s `switch_to.alert` to handle them.

## Why they matter in automation

Imagine your script clicks **Submit**, and a popup appears asking for confirmation or input. If you don’t handle the dialog, your script will pause or fail.

Browser dialogs must be handled **right away**. They block all other page actions until you accept, cancel, or dismiss them.

> 💡 Think of these as "pop-up roadblocks"—you can’t continue until you deal with them.

### Common Types of Browser Dialogs

| Type        | Description                         | Buttons     | Example Message                              |
| ----------- | ----------------------------------- | ----------- | -------------------------------------------- |
| **Alert**   | Shows a message                     | OK          | “Thank you for your feedback.”               |
| **Confirm** | Asks to confirm or cancel an action | OK / Cancel | “Are you sure you want to delete this item?” |
| **Prompt**  | Asks for text input from the user   | OK / Cancel | “Enter your email address.”                  |

> 📘 These dialogs are triggered by JavaScript functions like `alert()`, `confirm()`, and `prompt()`. Selenium handles them with `driver.switch_to.alert`.

## Why dialogs require special handling

- **Block automation flow:** Dialogs prevent any action on the web page until dismissed or accepted.
- **Not part of the DOM:** Selenium cannot find them using element locators.
- **Script failures:** If you do not account for dialogs, Selenium scripts will halt or fail unexpectedly.

Let’s learn how Selenium helps you control these dialogs.

## Controlling dialogs with Selenium

Selenium provides a single interface to interact with all browser dialogs: `driver.switch_to.alert`.

This tool allows you to:

- Switch focus from the web page to the open dialog.
- Read dialog text.
- Accept or dismiss the dialog with the right method.
- Enter text into prompts.

### How to interact with JavaScript dialogs in Selenium

Let’s connect actions with concrete Selenium methods.

#### 1. Switch to the open dialog

To interact with any dialog, you must first switch Selenium’s context.

```python
alert = driver.switch_to.alert # Now you can work with the dialog
```

If no dialog is present, Selenium will raise an error.

#### 2. Read the dialog’s message

Every dialog presents a message to the user. Use `.text` to retrieve it.

```python
message = alert.text
print(message) # Prints the dialog's message
```

#### 3. Responding to the dialog

- To click **OK** (accept): `.accept()`
- To click **Cancel** (dismiss): `.dismiss()` (only available for Confirm and Prompt dialogs)

```python
alert.accept() # Clicks "OK"
alert.dismiss() # Clicks "Cancel" (for confirm/prompt)
```

- For a **Prompt** dialog, enter text before accepting or dismissing.

```python
alert.send_keys('Your input here')
alert.accept() # Submits the input and closes the dialog
```

If you accept a prompt without entering text, Selenium submits an empty response.

### Selenium dialog methods

| Method             | Purpose                                       | Applies to      |
| ------------------ | --------------------------------------------- | --------------- |
| `.text`            | Gets dialog message                           | All dialogs     |
| `.accept()`        | Clicks “OK”                                   | All dialogs     |
| `.dismiss()`       | Clicks “Cancel”                               | Confirm, Prompt |
| `.send_keys(text)` | Types into prompt field (then accept/dismiss) | Prompt only     |

## Handling dialogs in automation scripts

Let’s see practical examples.

### Example 1: Handling an alert

Suppose you are automating a checkout process. Submitting the form produces an alert saying: “Success: Order Placed!”

Here’s how to handle it:

```python
from selenium import webdriver
import time

driver = webdriver.Chrome()
driver.get('https://the-internet.herokuapp.com/javascript_alerts')

# Trigger the alert by clicking the appropriate button

driver.find_element('css selector', "button[onclick='jsAlert()']").click()

# Switch to the alert and accept it

alert = driver.switch_to.alert
print(alert.text) # Prints: "I am a JS Alert"
alert.accept() # Dismisses the alert and lets your script continue

driver.quit()
```

### Example 2: Handling a confirm dialog

A confirm dialog is often used for irreversible actions, like deleting an account:

```python
driver = webdriver.Chrome()
driver.get('https://the-internet.herokuapp.com/javascript_alerts')

driver.find_element('css selector', "button[onclick='jsConfirm()']").click()
confirm = driver.switch_to.alert

print(confirm.text)
confirm.dismiss() # Choose "Cancel" to avoid deletion

driver.quit()
```

### Example 3: Handling a prompt

A prompt can be used for collecting verification codes or feedback.

```python
driver = webdriver.Chrome()
driver.get('https://the-internet.herokuapp.com/javascript_alerts')

driver.find_element('css selector', "button[onclick='jsPrompt()']").click()
prompt = driver.switch_to.alert

print(prompt.text)
prompt.send_keys('Selenium Rocks') # Type your message
prompt.accept() # Submit

driver.quit()
```

> 🏆 Best practice: Always use explicit waits before switching to an alert, especially if a dialog may appear with a delay.

### Waiting for a dialog with explicit waits

Dialogs may not appear instantly—especially if triggered by network events or delayed scripts. You should use explicit waits to pause until the dialog appears.

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

wait = WebDriverWait(driver, 5)
alert = wait.until(EC.alert_is_present()) # Waits up to 5 seconds
print(alert.text)
alert.accept()
```

<div class="activity group-exercise">
  <h2 class="title">Automate JavaScript Browser Dialogs in SimpleShop</h2>
  <span class="minutes">15 min</span>
</div>

Learn how to detect and respond to browser-native dialogs using Selenium's `switch_to.alert`. You’ll interact with:

- An `alert()` ("Clear Cart")
- A `confirm()` dialog ("Delete Item")
- A `prompt()` for user input ("Request Email")

### Scenario

You’ve just added browser dialog handling to your test flow. Now you need to verify that:

- Each dialog appears when triggered
- You can read and respond to it using Selenium
- You can assert or log its contents

Using the following starter code:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

driver = webdriver.Chrome()
driver.get("file:///path/to/your/index.html") # Replace with your actual file path

# 1. Handle alert

# 2. Handle confirm

# 3. Handle prompt

print("Dialogs handled successfully.")
driver.quit()
```

Write out, test, and discuss each with your group:

#### 1. Click the **Clear Cart** button

- Switch to the alert dialog
- Print its text to the console
- Accept it (simulate clicking "OK")

```python
driver.find_element(By.ID, "clear-cart").click()
alert = driver.switch_to.alert
print("ALERT TEXT:", alert.text)
alert.accept()
```

#### 2. Click the **Delete Item** button

- Switch to the confirm dialog
- Print its text
- Accept it **or** dismiss it and log which action was taken

```python
driver.find_element(By.ID, "delete-item").click()
confirm = driver.switch_to.alert
print("CONFIRM TEXT:", confirm.text)

# You can choose either:
confirm.accept()
# OR
# confirm.dismiss()
```

#### 3. Click the **Request Email** button

- Switch to the prompt dialog
- Print its text
- Send the input `test@example.com` and accept
- Log a message confirming the value was submitted

```python
driver.find_element(By.ID, "request-email").click()
prompt = driver.switch_to.alert
print("PROMPT TEXT:", prompt.text)
prompt.send_keys("test@example.com")
prompt.accept()
```

### Reflect

- What happens if you forget to call `.switch_to.alert`?
- Why is `.send_keys()` only supported in prompts, not alerts or confirms?
- What would break if you tried to `click()` something while a dialog is open?
