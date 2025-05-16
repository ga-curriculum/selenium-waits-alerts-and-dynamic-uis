<h1>
  <span class="headline">Selenium: Waits, Alerts, and Dynamic UIs</span>
  <span class="subhead">Instructor Guide</span>
</h1>

## Understanding Implicit and Explicit Waits

### How to deliver this microlesson

- Begin with a story or analogy about waiting for webpages or delivery—connect with learners' experiences of waiting for apps to load or actions to become available.
- Demonstrate both implicit and explicit waits step-by-step, showing what happens in the browser when the waits are too short or just right.
- Use the code samples as live demos, pausing to have learners predict what will happen before running each script.
- Facilitate discussion after the practice activity, focusing on common roadblocks (for example, elements stubbornly not appearing, or scripts that work one time but break the next).
- Encourage learners to think through which technique would work better for more complex, less predictable sites.

### Answers and solutions

#### Stretch challenge response

- **Observation:** Explicit waits provide better control and success for dynamic or unpredictable UI elements. Implicit waits may result in failed tests if the global timeout is shorter than a dynamic element's delay.

#### Code solutions

- For the main activity, scripts as provided in the lesson notes and Python code.

---

## Using WebDriverWait and Expected Conditions

### Delivering this microlesson

- Start by reinforcing the problem this microlesson solves: Selenium can’t safely interact with elements until they’re _ready_. Unlike static pages, modern UIs require **chained logic** for elements that load, animate, or depend on user actions.
- Emphasize the **difference between presence, visibility, and clickability**. Ask: “When is it safe to interact with an element?”
- Use the **SimpleShop “Load Offers” flow** to demonstrate how chaining multiple explicit waits mirrors real-world automation workflows.
- Step through the activity code in stages:
  1. Wait for the button
  2. Wait for the spinner to disappear
  3. Wait for cards to appear
- Encourage learners to **comment out individual waits** to trigger errors. Discuss what breaks and why.
- Invite learners to share examples from past testing experiences where they ran into race conditions or flaky scripts.
- Clarify that using specific conditions (rather than `time.sleep()` or overly broad presence checks) is the key to writing reliable tests.

---

## Handling JavaScript Alerts and Dialogs

### Goal

Teach students how to detect and interact with browser-native dialogs (alert, confirm, prompt) using `driver.switch_to.alert`.

### Key Reminders

- These dialogs **block the page** and **must be handled immediately**.
- They are **not part of the DOM**—you can’t locate them with selectors.
- Use `driver.switch_to.alert` to interact with them.
- Use `.text`, `.accept()`, `.dismiss()`, and `.send_keys()` (prompt only).

### Activity Notes

- Let students test each dialog one at a time
- Discuss what happens if the dialog isn’t handled
- Encourage use of EC.alert_is_present() for safer waits

---

🏗️ **Under Construction**

We are constantly working to improve our resources for instructors and students.

**Have something to contribute to this Instructor Guide?** [Let us know](https://pages.git.generalassemb.ly/modular-curriculum-all-courses/universal-resources-internal/module-feedback.html).

```

```
