<h1>
  <span class="headline">Selenium: Waits, Alerts and Dynamic UIs</span>
  <span class="subhead">Advanced Strategies for Dynamic Content</span>
</h1>

**Learning objective:** Apply advanced wait patterns and retry logic to handle flaky or short-lived dynamic elements reliably using real examples from the SimpleShop web page.

## Why dynamic elements are hard to catch

Modern UIs often use:

- **Toasts** and temporary banners (like the welcome toast on login)
- **Dialogs** that vanish quickly (like the welcome modal)
- **Content injected by JS after delays** (like the Checkout button or Offers section)

These elements may:

- Appear and disappear faster than your script can act
- Vary in timing based on network/device
- Be missed if you rely on fixed timing or a single wait

## Technique: Retry wait pattern

**Used for:** `#toast` message shown after login

Instead of relying on one `WebDriverWait`, retrying can catch elements like the `#toast` that briefly appear:

```python
from selenium.common.exceptions import TimeoutException

success = False
for attempt in range(3):
try:
toast = wait.until(
EC.visibility_of_element_located((By.CSS_SELECTOR, "#toast"))
)
print("Toast says:", toast.text)
wait.until(
EC.invisibility_of_element_located((By.CSS_SELECTOR, "#toast"))
)
success = True
break
except TimeoutException:
print(f"Attempt {attempt+1}: Toast not found.")
```

> 💡 Retry loops help when elements like toast messages are unpredictable or short-lived.

## Technique: Custom wait conditions

**Used for:** waiting on `#dismiss-welcome` inside the delayed modal

You can use `lambda` to wait for an element that is both visible and interactable—useful for the welcome modal dismiss button:

```python
confirm_btn = wait.until(
lambda d: d.find_element(By.ID, "dismiss-welcome").is_displayed() and
d.find_element(By.ID, "dismiss-welcome").is_enabled()
)
confirm_btn.click()
```

> 💡 Use custom waits when animations or multiple state changes are involved.

## ✅ Final checklist for dynamic UIs

- [ ] Did I wait for every new UI element before interacting? (`#checkout-btn`, `#toast`)
- [ ] Did I use `invisibility_of_element_located` for spinners or toasts? (`#offer-spinner`, `#toast`)
- [ ] Do I use retry logic for short-lived messages? (`#toast`)
- [ ] Am I accounting for both appearance and disappearance where needed?
- [ ] Did I avoid mixing implicit and explicit waits?

## Optional Challenge: Make Your Own Retry Function

**Use `#toast` as the dynamic element to test.**

```python
def wait*for_toast(driver, selector, timeout=6, retries=3):
wait = WebDriverWait(driver, timeout)
for * in range(retries):
try:
toast = wait.until(EC.visibility_of_element_located((By.CSS_SELECTOR, selector)))
print("Toast:", toast.text)
wait.until(EC.invisibility_of_element_located((By.CSS_SELECTOR, selector)))
return True
except TimeoutException:
pass
return False
```

In SimpleShop and other real-world UIs, timing issues are common. You now know how to:

- Catch elements like `#toast` using retries
- Wait for both appearance and disappearance (`#offer-spinner`, `#toast`)
- Use custom waits for elements like modal buttons
- Write robust, stable tests using best practices from this module
