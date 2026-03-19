# JavaFX UI Orientation Guide
### Switching Between LTR and Right-to-Left (RTL)
> Based on `BMIController.java` — Student Reference

---

## Table of Contents

- [Overview](#overview)
- [1. Project Structure Overview](#1-project-structure-overview)
- [2. How applyTextDirection() Works](#2-how-applytextdirection-works)
- [3. Step-by-Step Breakdown](#3-step-by-step-breakdown)
  - [Step 1 — Detect the language direction](#step-1--detect-the-language-direction)
  - [Step 2 — Wrap UI changes in Platform.runLater()](#step-2--wrap-ui-changes-in-platformrunlater)
  - [Step 3 — Set NodeOrientation on the root VBox](#step-3--set-nodeorientation-on-the-root-vbox)
  - [Step 4 — Align text inside the TextFields](#step-4--align-text-inside-the-textfields)
- [4. Wiring the Language Buttons](#4-wiring-the-language-buttons)
- [5. Language Codes Reference](#5-language-codes-reference)
- [6. Common Mistakes](#6-common-mistakes)
- [7. Quick Reference Card](#7-quick-reference-card)

---

## Overview

This guide explains how to change the layout direction of a JavaFX application between **Left-to-Right (LTR)** and **Right-to-Left (RTL)** at runtime. The examples are taken directly from `BMIController.java`, a BMI calculator that supports English, French, Vietnamese, Urdu, and Persian.

| Language group | Examples | Direction |
|---|---|---|
| LTR | English, French, Vietnamese | ← Left to Right |
| RTL | Persian, Urdu, Arabic, Hebrew | Right to Left → |

JavaFX handles both directions through a single property: **`NodeOrientation`**.

> [!NOTE]
> Setting `NodeOrientation` on the **root** node automatically propagates the direction to **every child node** in the layout. You never need to set it on individual labels, buttons, or fields.

---

## 1. Project Structure Overview

`BMIController.java` uses the **FXML pattern**. The root layout node is injected by the FXML loader via the `@FXML` annotation. The controller then manipulates it in code.
```java
// Fields injected from the FXML file
@FXML private VBox      rootVBox;       // <-- root layout node
@FXML private Label     lblWeight;
@FXML private Label     lblHeight;
@FXML private TextField tfWeight;
@FXML private TextField tfHeight;
@FXML private Button    btnCalculate;
@FXML private Label     lblResult;
@FXML private Label     lblLocalTime;
```

> [!IMPORTANT]
> In your `.fxml` file the root VBox **must** have `fx:id="rootVBox"` so the FXML loader can inject it.
>
> ```xml
> <VBox fx:id="rootVBox"
>       xmlns="http://javafx.com/javafx"
>       fx:controller="org.example.demo1.BMIController">
> ```
>
> Without this attribute `rootVBox` will be `null` at runtime and the orientation code will silently do nothing.

---

## 2. How applyTextDirection() Works

The entire orientation logic lives in one private method. Here it is exactly as written in `BMIController.java`:
```java
private void applyTextDirection(Locale locale) {

    // Step 1: Check if the language code belongs to an RTL language
    String lang = locale.getLanguage();
    boolean isRTL = lang.equals("fa")   // Persian
                 || lang.equals("ur")   // Urdu
                 || lang.equals("ar")   // Arabic
                 || lang.equals("he");  // Hebrew

    // Step 2: Wrap all UI changes in Platform.runLater()
    Platform.runLater(() -> {

        // Step 3: Flip the entire layout direction on the root node
        if (rootVBox != null) {
            rootVBox.setNodeOrientation(
                isRTL ? NodeOrientation.RIGHT_TO_LEFT
                      : NodeOrientation.LEFT_TO_RIGHT
            );
        }

        // Step 4: Align text inside the input fields
        tfWeight.setStyle(isRTL ? "-fx-text-alignment: right;"
                                : "-fx-text-alignment: left;");
        tfHeight.setStyle(isRTL ? "-fx-text-alignment: right;"
                                : "-fx-text-alignment: left;");
    });
}
```

---

## 3. Step-by-Step Breakdown

### Step 1 — Detect the language direction

Get the **ISO 639-1** language code from the `Locale` and check whether it belongs to an RTL language.
```java
String lang = locale.getLanguage();

// getLanguage() returns the two-letter ISO code, for example:
//   "en"  →  English    (LTR)
//   "fr"  →  French     (LTR)
//   "vi"  →  Vietnamese (LTR)
//   "fa"  →  Persian    (RTL)
//   "ur"  →  Urdu       (RTL)
//   "ar"  →  Arabic     (RTL)
//   "he"  →  Hebrew     (RTL)

boolean isRTL = lang.equals("fa")
             || lang.equals("ur")
             || lang.equals("ar")
             || lang.equals("he");
```

> [!TIP]
> To support additional RTL languages, add more conditions. For example, to add Yiddish:
> ```java
> boolean isRTL = lang.equals("fa")
>              || lang.equals("ur")
>              || lang.equals("ar")
>              || lang.equals("he")
>              || lang.equals("yi");  // Yiddish
> ```
> Always use the two-letter code returned by `Locale.getLanguage()`.

<details>
<summary>❌ Wrong ways to check for RTL</summary>
```java
// ❌ Wrong — depends on the JVM's own locale setting
locale.getDisplayLanguage().equals("Persian")

// ❌ Wrong — fragile, breaks if country code changes
locale.toString().equals("fa_IR")

// ✅ Correct — ISO 639-1 code, always reliable
locale.getLanguage().equals("fa")
```

</details>

---

### Step 2 — Wrap UI changes in Platform.runLater()

JavaFX requires all scene graph changes to happen on the **JavaFX Application Thread**. `Platform.runLater()` guarantees this.
```java
Platform.runLater(() -> {
    // All scene graph changes go inside here
});
```

> [!WARNING]
> `setLanguage()` is usually called from button-click event handlers, which already run on the JavaFX thread — so `Platform.runLater()` is technically optional in those cases.
>
> However, if `setLanguage()` is ever called from a **background thread** (e.g. after loading data from a database), the UI update **must** be wrapped in `Platform.runLater()`.
>
> Using it consistently is a safe habit that prevents hard-to-find threading bugs.

---

### Step 3 — Set NodeOrientation on the root VBox

One call on the root node flips the direction of **every child node** in the entire layout.
```java
if (rootVBox != null) {
    rootVBox.setNodeOrientation(
        isRTL ? NodeOrientation.RIGHT_TO_LEFT
              : NodeOrientation.LEFT_TO_RIGHT
    );
}
```

**What changes when you flip the orientation:**

| Element | LEFT_TO_RIGHT (LTR) | RIGHT_TO_LEFT (RTL) |
|---|---|---|
| Labels | Align to the left | Align to the right |
| Buttons | Stack from the left | Stack from the right |
| HBox children | Flow left → right | Flow right → left |
| Input cursor | Starts at left edge | Starts at right edge |
| Scrollbar | Appears on the right | Appears on the left |

> [!NOTE]
> The `null` check (`if (rootVBox != null)`) prevents a crash if the `fx:id` is missing from the FXML file. However it also silently does nothing in that case — always verify the `fx:id` is present.

---

### Step 4 — Align text inside the TextFields

`NodeOrientation` moves the cursor to the correct edge, but `-fx-text-alignment` **explicitly aligns the typed characters** inside the field.
```java
// RTL language: typed text appears on the right
tfWeight.setStyle("-fx-text-alignment: right;");
tfHeight.setStyle("-fx-text-alignment: right;");

// LTR language: typed text appears on the left
tfWeight.setStyle("-fx-text-alignment: left;");
tfHeight.setStyle("-fx-text-alignment: left;");
```

<details>
<summary>📌 NodeOrientation vs -fx-text-alignment — what's the difference?</summary>

| Property | What it controls |
|---|---|
| `NodeOrientation` | Overall layout direction of a container and all its children |
| `-fx-text-alignment` | Where text sits horizontally inside a single `TextField` |

Both are needed together:
- `NodeOrientation` moves the **cursor edge** (right vs left side of the field)
- `-fx-text-alignment` aligns the **characters** you type inside the field

</details>

---

## 4. Wiring the Language Buttons

Each language button in `BMIController.java` calls `setLanguage()` with the matching `Locale`. `setLanguage()` updates all the labels and then calls `applyTextDirection()` at the end.
```java
// Button handlers — each passes a different Locale
public void onENClick(ActionEvent e) { setLanguage(new Locale("en", "US")); }
public void onFRClick(ActionEvent e) { setLanguage(new Locale("fr", "FR")); }
public void onVIClick(ActionEvent e) { setLanguage(new Locale("vi", "VI")); }
public void onURClick(ActionEvent e) { setLanguage(new Locale("ur", "PA")); }
public void onFAClick(ActionEvent e) { setLanguage(new Locale("fa", "IR")); }
```
```java
private void setLanguage(Locale locale) {
    currentLocale = locale;   // 1. Store the new locale

    lblResult.setText("");    // 2. Clear the previous result

    // 3. Load localized strings from LocalizationService
    localizedStrings = LocalizationService.getLocalizedStrings(locale);

    // 4. Update all UI labels
    lblWeight.setText(localizedStrings.getOrDefault("weight", "Weight"));
    lblHeight.setText(localizedStrings.getOrDefault("height", "Height"));
    btnCalculate.setText(localizedStrings.getOrDefault("calculate", "Calculate"));

    // 5. Update the local time display
    displayLocalTime(locale);

    // 6. Apply RTL or LTR layout — always called last
    applyTextDirection(locale);
}
```

> [!IMPORTANT]
> `applyTextDirection()` must be called **after** all label texts have been set. This ensures the layout engine recalculates sizes with the correct content before flipping the direction.

---

## 5. Language Codes Reference

All five languages from `BMIController.java` with their locale constructors and direction:

| Language | `new Locale(...)` | Direction | Button handler |
|---|---|---|---|
| English | `"en", "US"` | ← LTR | `onENClick()` |
| French | `"fr", "FR"` | ← LTR | `onFRClick()` |
| Vietnamese | `"vi", "VI"` | ← LTR | `onVIClick()` |
| Urdu | `"ur", "PA"` | RTL → | `onURClick()` |
| Persian | `"fa", "IR"` | RTL → | `onFAClick()` |

---

## 6. Common Mistakes

> [!CAUTION]
> **Mistake 1 — `rootVBox` is `null` at runtime**
>
> **Cause:** the FXML file is missing `fx:id="rootVBox"` on the root element.
>
> **Fix:** open your `.fxml` file and add `fx:id="rootVBox"` to the root `VBox` tag:
> ```xml
> <VBox fx:id="rootVBox" ...>
> ```
> The `null` check in the code prevents a crash but silently does nothing — always verify the `fx:id` is present.

---

> [!CAUTION]
> **Mistake 2 — Not calling `applyTextDirection()` after `setLanguage()`**
>
> If `applyTextDirection()` is not called at the end of `setLanguage()`, the labels will update to the new language but the layout direction will remain unchanged.
>
> Always call `applyTextDirection(locale)` as the **last line** in `setLanguage()`.

---

> [!CAUTION]
> **Mistake 3 — Not storing `currentLocale` before using it in `onCalculateClick()`**
>
> `BMIController.java` stores the active locale at the start of `setLanguage()`:
> ```java
> currentLocale = locale;
> ```
> This allows `onCalculateClick()` to read the active language when saving results to the database. Skipping this assignment means `onCalculateClick()` always uses the default locale.

---

> [!CAUTION]
> **Mistake 4 — Checking the wrong property for RTL**
>
> ```java
> // ❌ Wrong — result depends on JVM's own locale
> locale.getDisplayLanguage().equals("Persian")
>
> // ❌ Wrong — fragile string match
> locale.toString().equals("fa_IR")
>
> // ✅ Correct — ISO 639-1 code, always reliable
> locale.getLanguage().equals("fa")
> ```

---

## 7. Quick Reference Card

<details>
<summary>Click to expand the full quick reference</summary>

| Task | Code |
|---|---|
| Import `NodeOrientation` | `import javafx.geometry.NodeOrientation;` |
| Import `Platform` | `import javafx.application.Platform;` |
| Get language code | `locale.getLanguage()` |
| Check if RTL (Persian) | `lang.equals("fa")` |
| Check if RTL (Urdu) | `lang.equals("ur")` |
| Check if RTL (Arabic) | `lang.equals("ar")` |
| Apply RTL to root | `rootVBox.setNodeOrientation(NodeOrientation.RIGHT_TO_LEFT)` |
| Apply LTR to root | `rootVBox.setNodeOrientation(NodeOrientation.LEFT_TO_RIGHT)` |
| Align field text right | `tf.setStyle("-fx-text-alignment: right;")` |
| Align field text left | `tf.setStyle("-fx-text-alignment: left;")` |
| Thread-safe UI update | `Platform.runLater(() -> { ... })` |

</details>

---

## Summary

The four things `applyTextDirection()` does:

1. Gets the ISO language code with `locale.getLanguage()`
2. Decides `isRTL` by checking the code against known RTL languages (`fa`, `ur`, `ar`, `he`)
3. Calls `rootVBox.setNodeOrientation()` to flip the **entire layout** with one call
4. Calls `tf.setStyle("-fx-text-alignment: ...")` to align text inside each input field

That is all that is needed. JavaFX propagates the orientation automatically from the root node down to every label, button, and input field in the scene graph.

---

*JavaFX Internationalization — Student Reference*
