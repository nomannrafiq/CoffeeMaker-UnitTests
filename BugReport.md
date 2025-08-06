CoffeeMaker Final Bug Report
Date: August 7, 2025
Author: [Your Name Here]
Project: CoffeeMaker Unit Testing Assignment
Objective: To identify and document bugs in the provided CoffeeMaker implementation using JUnit unit tests.

1. Executive Summary
A comprehensive suite of unit tests was developed and executed against the CoffeeMaker application using the Gradle build tool. The testing process successfully identified a total of nine bugs across various functionalities, including recipe management, inventory control, and the coffee-making process. The identified issues range from improper input validation to incorrect state management and flawed business logic.

2. Testing Methodology
The testing was conducted using the JUnit 4 framework. A CoffeeMakerTest.java file was created, containing multiple test methods (@Test) to cover the core functionality and edge cases of the CoffeeMaker class. The @Before annotation was used to establish a clean state for each test, ensuring test independence. Tests were run via the Gradle command-line tool, and failures were analyzed using the generated HTML test report.

3. Detailed Bug Findings by Module
3.1 Recipe Module Bugs
Bug #1: addRecipe Method Does Not Validate null Input

Test Case: testAddNullRecipe

Description: The addRecipe method lacks a check for null input. When a null Recipe object is passed, the program crashes with a java.lang.NullPointerException, failing to handle the invalid input gracefully.

Recommendation: Implement a null check at the start of the method to prevent this crash and return a boolean false.

Bug #2: addRecipe Method Fails to Handle a Full RecipeBook

Test Case: testAddRecipeWhenFull

Description: The RecipeBook has a capacity of three recipes. When a fourth recipe is added, the addRecipe method returns true, suggesting a successful operation. However, the recipe is not added to the collection, creating a false-positive result.

Recommendation: The method should check if the maximum capacity has been reached and return false if it is.

Bug #3: deleteRecipe Method Fails with Invalid Index Values

Test Case: testDeleteRecipeInvalidIndex

Description: The deleteRecipe method does not validate its index parameter. Supplying a negative index, such as -1, causes an java.lang.ArrayIndexOutOfBoundsException and a program crash.

Recommendation: Add a check to ensure the index is within the valid bounds of the recipe array before attempting to access an element.

Bug #4: deleteRecipe Method Returns Incorrect Value

Test Case: testDeleteRecipe

Description: After a recipe is successfully deleted, the deleteRecipe method is expected to return the name of the removed recipe. The test revealed that the method returns an incorrect value, indicating a bug in its return logic.

Recommendation: The method should be corrected to properly return the name of the recipe that was just deleted.

3.2 Inventory Module Bugs
Bug #5: addInventory Method Fails to Update Inventory Correctly

Test Case: testAddInventoryValid

Description: The addInventory method is not correctly adding new quantities to the existing inventory. The test showed that adding 10 units of each ingredient to an initial 15 did not result in the expected total of 25.

Recommendation: The method's logic for parsing string inputs and adding them to the current inventory should be fixed.

3.3 CoffeeMaker Module Bugs
Bug #6: makeCoffee Method Fails to Deduct Inventory with Exact Payment

Test Case: testMakeCoffeeWithExactFunds

Description: When a coffee is successfully purchased with the exact amount of money, the makeCoffee method fails to deduct the necessary ingredients from the inventory. This leads to a scenario where inventory levels remain unchanged even after a beverage is dispensed.

Recommendation: The makeCoffee method should correctly call the Inventory.useIngredients method to update ingredient levels after a successful transaction.

Bug #7: makeCoffee Method Fails to Deduct Inventory with Overpayment

Test Case: testMakeCoffeeWithMoreThanEnoughFunds

Description: When a coffee is purchased with more than the required amount of money, the makeCoffee method fails to deduct the necessary ingredients from the inventory. While the correct change is returned, the inventory levels remain unchanged.

Recommendation: The makeCoffee method should correctly deduct the ingredients from the inventory when a successful purchase is made, regardless of the payment amount.

Bug #8: makeCoffee Method Fails on Insufficient Inventory

Test Case: testMakeCoffeeWithInsufficientInventory

Description: The makeCoffee method's error handling for insufficient inventory is flawed. Although the correct amount of change is returned to the user, the inventory is not reset to its original state, indicating that the useIngredients method might be called or handled incorrectly.

Recommendation: The method should ensure that if a purchase fails due to insufficient inventory, no ingredients are deducted.

Bug #9: makeCoffee Method Fails with Zero-Amount Ingredients

Test Case: testMakeCoffeeWithZeroIngredient

Description: When a recipe requires zero units of a specific ingredient (e.g., coffee for hot chocolate), the makeCoffee method's inventory deduction process fails. The test reveals that the other required ingredients are not properly deducted, leading to an incorrect final inventory state.

Recommendation: The method should correctly handle recipes with zero-value ingredient requirements, ensuring all other required ingredients are properly deducted.