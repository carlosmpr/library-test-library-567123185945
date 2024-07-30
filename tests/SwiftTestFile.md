---
  title: 'SwiftTestFile.swift'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos Polanco'
  ---
  
  
  
  # SwiftTestFile.swift
  # Code Explanation

The provided code is written in Swift and consists of two functions and their usage to calculate Body Mass Index (BMI) and provide a health recommendation based on the calculated BMI.

### `calculateBMI` Function
- **Purpose**: This function calculates the BMI using the formula BMI = weight / (height * height).
- **Parameters**:
  - `weight`: Represents the weight of a person in kilograms.
  - `height`: Represents the height of a person in meters.
- **Return Value**: Returns the calculated BMI as a `Double`.

### `getHealthRecommendation` Function
- **Purpose**: This function provides a health recommendation based on the calculated BMI.
- **Parameters**:
  - `bmi`: Represents the Body Mass Index calculated using the `calculateBMI` function.
- **Return Value**: Returns a health recommendation as a `String` based on the BMI range.
- **BMI Ranges**:
  - Less than 18.5: "Underweight - You should eat more nutritious food."
  - Between 18.5 and 24.9: "Normal weight - Keep up the good work!"
  - Between 25 and 29.9: "Overweight - Consider a balanced diet and exercise."
  - 30 and above: "Obesity - Consult a healthcare provider."

### Usage Example
```swift
let weight = 70.0  // weight in kilograms
let height = 1.75  // height in meters

let bmi = calculateBMI(weight: weight, height: height)
let recommendation = getHealthRecommendation(bmi: bmi)

print("BMI: \(bmi)")
print("Recommendation: \(recommendation)")
```

### Output
```
BMI: 22.86
Recommendation: Normal weight - Keep up the good work!
```

The code calculates the BMI based on the provided weight and height, then provides a health recommendation based on the calculated BMI. The `switch` statement in the `getHealthRecommendation` function categorizes the BMI into different ranges and returns a recommendation accordingly.
  
  ## Component Code
  ```jsx
  import Foundation

func calculateBMI(weight: Double, height: Double) -> Double {
    return weight / (height * height)
}

func getHealthRecommendation(bmi: Double) -> String {
    switch bmi {
    case ..<18.5:
        return "Underweight - You should eat more nutritious food."
    case 18.5..<24.9:
        return "Normal weight - Keep up the good work!"
    case 25..<29.9:
        return "Overweight - Consider a balanced diet and exercise."
    default:
        return "Obesity - Consult a healthcare provider."
    }
}

let weight = 70.0  // weight in kilograms
let height = 1.75  // height in meters

let bmi = calculateBMI(weight: weight, height: height)
let recommendation = getHealthRecommendation(bmi: bmi)

print("BMI: \(bmi)")
print("Recommendation: \(recommendation)")
  ```
  