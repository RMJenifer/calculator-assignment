# RoqueMartinez-lesson2ai.md 

  

## First Prompt 

I want to create a calculator program in python using the procedural paradigm Initially, the feature will be for addition, subtraction, multiplication, and division. I will add other features such as present value, mortgage payments, etc. later so the design must easily allow this... 

  

### AI Feedback 

def add(x, y): 

    return x + y 

  

def subtract(x, y): 

    return x - y 

  

def multiply(x, y): 

    return x * y 

  

def divide(x, y): 

    if y == 0: 

        return "Error: Division by zero" 

    return x / y 

  

def calculator(): 

    print("Select operation:") 

    print("1. Add") 

    print("2. Subtract") 

    print("3. Multiply") 

    print("4. Divide") 

  

    choice = input("Enter choice (1/2/3/4): ") 

  

    if choice in ['1', '2', '3', '4']: 

        num1 = float(input("Enter first number: ")) 

        num2 = float(input("Enter second number: ")) 

  

        if choice == '1': 

            print(f"{num1} + {num2} = {add(num1, num2)}") 

        elif choice == '2': 

            print(f"{num1} - {num2} = {subtract(num1, num2)}") 

        elif choice == '3': 

            print(f"{num1} * {num2} = {multiply(num1, num2)}") 

        elif choice == '4': 

            print(f"{num1} / {num2} = {divide(num1, num2)}") 

    else: 

        print("Invalid input") 

  

if __name__ == "__main__": 

    calculator() 

 

Explanation: 

This is a simple calculator program that uses functions to perform basic arithmetic operations. 

The calculator() function is the main interface for user interaction. 

 

  

## Second Prompt 

Can unit tests using `pytest` be created to test the code easily with minimum mocking? 

  

### AI Feedback 

import pytest 

from calculator import add, subtract, multiply, divide 

  

def test_add(): 

    assert add(2, 3) == 5 

    assert add(-1, 1) == 0 

    assert add(0, 0) == 0 

  

def test_subtract(): 

    assert subtract(5, 3) == 2 

    assert subtract(0, 0) == 0 

    assert subtract(-1, -1) == 0 

  

def test_multiply(): 

    assert multiply(2, 3) == 6 

    assert multiply(-1, 1) == -1 

    assert multiply(0, 10) == 0 

  

def test_divide(): 

    assert divide(6, 3) == 2 

    assert divide(5, 2) == 2.5 

    assert divide(1, 0) == "Error: Division by zero" 

Explanation: 

This feedback provides unit tests for each of the calculator functions. 

The tests ensure the functions work correctly and handle edge cases, such as division by zero. 

 

  

## Third Prompt 

Does the code meet the following principles? If not, where and how do I refactor Code that meets SOLID principles as well as DRY, KISS, YAGNI, Law of Demeter, Composition over inheritance, Encapsulation, High Cohesion, Low Coupling, modular design, OOP principles. Code should be easy to maintain, easy to add features, and easy to test using `Pytest`.... 

  

### AI Feedback 

class Operation: 

    def execute(self, x, y): 

        pass 

  

class Add(Operation): 

    def execute(self, x, y): 

        return x + y 

  

class Subtract(Operation): 

    def execute(self, x, y): 

        return x - y 

  

class Multiply(Operation): 

    def execute(self, x, y): 

        return x * y 

  

class Divide(Operation): 

    def execute(self, x, y): 

        if y == 0: 

            return "Error: Division by zero" 

        return x / y 

  

class Calculator: 

    def __init__(self): 

        self.operations = { 

            '1': Add(), 

            '2': Subtract(), 

            '3': Multiply(), 

            '4': Divide() 

        } 

  

    def perform_operation(self, choice, num1, num2): 

        operation = self.operations.get(choice) 

        if operation: 

            return operation.execute(num1, num2) 

        else: 

            return "Invalid operation" 

  

def user_interface(): 

    calculator = Calculator() 

    print("Select operation:") 

    print("1. Add") 

    print("2. Subtract") 

    print("3. Multiply") 

    print("4. Divide") 

  

    choice = input("Enter choice (1/2/3/4): ") 

    num1 = float(input("Enter first number: ")) 

    num2 = float(input("Enter second number: ")) 

  

    result = calculator.perform_operation(choice, num1, num2) 

    print(f"Result: {result}") 

  

if __name__ == "__main__": 

    user_interface() 

  

Explanation: 

This feedback refactors the calculator code to adhere to SOLID principles, modular design, and OOP principles. 

The code is now more maintainable, extensible, and easier to test. 

 

 

## Fourth Prompt 

