# Node.js Developer Notes: Standards & AI Prompts

This document outlines key coding standards and best practices for Node.js development using pure JavaScript (ES6+ features). When generating or reviewing Node.js code with an AI/LLM, these points serve as a checklist to ensure high-quality, maintainable, and secure solutions.

## Standards Adhered To

### 1. Code Style & Readability (ES6+)

* **`const` and `let` Preference:** Always prefer `const` for variables whose values won't change, and `let` for variables that will be reassigned. **Avoid `var`** due to its function-scoping and hoisting quirks.

* **Arrow Functions (`=>`):** Use arrow functions for concise syntax, especially for callbacks and when lexical `this` binding is desired.

* **ES Modules (`import`/`export`):** Use `import` and `export` syntax for module management. Ensure `package.json` has `"type": "module"`.

* **Destructuring:** Use object and array destructuring for cleaner variable assignments.

* **Spread/Rest Operators:** Leverage `...` for array/object copying, merging, and variadic function arguments.

* **Template Literals:** Use backticks `` ` `` for string interpolation and multi-line strings.

* **Consistent Naming Conventions:** Use `camelCase` for variables and functions, `PascalCase` for classes, and `UPPER_SNAKE_CASE` for constants.

* **Code Linting & Formatting:** Use ESLint and Prettier to enforce a consistent code style (e.g., based on a popular guide like Airbnb or StandardJS) and identify potential issues automatically.

### 2. Classes & Object-Oriented Programming (ES6+)

* **Class Syntax:** Use the `class` keyword for defining constructors, methods, getters, and setters.

* **`constructor()` Method:** Initialize instance properties within the `constructor`.

* **`super()` Calls:** In derived classes, always call `super()` in the constructor before using `this`.

* **Instance Properties:** Define instance properties directly in the class body or within the constructor.

* **Static Methods/Properties:** Use `static` keyword for class-level methods or properties that don't depend on instance data.

* **Private Class Fields (`#`):** Use private fields (e.g., `#privateField`) for true encapsulation when appropriate.

* **Inheritance:** Use `extends` for class inheritance.

* **Method Binding:** Be mindful of `this` context in class methods, especially when passing them as callbacks. Use arrow functions or `.bind()` if necessary.

* **SOLID Principles:** Adhere to SOLID principles (Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion) for scalable and maintainable object-oriented code.

* **Dependency Injection (DI):** Prefer Dependency Injection to create loosely coupled, testable components. Avoid creating hard-coded dependencies inside classes.

### 3. "Strict Types" (JavaScript Only)

* **Strict Mode (`"use strict";`):** Ensure code runs in strict mode. For ES Modules (`"type": "module"` in `package.json`), this is automatic. For CommonJS, include `"use strict";` at the top of files/functions.

    * **Reasoning:** Strict mode catches common coding mistakes as errors, eliminates JavaScript's "silent failures," and makes code more robust and easier to optimize by engines.

