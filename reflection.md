# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- When making a guess, you can't hit enter to guess, expected to confirm the guess.
- New game button doesn't work. 
- To make a new guess, you need to hit submit button again so that the previous number gets logged and then the next number is checked. Expected to check the guess right away.

---

## 2. How did you use AI as a teammate?

- Used copilot
- Example of a correct AI suggestion: The AI suggested using a Streamlit form (`st.form` + `st.form_submit_button`) for the guess input so that pressing Enter would submit immediately. I implemented this in `app.py` by wrapping the text input in a form and replacing the plain `st.button` with `st.form_submit_button`. I verified the fix manually by running the app and confirming that pressing Enter submits the guess immediately and that the guess is evaluated right away; the UI behavior matched the expectation and the unit tests still passed.

- Example of an incorrect or misleading AI suggestion: Initially the AI refactor suggested that `check_guess` should return both an outcome and a human-facing message (a tuple), and that the UI should consume that tuple. That was misleading because the existing tests import `check_guess` and expect a single outcome string. After I implemented the tuple return, `pytest` failed because the tests asserted a single string result. I fixed this by changing `check_guess` to return only the outcome string (so tests pass) and by moving the message mapping into `app.py`; I verified the fix by running the test suite and observing the tests pass.

---

## 3. Debugging and testing your fixes

- I decided a bug was fixed when both manual behavior and automated tests confirmed the expected outcome. For UI issues I verified by interacting with the app (pressing Enter to submit, using the New Game button, and observing the Developer Debug Info) to ensure the secret, attempts, and score behaved as expected. For logic and refactor changes I relied on `pytest` to validate that core functions returned the expected values and didn't break existing contracts. Only when both manual checks and unit tests passed did I consider the bug resolved.

- One test I ran was the existing unit tests that import `check_guess` (run via `pytest`). These tests verify that `check_guess(50, 50)` returns `"Win"`, `check_guess(60, 50)` returns `"Too High"`, and `check_guess(40, 50)` returns `"Too Low"`. Running the test suite after refactoring confirmed the logic functions behave as expected and that the refactor didn't introduce regressions (all tests passed). I also performed manual testing by entering the secret value shown in Developer Debug Info and confirming the UI displayed a win and updated the score.

- AI helped frame the refactor and testing approach by suggesting separation of logic from UI, which made the functions straightforward to unit test. However, one AI suggestion was misleading (changing `check_guess`'s return signature) and caused test failures; that failure was useful because it revealed the tests' expectations and guided me to revert the function signature. Overall AI helped me think about testability and separation of concerns, and the tests provided quick feedback when suggestions needed adjustment.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