Add a new feature that would require a database or JSON file. Modularize the code into at least four modules to help in team development, testing, maintenance, and minimizing refactoring while adding features... 

  

### AI Feedback 

To add a new feature that requires a database or JSON file, let's consider adding a "history" feature that stores the results of calculations. We’ll use a JSON file to store the history. 

We’ll also modularize the code into four distinct modules: 

operations.py: Handles the arithmetic operations. 

calculator.py: Manages the calculator logic. 

history.py: Handles saving and retrieving calculation history using JSON. 

interface.py: Manages user interactions. 

1. operations.py 

This module contains the classes for the arithmetic operations. 

python 

Copy code 

class Operation: 
    def execute(self, x, y): 
        pass 
 
class Add(Operation): 
    def execute(self, x, y): 
        return x + y 
 
class Subtract(Operation): 
    def execute(self, x, y): 
        return x - y 
 
class Multiply(Operation): 
    def execute(self, x, y): 
        return x * y 
 
class Divide(Operation): 
    def execute(self, x, y): 
        if y == 0: 
            return "Error: Division by zero" 
        return x / y 
 

2. calculator.py 

This module manages the calculator logic and integrates with the operations and history. 

python 

Copy code 

from operations import Add, Subtract, Multiply, Divide 
from history import History 
 
class Calculator: 
    def __init__(self): 
        self.operations = { 
            '1': Add(), 
            '2': Subtract(), 
            '3': Multiply(), 
            '4': Divide() 
        } 
        self.history = History() 
 
    def perform_operation(self, choice, num1, num2): 
        operation = self.operations.get(choice) 
        if operation: 
            result = operation.execute(num1, num2) 
            self.history.save_history(num1, num2, choice, result) 
            return result 
        else: 
            return "Invalid operation" 
 
    def show_history(self): 
        return self.history.load_history() 
 

3. history.py 

This module handles saving and retrieving the calculation history using a JSON file. 

python 

Copy code 

import json 
 
class History: 
    def __init__(self, filename='history.json'): 
        self.filename = filename 
 
    def save_history(self, num1, num2, operation, result): 
        entry = { 
            'num1': num1, 
            'num2': num2, 
            'operation': operation, 
            'result': result 
        } 
        try: 
            with open(self.filename, 'r+') as file: 
                data = json.load(file) 
                data.append(entry) 
                file.seek(0) 
                json.dump(data, file) 
        except FileNotFoundError: 
            with open(self.filename, 'w') as file: 
                json.dump([entry], file) 
 
    def load_history(self): 
        try: 
            with open(self.filename, 'r') as file: 
                return json.load(file) 
        except FileNotFoundError: 
            return "No history found." 
 

4. interface.py 

This module manages the user interface and integrates with the calculator logic. 

python 

Copy code 

from calculator import Calculator 
 
def user_interface(): 
    calculator = Calculator() 
    print("Select operation:") 
    print("1. Add") 
    print("2. Subtract") 
    print("3. Multiply") 
    print("4. Divide") 
    print("5. Show History") 
 
    choice = input("Enter choice (1/2/3/4/5): ") 
 
    if choice == '5': 
        history = calculator.show_history() 
        print(f"History: {history}") 
    else: 
        num1 = float(input("Enter first number: ")) 
        num2 = float(input("Enter second number: ")) 
 
        result = calculator.perform_operation(choice, num1, num2) 
        print(f"Result: {result}") 
 
if __name__ == "__main__": 
    user_interface() 
 

Explanation: 

Modularization: The code is split into four modules (operations.py, calculator.py, history.py, interface.py) to allow for easier team development, testing, and maintenance. 

History Feature: A new feature to save and load calculation history using a JSON file is added. This is managed by the History class in the history.py module. 

JSON for Persistence: The history of calculations is stored in a JSON file, making it easy to persist data across sessions. 

  

## Fifth Prompt 

If I wanted to eventually turn this into a web application, what refactoring should I do to allow this in the future?... 

  

### AI Feedback 

To refactor the calculator program for future adaptation into a web application, you should consider the following steps: 

Separation of Concerns: 

Separate the core logic of the application (like operations and history) from the user interface entirely. The user_interface in interface.py should be decoupled from the core Calculator logic so that the UI can be replaced with a web-based interface in the future. 

API Layer: 

Create an API layer that will serve as the interface between the core logic and the web front end. This API could be built using frameworks like Flask or Django in Python. 

Stateless Design: 

Refactor the Calculator class and related modules to follow a stateless design pattern. In a web application, each request should be independent and not rely on any previous state stored in memory. 

Database Integration: 

