# PHP Developer Notes: Standards & AI Prompts

This document outlines key coding standards and best practices for PHP development. When generating or reviewing PHP code with an AI/LLM, these points serve as a checklist to ensure high-quality, maintainable, and secure solutions.

## Standards Adhered To

---

### 1. Code Style & Readability

* **PSR-12 Compliance:** Adhere to PHP Standard Recommendations for coding style.

* **Explicit Type Declarations:** Use **type hints** for all function/method parameters, return types, and class properties.

* **PHPDoc for Clarity:** Use PHPDoc blocks to explain the purpose of complex functions, their parameters, and any non-obvious logic, complementing type hints.

* **Strict Comparison:** Prefer `===` and `!==` over `==` and `!=` to avoid type juggling issues.

* **No Short Open Tags:** Always use `<?php` and `<?=` instead of `<?`.

* **No Empty Constructors:** Remove constructors that have no custom logic or dependency injection.

* **Concise Functions:** Prefer **arrow functions (`fn`)** for single-expression closures (PHP 7.4+).

* **First-Class Callables:** Use `ClassName::method(...)` syntax for callables (PHP 8.1+) for better type safety.

* **No Chained Assignments:** Avoid `$a = $b = 0;` for clarity.

* **Simplified Booleans:** Simplify `condition ? true : false` to `condition`.

### 2. Variable & Constant Management

* **Prefer Enums:** Use **Enums** (PHP 8.1+) for a fixed set of distinct values instead of class constants or string literals.

### 3. Function & Method Design

* **Return Type Declarations:** All functions and methods should explicitly declare their return types.

* **No Variable Functions:** Avoid dynamic function calls like `$funcName()`.

### 4. Error Handling & Debugging

* **Graceful Error Handling:** Use `try-catch` blocks for expected errors and exceptions for unexpected issues.

* **No `die()`/`exit()`:** Avoid abrupt script termination. Use exceptions or appropriate HTTP responses.

* **No Error Control Operator (`@`):** Do not suppress errors. Allow them to propagate for proper logging and handling.

### 5. Security

* **No `eval()`:** Never use `eval()` due to severe code injection risks.

* **Safe Deserialization:** Avoid `unserialize()` with untrusted input to prevent PHP Object Injection.

* **Command Injection Prevention:** Be extremely cautious with `shell_exec()`, `exec()`, `system()`, `passthru()`, and backtick operators (`` ` ``). Ensure all input is strictly sanitized.

* **Environment Variable Access:** Do not use `env()` helper directly in application code. Access environment variables via `config()` after they're loaded into configuration files.

* **Secure Input Handling:** Avoid direct access to superglobals (`$_GET`, `$_POST`, `$_REQUEST`, `$_FILES`) without robust validation and sanitization.

* **No Hardcoded Credentials:** Never embed sensitive data (passwords, API keys, usernames) directly in code. Use environment variables or a secure configuration system.

* **Exposed Files:** Ensure sensitive files (`.env`, `config.php`, application logic files) are not directly accessible via a web server.

* **File/Folder Naming:** Avoid spaces and special characters in file and folder names to prevent parsing issues and security risks.

### 6. Modern PHP Features (PHP 8.x)

* **Constructor Property Promotion:** Use this feature for concise constructor definitions (PHP 8.0+).

* **`match` Expression:** Prefer `match` over `switch` for simple equality checks (PHP 8.0+).

* **Nullsafe Operator:** Use `?->` for chained method calls on potentially null objects (PHP 8.0+).

* **Union Types:** Utilize `TypeA|TypeB` for parameters and return types (PHP 8.0+).

### 7. Architecture & Design

* **SOLID Principles:** Adhere to SOLID principles (Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion) for scalable and maintainable object-oriented code.

* **Dependency Injection:** Prefer Dependency Injection (DI) to create loosely coupled, testable components. Avoid using global state (`$GLOBALS`, `global` keyword) or service locators.

* **Immutability:** Favor immutable objects and use `readonly` properties (PHP 8.2+) where possible to prevent unintended state changes and create more predictable code.

* **Prefer `final` Classes:** Consider making classes `final` by default to prevent inheritance unless a clear and intentional extension point is designed.

### 8. Dependency Management & Autoloading

* **Composer:** Use Composer for managing all PHP dependencies.
* **Autoloading Preference:** Rely on **Composer's PSR-4 autoloader** for loading classes. Avoid manual `include`, `require`, `include_once`, or `require_once` statements for application classes.

### 9. File Management

* **No `php.ini` in Codebase:** `php.ini` files are server configurations and should not be committed to the repository.

---

## Questions to Ask an AI/LLM When Generating PHP Code

When prompting an AI for PHP code, consider asking these questions to guide it towards best practices:

* "Should I add **type hints** to all parameters and return types in this function/method?"

* "Am I using **PHPDoc** to clarify the purpose and usage of this code?"

* "Is this comparison **strictly typed** (`===`)?"

* "How should I handle **errors** here using exceptions, rather than `die()` or `@`?"

* "Can this closure be converted to a more concise **arrow function** (`fn`)?"

* "Am I properly **sanitizing** all user input before using it in database queries or shell commands?"

* "Am I avoiding **global variables** and using **dependency injection** instead?"

* "Can I use **constructor property promotion** in this class?"

* "Is a **`match` expression** a better fit than `switch` here?"

* "Can I use the **nullsafe operator** to simplify this null check?"

* "Should I use a **Union Type** for this parameter?"

* "Is an **Enum** a better fit than class constants for these values?"

* "Am I accessing environment variables securely (via `config()`, not `env()`)?"

* "Are there any **hardcoded credentials** in this code?"

* "Does this design adhere to **SOLID principles**?"

* "Can this property or class be made **immutable** or `readonly`?"

* "Should this class be marked as `final` to prevent unintended extension?"

* "Is this file's name compliant with **best practices** (no spaces/special characters)?"

* "Am I relying on **Composer's autoloader** rather than manual `require` statements?"