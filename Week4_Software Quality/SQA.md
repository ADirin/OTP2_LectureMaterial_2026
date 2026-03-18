# OTP2 – Week 4: Statistical Code Analysis

**Fall 2025**  
**Instructor:** Amir Dirin  

> ⚠️ **Note:**  
> This material is intended to support face-to-face lectures.  
> Without attending lectures and demonstrations, this summary alone is **not sufficient** to pass the exam or complete assignments.

---

## 📌 1. Introduction to Clean Code in Software Engineering

Clean code refers to writing code that is:

- Easy to understand  
- Easy to modify  
- Easy to maintain  

Clean code is not only functional but also:
- Organized  
- Consistent  
- Expressive  

Following clean code principles helps create software that is:
- Less error-prone  
- Easier to debug  
- Adaptable to change  

---

## 🔍 1.1 Why Is Clean Code Important?

- **📖 Readability**  
  Easy to read and understand, especially in team environments.

- **🛠 Maintainability**  
  Easier to update and modify without breaking other parts.

- **📈 Scalability**  
  Supports adding new features with minimal rework.

- **💳 Reduced Technical Debt**  
  Avoids future problems caused by quick, poor decisions.

- **🐞 Improved Debugging and Testing**  
  Easier to find bugs and test code effectively.

---

## 🧠 1.2 Key Principles of Clean Code

- **Simplicity**  
  Avoid unnecessary complexity.

- **Naming Conventions**  
  Use meaningful and descriptive names.

- **DRY (Don't Repeat Yourself)**  
  Avoid duplicate code.

- **Single Responsibility Principle**  
  One function/class = one responsibility.

- **Consistency**  
  Follow consistent style and structure.

- **Formatting**  
  Use consistent indentation (Java: 4 spaces).

- **Comments & Documentation**  
  Explain *why*, not *what*.

- **Refactoring**  
  Continuously improve code quality.

---

## 🧰 2. Tools for Clean Code in Java

### 🔎 Code Linters
- **Checkstyle** – Enforces coding standards  
- **PMD** – Detects common issues (unused variables, etc.)  
- **SpotBugs** – Finds bugs in compiled code  

---

### 💻 IDE Plugins
- **IntelliJ Code Inspector** – Suggests improvements  
- **Eclipse Checkstyle Plugin** – Enforces style rules  

---

### 📊 Static Code Analysis
- **SonarQube**  
  - Detects bugs  
  - Measures code quality  
  - Reports complexity & duplication  

---

### 🧪 Testing Frameworks
- **JUnit** – Unit testing  
- **Mockito** – Mocking dependencies  

---

### 🔌 Dependency Injection
- **Spring / Guice**  
  Helps create modular, testable applications  

---

## 📘 3. Clean Code in Theory

Based on:  
*Working Effectively with Legacy Code (Martin & Feathers, 2009)*

This section connects:
- Clean code principles  
- Developer Experience (DX)  
- Real-world software practices  

---

## ⚖️ 3.1 Good Code vs. Bad Code

- **✅ Good Code**
  - Clean  
  - Readable  
  - Maintainable  
  - Scalable  

- **❌ Bad Code ("Spaghetti Code")**
  - Disorganized  
  - Hard to maintain  
  - Error-prone  

---

## 💸 3.2 The Cost of Bad Code

Bad code leads to:
- Slower development  
- More bugs  
- Higher costs  
- Poor team productivity  

---

## 🧩 4. Core Clean Code Principles

---

### 🏷 4.1 Naming

Good naming is critical:

- Reflect purpose (e.g., `isUserLoggedIn`)  
- Avoid misleading terms  
- Be consistent  
- Be descriptive but not excessive  

---

### ⚙️ 4.2 Functions

- Keep functions **small**
- Do **one thing only**
- Use **descriptive names**

✔ Improves readability and maintainability  

---

### 💬 4.3 Comments

- Write comments only when necessary  
- Avoid obvious comments  
- Keep them updated  

---

### 📐 4.4 Formatting and Indentation

- Improves readability  
- Enhances developer experience  

#### Vertical Formatting
- Structure like an article  
- High-level → details  

#### Horizontal Formatting
- Keep lines short  
- Improve readability across screens  

---

## 🏁 Summary

Clean code leads to:

- Better collaboration  
- Easier maintenance  
- Higher quality software  
- Reduced long-term costs  

---

## 🏷 Tags

`#CleanCode` `#Java` `#SoftwareEngineering` `#CodeQuality` `#StaticAnalysis` `#Refactoring` `#BestPractices` `#OTP2`