Replace the JSON file-based history with a database solution (like SQLite, PostgreSQL, or MySQL) that scales better with web applications. This will allow better concurrency and data management. 

Error Handling: 

Enhance error handling to provide meaningful feedback in a web context. This includes converting error messages into appropriate HTTP response codes (e.g., 400 for bad requests). 

Security: 

Implement security measures to protect against common web application vulnerabilities such as SQL injection, cross-site scripting (XSS), and cross-site request forgery (CSRF). 

Testing with Web Framework: 

Adapt your unit tests to work with the web framework you choose. You might need to write additional tests for the API endpoints and integrate them with your existing Pytest suite. 

Refactored Code Example 

1. operations.py and 2. calculator.py would largely remain the same, focusing purely on business logic. 

3. api.py This new module will serve as the API layer, interfacing between the core logic and the web front end. 

python 

Copy code 

from flask import Flask, request, jsonify 
from calculator import Calculator 
 
app = Flask(__name__) 
calculator = Calculator() 
 
@app.route('/calculate', methods=['POST']) 
def calculate(): 
    data = request.json 
    operation = data.get('operation') 
    num1 = data.get('num1') 
    num2 = data.get('num2') 
     
    if not operation or num1 is None or num2 is None: 
        return jsonify({"error": "Invalid input"}), 400 
     
    result = calculator.perform_operation(operation, float(num1), float(num2)) 
     
    return jsonify({"result": result}) 
 
@app.route('/history', methods=['GET']) 
def history(): 
    return jsonify(calculator.show_history()) 
 
if __name__ == "__main__": 
    app.run(debug=True) 
 

Explanation: 

API Layer: The api.py module sets up an API with endpoints for performing calculations and retrieving history. This will serve as the backend logic for a web application. 

Flask Framework: The example uses Flask to demonstrate how the application could be adapted into a web service, with routes for different functionalities. 

Database Integration: Replace JSON with a database in history.py to handle multiple users and larger data sets. 

 

  

## Your Choice Prompt 

What are some tips for improving code readability and maintainability?" 

  

### AI Feedback 

Improving code readability and maintainability is crucial for long-term success and ease of collaboration. Here are some tips to help achieve that: 

Tips for Improving Code Readability and Maintainability 

Follow Consistent Coding Standards: 

Style Guide: Use a consistent style guide, such as PEP 8 for Python. This includes naming conventions, indentation, and spacing. 

Code Formatters: Utilize code formatters like black or autopep8 for Python to enforce consistent styling. 

Write Clear and Descriptive Comments: 

Docstrings: Use docstrings to describe the purpose of modules, classes, and functions. This helps other developers understand the intent and usage. 

Inline Comments: Use inline comments to explain complex logic or decisions, but avoid over-commenting. 

Choose Meaningful Names: 

Variables and Functions: Use descriptive names for variables and functions that convey their purpose. For example, use calculate_mortgage instead of calc for a function name. 

Avoid Abbreviations: Avoid overly short or ambiguous names. 

Modularize Your Code: 

Functions and Classes: Break down code into smaller, reusable functions and classes. Each function or class should have a single responsibility. 

Separation of Concerns: Keep different aspects of the application (e.g., logic, data handling, UI) in separate modules or files. 

Implement Error Handling: 

Try-Except Blocks: Use try-except blocks to handle potential errors gracefully. This can make your code more robust and easier to debug. 

Custom Exceptions: Define custom exceptions for specific error scenarios to provide clearer error messages. 

Use Version Control Effectively: 

Commit Messages: Write clear and informative commit messages that describe the changes made. This helps track the history of changes. 

Branching: Use branches for new features or bug fixes to keep the main codebase stable. 

Write Unit Tests: 

Coverage: Write unit tests to cover the core functionalities of your code. This ensures that changes do not introduce new bugs. 

Readability: Ensure that tests are also readable and provide meaningful descriptions of what they are testing. 

Refactor Regularly: 

Code Smells: Address code smells (e.g., long functions, duplicated code) during regular refactoring to keep the codebase clean. 

Continuous Improvement: Regularly review and improve the code structure and design. 

Document the Codebase: 

README Files: Include a README file with an overview of the project, setup instructions, and usage examples. 

Architecture Documentation: Document the overall architecture and design decisions to provide context for future developers. 

Encourage Code Reviews: 

Peer Review: Engage in code reviews with peers to get feedback and identify areas for improvement. 

Best Practices: Use code review as an opportunity to share best practices and learn from others. 

By following these tips, you can enhance the readability and maintainability of your code, making it easier to manage and extend over time. 

 
