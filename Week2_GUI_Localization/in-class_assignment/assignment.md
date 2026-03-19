# SEP2 / UI Localization  
## Week 2 / AD  

## Description of the Exercise  
**Fuel Consumption Calculator with Multi-Language Support**

This exercise demonstrates how to create a JavaFX-based fuel consumption calculator application that supports multiple languages (English, French, Japanese, and Persian). The application allows users to input distance and fuel values, calculate fuel consumption, and display the result in various languages based on user selection.

---

## 1. Main Components and Flow

### • FXML Layout
- The UI is constructed using an FXML file (JavaFX) that defines the structure of the application.  
- It contains:
  - Labels for distance and fuel inputs  
  - Text fields for user input  
  - A button for calculating fuel consumption  
  - A label for displaying the result  
- The layout also includes language selection buttons:
  - `EN`, `FR`, `JP`, `IR`  
- These allow users to dynamically change the UI language.

### • Controller Class (`HelloController`)
- Handles application logic and user interactions:
  - Clicking the **Calculate** button  
  - Selecting a language  
- Loads appropriate language resources  
- Updates UI elements with localized text  

---

## 2. User Interface Layout (`hello-view.fxml`)

The layout uses a `VBox` container and includes:

### Labels
- `lblDistance` → "Distance" field  
- `lblFuel` → "Fuel" field  
- `lblResult` → Displays result or error messages  

### Text Fields
- `txtDistance` → Input for distance  
- `txtFuel` → Input for fuel

### • Buttons
- `btnCalculate` → Triggers calculation  
- Language buttons:
  - `EN`, `FR`, `JP`, `IR`  

---

## 3. Controller Logic (`HelloController.java`)

### • Language Support
- `setLanguage()` loads language resources based on locale:
  - `en`, `fr`, `ja`, `fa`  
- Updates UI labels using `ResourceBundle`  
- Displays an error if a resource file is missing  

### • Fuel Consumption Calculation
- `onCalculateClick()`:
  - Retrieves distance and fuel input  
  - Calculates fuel consumption *(liters per 100 km)*  
  - Displays the result in `lblResult`  
- Handles invalid input:
  - Shows error message for non-numeric values  

---

## 4. Language Resource Files (`messages.properties`)

Uses `ResourceBundle` for localization.

### Required files:
- `messages_en_US.properties` (English)  
- `messages_fr_FR.properties` (French)  
- `messages_ja_JP.properties` (Japanese)  
- `messages_fa_IR.properties` (Persian)  

### Example key-value pairs:
```properties
distance.label=Distance
fuel.label=Fuel
calculate.button=Calculate
result.label=Fuel consumption: {0} L/100 km
invalid.input=Invalid input, please enter valid numbers.

```
5. Application Flow
• Initialization
- initialize() sets default language:

```java

setLanguage(new Locale("en"));

```
## Language Change

- Button handlers:
   - onENClick
   - onFRClick
   - onJPClick
   - onIRClick

- Updates UI language dynamically

## Calculation

1. User enters values in:

  - txtDistance
  - txtFuel

2. Clicks Calculate

3. onCalculateClick() executes calculation

4. Displays result or error message


## 6. Example Workflow
- User Input
   - Distance: 100
   - Fuel: 8

- Calculation

```
(8 / 100) * 100 = 8 L/100 km
```
## Result

- Output:
```
Fuel consumption: 8.00 L/100 km
````

- Language Change Example

   - Switching to French updates UI and result:
````
Consommation de carburant: 8,00 L/100 km
````

## Submission Method

1. GitHub repository link of the final solution

2. Screenshots of the solution with your name tag


---























