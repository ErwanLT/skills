# Struts 2 Best Practices (Actions, Patterns, Modernization)

## Table of Contents
- Actions & Results
- Interceptors & Security
- Validation
- Testing Actions
- Modernization & Migration Tips
- Review Checklist

---

## Actions & Results

- Use **Thread-safe Actions**: Remember that Struts 2 creates a new instance of the Action for every request.
- Keep Actions **thin**: Logic should reside in service layers, not in the Action itself.
- Use **POJO Actions**: Prefer implementing `Action` interface or extending `ActionSupport` only when needed (for validation/i18n).
- Define **Result types** clearly (dispatcher, redirectAction, json, etc.).

**When to act:**
- Business logic embedded in Actions.
- Inconsistent use of Result types.
- Over-reliance on the `ServletActionContext`.

---

## Interceptors & Security

- Use **Interceptors** for cross-cutting concerns (logging, authentication, validation).
- **OGNL Security**: Be extremely careful with OGNL expressions. Sanitize inputs and restrict access to the context.
- Configure the **Parameters Interceptor** to allow only specific fields (using `acceptWhitelist`).
- Use the `token` interceptor to prevent double-submits and CSRF.

**When to act:**
- Security warnings related to OGNL.
- Repetitive logic in Actions that could be an Interceptor.
- Lack of input filtering.

---

## Validation

- Prefer **XML Validation** (`-validation.xml`) for complex rules to keep Java code clean.
- Use **Annotation-based validation** for simple, field-level constraints.
- Always provide meaningful error messages via `i18n` resource bundles.
- Ensure `workflow` interceptor is present to redirect to the `input` result on error.

**When to act:**
- Manual validation logic inside `execute()` methods.
- Hardcoded error messages.
- Missing `input` results in `struts.xml`.

---

## Testing Actions

- Use `StrutsTestCase` (JUnit plugin) to simulate the Struts environment.
- Mock services injected into Actions to ensure pure unit tests.
- Verify the **Action mapping** and the **Result** returned.
- Test the validation logic by injecting "bad" parameters.

**When to act:**
- No tests for Action logic.
- Difficulty testing Actions due to Servlet API dependencies.

---

## Modernization & Migration Tips

- **Dependency Injection**: Use the Struts-Spring plugin or similar to avoid manual service lookups.
- **REST Plugin**: Use the REST plugin if you need to expose modern APIs from a Struts base.
- **Gradual Migration**: Prepare for migration by decoupling the service layer from Struts-specific classes.

---

## Review Checklist

### Actions
- Thin actions (logic in services).
- Proper use of `ActionSupport`.
- Thread-safety respected.

### Configuration
- `struts.xml` is organized (use `include`).
- Namespace usage is consistent.
- `input` results are defined for all forms.

### Security
- Parameters whitelist configured.
- OGNL vulnerabilities addressed.
- CSRF protection enabled for sensitive actions.

### Testing
- `StrutsTestCase` used for Action verification.
- Services are mocked.

---

## Example (Struts 2 Action with Validation)

```java
public class UserAction extends ActionSupport {
    private User user;
    private UserService userService; // Injected

    @Override
    public String execute() {
        userService.save(user);
        return SUCCESS;
    }

    public void validate() {
        if (user.getName() == null || user.getName().length() == 0) {
            addFieldError("user.name", "Name is required");
        }
    }

    // Getters and Setters
}
```