* **JSDoc for Type Hinting:** While not enforced by the runtime, use **JSDoc comments** (`@param`, `@returns`, `@type`, `@property`) to document expected types for improved readability, IDE auto-completion, and static analysis (e.g., with VS Code's built-in JS type checking).

* **Input Validation:** Strictly validate and sanitize all incoming data (from HTTP requests, external APIs, file reads) at the earliest possible point.

* **Type Coercion Awareness:** Be aware of JavaScript's automatic type coercion and prefer explicit conversions or strict comparison (`===`) to avoid unexpected behavior.

### 4. Asynchronous Programming

* **`async`/`await` Preference:** Use `async`/`await` for handling asynchronous operations. This provides more readable and maintainable code compared to nested callbacks or complex Promise chains.

* **Promises:** Understand and use Promises for asynchronous operations, especially when integrating with third-party libraries that return Promises.

* **Concurrent Operations:** Use `Promise.all()` or `Promise.allSettled()` to efficiently handle multiple concurrent asynchronous operations.

* **Error-First Callbacks (Legacy):** Be familiar with, but avoid writing new code with, error-first callbacks.

* **Avoid Blocking the Event Loop:** Do not perform CPU-intensive or long-running synchronous operations directly on the main thread. Use Worker Threads for heavy computations or asynchronous I/O operations.

### 5. Module Organization

* **Small, Focused Modules:** Each module/file should have a single, well-defined responsibility.

* **Clear Exports:** Explicitly `export` only what needs to be public from a module.

* **Absolute vs. Relative Paths:** Use relative paths (`./`, `../`) for local modules and absolute paths (or package names) for installed dependencies.

### 6. Error Handling & Debugging

* **Centralized Error Handling:** Implement a global error handling mechanism (e.g., Express middleware). Use `process.on('uncaughtException')` and `process.on('unhandledRejection')` with extreme caution, primarily for logging the error before gracefully shutting down the process, not for resuming normal operation.

* **`try-catch` with `async`/`await`:** Always wrap `await` calls in `try-catch` blocks to handle rejections.

* **Custom Error Classes:** Create custom error classes that extend `Error` for specific application errors.

* **Logging:** Use a robust logging library (e.g., Winston, Pino) for structured logging of application activity and errors. Avoid `console.log` in production.

* **No Information Leakage:** Never expose sensitive error details (stack traces, internal paths) to clients in production.

### 7. Security

* **Input Validation & Sanitization:** Crucial for preventing XSS, SQL Injection, Command Injection, etc. Use libraries like `validator.js` or `express-validator`.

* **Path Traversal:** Sanitize and validate all user-provided file paths to prevent attackers from accessing unintended files (e.g., by using `../`).

* **Prototype Pollution:** Be cautious with libraries that perform deep object merges, as this can be a vector for prototype pollution. Keep dependencies updated.

* **Output Escaping:** Escape all user-generated content before rendering it in HTML to prevent XSS.

* **Dependency Security:** Regularly audit `node_modules` for known vulnerabilities (e.g., `npm audit`, Snyk). Keep packages updated.

* **Environment Variables:** Store sensitive information (API keys, database credentials) in environment variables (`process.env`), not directly in code.

* **Secure Headers:** Use security-focused middleware (e.g., Helmet for Express) to set appropriate HTTP headers.

* **Rate Limiting:** Implement rate limiting to prevent brute-force attacks and DoS.

* **Session Management:** Secure session cookies (HttpOnly, Secure, SameSite flags).

* **Access Control:** Implement robust authentication and authorization (e.g., JWT, OAuth2, ACLs).

* **Avoid Dangerous Functions:** Be cautious with functions that execute arbitrary code (e.g., `eval()`, `child_process.exec()`) if input is not strictly controlled.

### 8. Performance Optimization

* **Asynchronous I/O:** Leverage Node.js's non-blocking I/O model. Avoid synchronous file system or network operations in main application flow.

* **Worker Threads:** Use Worker Threads for CPU-bound tasks to avoid blocking the event loop.

* **Clustering:** Utilize the `cluster` module to distribute load across multiple CPU cores.

* **Caching:** Implement caching strategies (e.g., Redis) for frequently accessed data.

* **Stream Processing:** Use Node.js Streams for handling large amounts of data efficiently.

* **Database Query Optimization:** Optimize database queries and use proper indexing.

* **Payload Optimization:** Keep API request and response payloads lean.

### 9. Dependency Management

* **Package Manager:** Use `npm` or `yarn` to manage project dependencies.

* **Lock Files:** Always commit the lock file (`package-lock.json` or `yarn.lock`) to your repository. This ensures that every developer and CI/CD environment installs the exact same dependency versions, leading to reproducible builds.

* **Semantic Versioning:** Understand and respect semantic versioning (`^` and `~`) when defining dependencies in `package.json`.

### 10. Testing

* **Testing Frameworks:** Use a standard testing framework like **Jest** or **Mocha** with an assertion library like **Chai**.

* **Test Types:** Write a healthy mix of tests:
    * **Unit Tests:** Test individual functions or classes in isolation. Use mocking/stubbing libraries (e.g., Jest's built-in mocks, Sinon.js) to isolate dependencies.
    * **Integration Tests:** Test how multiple modules work together (e.g., a service layer interacting with a database module).
    * **End-to-End (E2E) Tests:** Test the full application flow from the user's perspective, often by making real HTTP requests to a running instance of the application (e.g., using Supertest).

* **Test Coverage:** Aim for meaningful test coverage. Focus on testing critical paths, business logic, and edge cases rather than just aiming for a high percentage.

## Questions to Ask an AI/LLM When Generating Node.js (JavaScript) Code

When prompting an AI for Node.js code, consider asking these questions to guide it towards best practices:

* "Am I using `const` or `let` for all variable declarations, avoiding `var`?"

* "Are **arrow functions** used appropriately for callbacks and lexical `this`?"

* "Is this module using **ES Modules (`import`/`export`)** syntax?"

* "How should I use **JSDoc** to document the types for this function/class?"

* "Is this code running in **strict mode**?"

* "Are **`async`/`await`** used for all asynchronous operations, and are errors handled with `try-catch`?"

* "Is this module **single-responsibility** and **loosely coupled**?"

* "How can I ensure all **user input is validated and sanitized** before processing?"

* "Am I storing **sensitive data** in environment variables, not hardcoding it?"

* "Are there any **synchronous I/O operations** that could block the event loop?"

* "Should this CPU-intensive task be offloaded to a **Worker Thread**?"

* "Are **private class fields (`#`)** used for internal state where appropriate?"

* "How can I optimize this **loop/data processing** for better performance?"

* "Am I using **Promises** correctly, with `.catch()` handlers or `try-catch` in `async` functions?"

* "Are all **dependencies** properly managed and audited for security?"

* "What **unit, integration, and E2E tests** should I write for this feature?"

* "Does this design adhere to **SOLID principles** and use **Dependency Injection**?"

* "Are all **dependencies** properly managed and audited for security?"