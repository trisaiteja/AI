# Code Review Checklist  

This document defines the standard practices for reviewing code in our applications. It applies to **Java, JavaScript, jQuery, JSP, Struts 1.x, Hibernate, Spring Boot**, and related technologies.  

The goals of this checklist are:  
- Ensure **Acceptance Criteria (AC)** are met.  
- Maintain **security, performance, and maintainability**.  
- Promote **readability and consistency** across the codebase.  

---

## ✅ General Code Review Guidelines  
- Are all **Acceptance Criteria** as defined by the development process met?  
- Is the code **readable, consistent, and maintainable**?  
- Are methods/functions **small and single-responsibility**?  
- Are **naming conventions** followed (classes → PascalCase, methods/variables → camelCase)?  
- Is duplicate code avoided (favor reuse/utilities)?  
- Are comments clear and **explain why, not what**?  
- Are comments free of **sensitive information** (credentials, secrets, tokens)?  
- Is consistent **formatting and indentation** followed?  

---

## 🔒 Security Checklist  
- Are framework-provided **security features leveraged** where possible?  
- Is **query parameterization** used for all SQL/HQL queries?  
- Are inputs **validated**:  
  - **Syntax validity** (form, structure)?  
  - **Semantic validity** (range, meaning)?  
- Are outputs properly **encoded** (HTML, JavaScript, SQL)?  
- Is sensitive data **encrypted in transit** (HTTPS/TLS)?  
- Is sensitive data **encrypted at rest** (DB, files)?  
- Is sensitive data **excluded from logs**?  
- Are logs meaningful (who did what, when)?  
- Are exceptions handled gracefully?  
  - No unhandled Null Pointer Exceptions (NPEs).  
  - Exceptions are logged securely (no PII in stack traces).  
- Are new DB columns/scripts **sanitized** if they may contain PII?  
- Was application security consulted for any hard-coded or sensitive items?  
- Is CSRF protection enabled for forms and APIs?  
- Is authentication & authorization enforced for all endpoints?  
- Is session management secure (no sensitive data in session, sessions invalidated on logout)?  

---

## ☕ Java / Spring Boot / Hibernate  
- Use **DTOs/POJOs with Lombok** (`@Data`, `@Builder`) instead of exposing entities directly.  
- Validate request DTOs using **JSR-303 annotations** (`@NotNull`, `@Email`, `@Size`, etc.).  
- Use `Optional` for nullable returns instead of returning `null`.  
- Ensure proper **transaction management** (`@Transactional` on service layer, not controllers).  
- Avoid **eager fetching** in Hibernate unless required.  
- Close DB resources using **try-with-resources**.  
- No secrets/configs in code → externalize to config files.  
- REST endpoints:  
  - Follow **HTTP status conventions**.  
  - Return meaningful error messages without leaking sensitive details.  

---

## 🖥 JSP / Struts 1.x  
- Avoid **scriptlets** (`<% %>`); use JSTL or Expression Language (EL).  
- Validate user input in **Struts ActionForms**.  
- Ensure Struts mappings are correct, with no open redirects.  
- Escape user input before rendering in JSP (prevent XSS).  
- Remove deprecated APIs and flag legacy Struts risks.  

---

## 🌐 JavaScript / jQuery  
- Use `let`/`const` instead of `var`.  
- Avoid `eval()` and `Function()` constructors.  
- Sanitize data before injecting into the DOM.  
- Always handle errors in asynchronous code (`fetch`, AJAX).  
- Use **event delegation** instead of binding multiple event listeners directly.  
- Keep DOM manipulation efficient (avoid repeated nested selectors).  

---

## 📊 Performance & Maintainability  
- Are DB queries optimized (no **N+1 issues** in Hibernate)?  
- Are **indexes** created for new DB columns used in WHERE/JOIN clauses?  
- Is pagination implemented for endpoints returning large datasets?  
- Avoid inline SQL → keep queries in DAO/Repository.  
- Is the frontend efficient (avoid unnecessary loops/reflows)?  

---

## 🧪 Testing & Coverage  
- Were new/updated **unit tests** created?  
- Are **integration tests** added for DB/API changes?  
- Do test cases cover **positive, negative, and edge scenarios**?  
- Do tests validate **security rules** (input validation, output encoding)?  

---

## 📦 Dependency & Build Management  
- Are third-party libraries **encapsulated**?  
- Are vulnerable dependencies avoided (`mvn dependency-check`, `npm audit`)?  
- Are dependency versions pinned (in `pom.xml` / `package.json`)?  
- Is dependency injection used instead of manual instantiation (`new`)?  
- Is jQuery version secure (avoid 1.x/2.x with known vulnerabilities)?  

---

## ✅ Review Flow  
1. Check **AC compliance**.  
2. Validate **security**.  
3. Check **code quality & maintainability**.  
4. Validate **framework-specific best practices**.  
5. Ensure **tests exist and pass**.  
6. Verify **performance considerations**.  
7. Approve or request changes.  
