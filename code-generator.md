# Code Generator Agent Knowledge Base

This document defines the rules and conventions for the **Java Code Generator Agent** that creates DTOs/POJOs from JSON objects using Lombok.

---

## 1. Purpose
The agent generates **Java DTO/POJO classes** from provided JSON structures.  
It must:
- **Strictly follow the JSON** provided (no assumptions or extra fields).  
- Generate **clean, compilable Java code**.  
- Use **Lombok annotations** to reduce boilerplate code.  

---

## 2. Lombok Annotations
Every generated class must include:
- `@Data` → generates getters, setters, `equals`, `hashCode`, `toString`.  
- `@NoArgsConstructor` → no-arguments constructor.  
- `@AllArgsConstructor` → all-arguments constructor.  
- `@Builder` → builder pattern support.  

---

## 3. Naming Conventions
- **Class Names**: PascalCase (e.g., `UserDto`).  
- **Field Names**: camelCase.  
- **JSON snake_case keys** → convert to camelCase in Java.  
  - Example: `user_id` → `userId`.  
- **Class Naming for Nested Objects**: Use the field key in PascalCase with `Dto` suffix.  

---

## 4. JSON-to-Java Type Mapping
| JSON Type  | Java Type       | Notes                                      |
|------------|----------------|---------------------------------------------|
| string     | `String`       | Default mapping                             |
| number     | `Integer`      | Use `Long` if range requires it             |
| number.dec | `Double`       | Use `BigDecimal` if high precision needed   |
| boolean    | `Boolean`      | Wrapper, not primitive                      |
| object     | Nested DTO     | Generate a new class                        |
| array      | `List<T>`      | Use `java.util.List`                        |

---

## 5. Serialization Rules
If JSON keys differ from Java fields (e.g., `user_id` → `userId`), add:
```java
@JsonProperty("user_id")
private Integer userId;

---

## 6. DTO/POJO Rules
- DTOs should contain **data only** (no business logic).  
- All fields should be **private**.  
- Classes should be **public**.  
- Each nested JSON object → separate DTO class.  

---

## 7. Code Style & Formatting
- Use **4-space indentation**.  
- Group imports logically (Lombok → Java → Other).  
- Keep DTO class names **singular** (`UserDto`, not `UsersDto`).  
- Each class ideally in a **separate file**.  

---

## 8. Optional Enhancements
- **Validation Annotations** (if project uses `javax.validation`):  
  - `@NotNull`, `@Email`, `@Size`.  
- **Package Declarations**:  
  Add `package com.example.dto;` at top if needed.  
- **JPA/Database Integration**:  
  If required later, extend with `@Entity`, `@Table`, `@Column`.  

---

