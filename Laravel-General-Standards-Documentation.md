# Laravel Developer Notes: Standards & AI Prompts

This document outlines key coding standards and best practices specifically for Laravel applications. When generating or reviewing Laravel code with an AI/LLM, refer to these points to ensure adherence to framework conventions, security, and maintainability.

---

## Standards Adhered To

### 1. MVC & Architectural Patterns

* **Separation of Concerns (SRP):**
    * **Thin Controllers:** Controllers should primarily handle request/response cycles, delegating complex business logic to dedicated **Service Classes**.
    * **Models for Data:** Eloquent models should focus on database interaction and data relationships.
* **Laravel Conventions:** Follow Laravel's naming conventions (e.g., singular model names, plural table names, RESTful controller methods).
* **Service Layer:** Extract reusable business logic into dedicated service classes.
* **Queues & Jobs:** Offload long-running tasks (e.g., sending emails, processing images) to **Queues** to improve application response time.

### 2. Eloquent & Database Interactions

* **Mass Assignment Protection:** Always define `$fillable` (whitelist) or `$guarded` (blacklist) properties on Eloquent models to prevent mass assignment vulnerabilities.
* **Eloquent/Query Builder Preference:** Prefer Eloquent ORM or Laravel's Query Builder for database interactions.
* **No Raw DB Queries (with unsanitized input):** Avoid `DB::raw()` or `DB::statement()` with unsanitized user input to prevent SQL Injection.
* **Eager Loading (N+1 Prevention):** Use **Eager Loading** (`with()`) to prevent N+1 query problems when loading model relationships.

### 3. Requests & Validation

* **Form Requests:** Utilize **Form Request classes** for all complex input validation.
* **Comprehensive Validation:** Validate all incoming user data (from forms, APIs) thoroughly.

### 4. API Design

* **Laravel Resources:** Use **API Resource classes** to transform Eloquent models into consistent and standardized JSON API responses.
* **API Authentication:** Implement robust authentication mechanisms (e.g., Laravel Sanctum, or a custom API key middleware for simpler cases).
* **API Rate Limiting:** Apply **rate limiting** to API endpoints to prevent abuse and ensure fair resource usage.

### 5. Blade & Frontend Security

* **XSS Prevention:** Always escape user-provided data in Blade templates using `{{ }}` syntax to prevent Cross-Site Scripting (XSS) attacks. Use `{!! !!}` with extreme caution and only with trusted, pre-sanitized HTML.

### 6. Configuration

* **Use `config()` Helper:** Access configuration values using the `config()` helper. Avoid using `env()` directly in application code (outside of config files) as it will not work when configuration is cached for performance.

### 7. Testing

* **Test-Driven Development (TDD):** Adopt a TDD approach where tests are written before code.
* **PHPUnit:** Use PHPUnit for both **Unit Tests** (isolated components) and **Feature Tests** (API endpoints, Artisan commands).
* **Database Refreshing:** Use `RefreshDatabase` trait in feature tests for a clean database state.

### 8. Debugging

* **No Debugging Functions in Production:** Ensure `dd()` or `dump()` are removed from production code.

### 9. Dependency Management & Autoloading

* **Composer:** Use Composer for managing all PHP dependencies.
* **Autoloading Preference:** Rely on **Composer's autoloader** for loading classes, interfaces, and traits. Avoid manual `include`, `require`, `include_once`, or `require_once` statements for application classes.

---

## Questions to Ask an AI/LLM When Generating Laravel Code

When prompting an AI for Laravel code, consider asking these questions to ensure it adheres to Laravel-specific best practices:

* "Should this business logic be in a **Service Class** instead of directly in the controller?"
* "Am I using a **Form Request** for validation in this controller method?"
* "Is this Eloquent model properly protected against **mass assignment** (using `$fillable` or `$guarded`)?"
* "Am I using **Eloquent** or the **Query Builder** securely for this database operation, avoiding raw SQL with unsanitized input?"
* "Am I introducing an **N+1 query problem** here? Should I use **eager loading** (`with()`)?"
* "Am I using a **Laravel Resource** to format this API response?"
* "Is this API endpoint properly **authenticated** and **rate-limited**?"
* "Should this task be processed **asynchronously** in a **Queue Job** to improve user response time?"
* "Am I properly **escaping all output** in this Blade template to prevent XSS vulnerabilities?"
* "What **unit tests** and **feature tests** should I write for this new Laravel component/feature?"
* "Are there any **`dd()` or `dump()`** calls left in this code?"
* "Am I following **Laravel's conventions** for naming and folder structure?"
* "Am I accessing environment variables correctly using the `config()` helper instead of `env()`?"
* "Is a **constructor** actually required for this class, or can it be omitted?"
* "Am I relying on **Composer's autoloader** for class loading, rather than manual `include`/`require` statements?"