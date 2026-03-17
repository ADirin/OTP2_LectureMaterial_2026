# OTP2 / Database Localization Assignment – Fall 2025  
## Week 3 / AD  

## 📌 Assignment Overview
This is a **mandatory in-class assignment**.  

You are required to submit:
- Screenshots of the application in execution  
- The project’s GitHub repository link  

⚠️ **Note:**  
The use of any AI-based tools in completing this assignment is strictly prohibited. Any violation will result in a score of zero.

---

## 🔁 Continuation Note
This assignment continues from last week’s task.  

If not completed:
- Clone the previous assignment from last week’s in-class folder  
- Continue with the instructions below  

---

## ✅ Summary of Tasks

1. Read GUI captions from `LocalizationService` instead of `ResourceBundle`  
2. Save BMI calculations into the `bmi_results` table using `BMIResultService`  

---

## 🗄️ 1. Database Setup

```sql
CREATE DATABASE IF NOT EXISTS bmi_localization 
CHARACTER SET utf8mb4 
COLLATE utf8mb4_unicode_ci;

USE bmi_localization;

-- Stores BMI calculation results
CREATE TABLE IF NOT EXISTS bmi_results (
    id INT AUTO_INCREMENT PRIMARY KEY,
    weight DOUBLE NOT NULL,
    height DOUBLE NOT NULL,
    bmi DOUBLE NOT NULL,
    language VARCHAR(10),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Stores localized UI text
CREATE TABLE IF NOT EXISTS localization_strings (
    id INT AUTO_INCREMENT PRIMARY KEY,
    `key` VARCHAR(100) NOT NULL,
    value VARCHAR(255) NOT NULL,
    language VARCHAR(10) NOT NULL
);

```
## 🌍 2. Populate Localization Data
**English**
```sql
INSERT INTO localization_strings (`key`, value, language) VALUES
('weight', 'Weight (kg)', 'en'),
('height', 'Height (cm)', 'en'),
('calculate', 'Calculate BMI', 'en'),
('result', 'Your BMI is', 'en'),
('invalid', 'Invalid input', 'en'),
('localTime', 'Local Time', 'en');

```
**French**
```sql
INSERT INTO localization_strings (`key`, value, language) VALUES
('weight', 'Poids (kg)', 'fr'),
('height', 'Taille (cm)', 'fr'),
('calculate', 'Calculer IMC', 'fr'),
('result', 'Votre IMC est', 'fr'),
('invalid', 'Entrée invalide', 'fr'),
('localTime', 'Heure locale', 'fr');

```
**Vietnamese**
```sql
INSERT INTO localization_strings (`key`, value, language) VALUES
('weight', 'Cân nặng (kg)', 'vi'),
('height', 'Chiều cao (cm)', 'vi'),
('calculate', 'Tính BMI', 'vi'),
('result', 'Chỉ số BMI của bạn là', 'vi'),
('invalid', 'Dữ liệu không hợp lệ', 'vi'),
('localTime', 'Giờ địa phương', 'vi');

```
**Urdu (UTF-8 Required)**
```sql
INSERT INTO localization_strings (`key`, value, language) VALUES
('weight', 'وزن (کلوگرام)', 'ur'),
('height', 'قد (سینٹی میٹر)', 'ur'),
('calculate', 'BMI کریں حساب', 'ur'),
('result', 'آپ کا BMI ہے', 'ur'),
('invalid', 'غلط ان پٹ', 'ur'),
('localTime', 'مقامی وقت', 'ur');

```
## 💾 3. BMIResultService Class
```java
package org.example.demo1;

import java.sql.*;

public class BMIResultService {
    private static final String DB_URL = "jdbc:mysql://localhost:3306/bmi_localization";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "Test12";

    public static void saveResult(double weight, double height, double bmi, String language) {
        String query = "INSERT INTO bmi_results (weight, height, bmi, language) VALUES (?, ?, ?, ?)";

        try (Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);
             PreparedStatement stmt = conn.prepareStatement(query)) {

            stmt.setDouble(1, weight);
            stmt.setDouble(2, height);
            stmt.setDouble(3, bmi);
            stmt.setString(4, language);

            stmt.executeUpdate();
            System.out.println("BMI result saved to database.");

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```
## 🌐 4. LocalizationService Class

```java
package org.example.demo1;

import java.sql.*;
import java.util.HashMap;
import java.util.Locale;
import java.util.Map;

public class LocalizationService {

    private static final String DB_URL = "jdbc:mysql://localhost:3306/bmi_localization";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "Test12";

    public static Map<String, String> getLocalizedStrings(Locale locale) {
        Map<String, String> strings = new HashMap<>();
        String lang = locale.getLanguage();

        try (Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD)) {

            String query = "SELECT `key`, value FROM localization_strings WHERE language = ?";
            try (PreparedStatement stmt = conn.prepareStatement(query)) {
                stmt.setString(1, lang);
                ResultSet rs = stmt.executeQuery();

                while (rs.next()) {
                    strings.put(rs.getString("key"), rs.getString("value"));
                }
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }

        return strings;
    }
}

```
## 🧠 5. Update BMIController
### Add field:
```java
private Map<String, String> localizedStrings;

```
## 🔧 6. Modify Controller Methods
### Language Setup
```java
private void setLanguage(Locale locale) {
    lblResult.setText("");
    localizedStrings = LocalizationService.getLocalizedStrings(locale);

    lblWeight.setText(localizedStrings.getOrDefault("weight", "Weight"));
    lblHeight.setText(localizedStrings.getOrDefault("height", "Height"));
    btnCalculate.setText(localizedStrings.getOrDefault("calculate", "Calculate"));

    displayLocalTime(locale);
}


```

### BMI Calculation
```java
public void onCalculateClick(ActionEvent actionEvent) {
    try {
        double weight = Double.parseDouble(tfWeight.getText());
        double height = Double.parseDouble(tfHeight.getText()) / 100.0;
        double bmi = weight / (height * height);

        DecimalFormat df = new DecimalFormat("#0.00");
        lblResult.setText(
            localizedStrings.getOrDefault("result", "Your BMI is") 
            + " " + df.format(bmi)
        );

        // Save to database
        String language = Locale.getDefault().getLanguage();
        BMIResultService.saveResult(weight, height, bmi, language);

    } catch (NumberFormatException e) {
        lblResult.setText(localizedStrings.getOrDefault("invalid", "Invalid input"));
    }
}


```

## 🕒 7. Update Local Time Label
```java
lblLocalTime.setText(
    localizedStrings.getOrDefault("localTime", "Local Time") 
    + "\n" + formattedTime
);

```

## 📤 8. Submission Requirements

 - Screenshots of the bmi_results table records
 - Screenshots of the user interface
 - GitHub repository link of the project


 
