# Reflection:

My approach was guided by these key choices.

I did a **feature engineering**. My EDA showed that features like file extensions and specific keywords in commit messages were extremely strong role indicators. By manually creating features like binary flags for extensions (`has_js`, `has_py`) and calculating metrics like `netCodeChange`, I was able to build a very powerful baseline model (`logisticRegression`) that achieved a high Macro F1 score of 0.9764. This decision prioritized understanding the data over using a "black box" approach, aligning with the project's emphasis on reasoning.

For my improved model, I chose to create **custom word embeddings using `word2Vec`**. Instead of relying on simple word counts with TFIDF, I trained a model on my commit message data. I did this because the dataset has domain specific jargon (like "modal", "endpoint", "schema") that a custom model could learn the context for. This move to a richer, semantic representation of the text was the key factor that pushed the improved `xGBoost` model's Macro F1 score to 0.9813.

The most critical evaluation decision was **prioritizing the Macro F1 score**. My EDA immediately revealed a class imbalance, with the `qa` role being underrepresented. By choosing Macro F1 as the guider, I ensured that my model had to perform well on all roles, including the smallest one. This prevented me from building a misleading model that was simply good at predicting the majority classes and led to a more robust and a well evaluated solution.
---

# Annotated Examples:

Here are ten commits from the dataset, annotated to explain how their features lead to a specific role assignment.

### Example 1: Frontend (index 0):
* **Commit Type:** `feature`
* **File Extensions:** `['js_ts']`
* **Commit Message Snippet:** "Implement responsive UI component with dropdown and modal functionality, enhancing navigation and theme style across the page, with improved CSS and animation effects"
* **Annotation:** The combination of JavaScript/TypeScript file extensions (`js_ts`) and a message filled with user interface keywords like "UI component," "CSS," "style," and "animation" is a classic signature of a frontend developer.

### Example 2: Frontend (index 23):
* **Commit Type:** `feature`
* **File Extensions:** `['css']`
* **Commit Message Snippet:** "Refactor page layout with responsive CSS styles, implementing a new modal component with dropdown navigation..."
* **Annotation:** This commit exclusively involves CSS files and the message is focused on visual elements like "page layout," "responsive CSS styles," and "modal component." This is clearly frontend work.

### Example 3: Backend (index 6):
* **Commit Type:** `bugfix`
* **File Extensions:** `['py']`
* **Commit Message Snippet:** "Fixed authentication logic in backend API: validated user sessions and improved database query for login endpoint..."
* **Annotation:** The Python (`.py`) file extension combined with terms like "backend API," "authentication," "database query," and "login endpoint" strongly indicates a backend developer fixing server-side logic.

### Example 4: Backend (index 14):
* **Commit Type:** `bugfix`
* **File Extensions:** `['sql']`
* **Commit Message Snippet:** "Fixed authentication logic in backend API: validated user sessions and tokens upon login, updated database schema..."
* **Annotation:** A commit modifying SQL files and referencing "database schema," "authentication logic," and "backend API" is definitively the work of a backend developer focused on data and server security.

### Example 5: Backend (index 10):
* **Commit Type:** `feature`
* **File Extensions:** `['java_go']`
* **Commit Message Snippet:** "Implement API endpoint for user login with authentication and validation logic"
* **Annotation:** Using backend languages like Java/Go and implementing an "API endpoint" with "authentication" logic are core responsibilities of a backend developer.

### Example 6: Fullstack (index 8):
* **Commit Type:** `feature`
* **File Extensions:** `['js_ts', 'py', 'css']`
* **Commit Message Snippet:** "...Implemented responsive UI component with modal... added API and database validation for authentication..."
* **Annotation:** This commit is the hallmark of a fullstack role. It touches frontend technologies (`js_ts`, `css`) and mentions UI components, while also involving backend work (`py`) with "API and database validation." This mix of responsibilities is key.

### Example 7: Fullstack (index 13):
* **Commit Type:** `refactor`
* **File Extensions:** `['js_ts', 'md', 'html', 'test_js']`
* **Commit Message Snippet:** "...updated CSS styles for modal... added API endpoint for authentication and token validation; integrated backend logic for session management..."
* **Annotation:** Here again, we see a mix of concerns. The developer is refactoring frontend components ("CSS styles for modal") while also adding and integrating backend services ("API endpoint for authentication," "backend logic"). This dual focus points directly to a fullstack role.

### Example 8: QA (index 12):
* **Commit Type:** `test`
* **File Extensions:** `['test_js']`
* **Commit Message Snippet:** "QA: Updated test data for commit message model training..."
* **Annotation:** The evidence here is overwhelming: the commit type is `test`, the file extension is specifically a test file (`test_js`), and the message explicitly starts with "QA" and discusses "test data." This is a clear example of a QA commit.

### Example 9: QA (index 16):
* **Commit Type:** `bugfix`
* **File Extensions:** `['test_js']`
* **Commit Message Snippet:** "qa-213: Fixed test_js module by adding 202 lines... to address issues discovered during testing"
* **Annotation:** Even though the type is `bugfix`, the context is entirely about the testing infrastructure. The commit modifies a `test_js` file, references a QA ticket (`qa-213`), and mentions "issues discovered during testing." This is a QA developer fixing the tests themselves.

### Example 10: QA (index 28):
* **Commit Type:** `test`
* **File Extensions:** `['test_py']`
* **Commit Message Snippet:** "Added QA test cases for commit message analysis model: updated 5 test files..."
* **Annotation:** Similar to the other QA examples, this commit involves Python test files (`test_py`) and a message that explicitly states "Added QA test cases." The focus is on verifying the system's behavior, which is the primary role of QA.