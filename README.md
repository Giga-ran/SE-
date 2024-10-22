# SE-README.md
Calculator Black box
code

     public class Calculator {
      // Method to add two numbers
    public double add(double num1, double num2) {
        return num1 + num2;
            }
    // Method to multiply two numbers
    public double multiply(double num1, double num2) {
        return num1 * num2;
    }

    // Method to divide two numbers
    public double divide(double num1, double num2) throws ArithmeticException {
        if (num2 == 0) {
            throw new ArithmeticException("Division by zero is not allowed");
        }
        return num1 / num2;
    }

    // Method to find the square root of a number
    public double squareRoot(double num) throws IllegalArgumentException {
        if (num < 0) {
            throw new IllegalArgumentException("Square root of negative numbers is not allowed");
        }
        return Math.sqrt(num);
     }
    }


test case

    import static org.junit.jupiter.api.Assertions.*;
    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.Test;

    public class CalculatorTest {
    private Calculator calculator;

    @BeforeEach
    public void setUp() {
        calculator = new Calculator();
    }

    // 1. Test case for adding two positive numbers
    @Test
    public void testAddTwoPositiveNumbers() {
        double result = calculator.add(5, 10);
        assertEquals(15, result, "Addition of 5 and 10 should be 15");
    }

    // 2. Test case for multiplying two numbers
    @Test
    public void testMultiplyTwoNumbers() {
        double result = calculator.multiply(4, 5);
        assertEquals(20, result, "Multiplication of 4 and 5 should be 20");
    }

    // 3. Test case for division by zero
    @Test
    public void testDivisionByZero() {
        Exception exception = assertThrows(ArithmeticException.class, () -> {
            calculator.divide(10, 0);
        });
        assertEquals("Division by zero is not allowed", exception.getMessage());
    }

    // 4. Test case for square root of a positive number
    @Test
    public void testSquareRootOfPositiveNumber() {
        double result = calculator.squareRoot(16);
        assertEquals(4, result, "Square root of 16 should be 4");
    }

    // Additional case: Test square root of a negative number
    @Test
    public void testSquareRootOfNegativeNumber() {
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            calculator.squareRoot(-9);
        });
        assertEquals("Square root of negative numbers is not allowed", exception.getMessage());
    }
    }
    
Black Box Unit Converter

    public class UnitConverter {

    // Method for converting meters to kilometers
    public double convertMetersToKilometers(double meters) {
        if (meters < 0) {
            throw new IllegalArgumentException("Input cannot be negative");
        }
        return meters / 1000;
    }

    // Method for converting Celsius to Fahrenheit
    public double convertCelsiusToFahrenheit(double celsius) {
        return (celsius * 9/5) + 32;
    }

    // Method for invalid (non-numeric) input handling
    public double parseInput(String input) {
        try {
            return Double.parseDouble(input);
        } catch (NumberFormatException e) {
            throw new IllegalArgumentException("Input is not a valid number");
        }
      }
    }
test case

    import org.junit.jupiter.api.Test;
    import static org.junit.jupiter.api.Assertions.*;

    public class UnitConverterTest {

    UnitConverter unitConverter = new UnitConverter();

    // 1. Test for Length Conversion (Meters to Kilometers)
    @Test
    public void testConvertMetersToKilometers() {
        // Valid input case: Convert 1500 meters to kilometers
        assertEquals(1.5, unitConverter.convertMetersToKilometers(1500), 0.001);

        // Valid input case: Convert 0 meters to kilometers
        assertEquals(0.0, unitConverter.convertMetersToKilometers(0), 0.001);
    }

    // 2. Test for Temperature Conversion (Celsius to Fahrenheit)
    @Test
    public void testConvertCelsiusToFahrenheit() {
        // Valid input case: Convert 0°C to °F (should return 32°F)
        assertEquals(32.0, unitConverter.convertCelsiusToFahrenheit(0), 0.001);

        // Valid input case: Convert 100°C to °F (should return 212°F)
        assertEquals(212.0, unitConverter.convertCelsiusToFahrenheit(100), 0.001);
    }

    // 3. Test for Invalid Input (Negative Value) - Length Conversion
    @Test
    public void testNegativeInputLengthConversion() {
        // Invalid input case: Negative meters value
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            unitConverter.convertMetersToKilometers(-1000);
        });
        assertEquals("Input cannot be negative", exception.getMessage());
    }

    // 4. Test for Invalid Input (Non-numeric Value)
    @Test
    public void testNonNumericInput() {
        // Invalid input case: Non-numeric value for conversion
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            unitConverter.parseInput("invalid");
        });
        assertEquals("Input is not a valid number", exception.getMessage());
    }
    }
Recipe managee
code

    import java.util.HashMap;
    import java.util.Map;

    public class RecipeManager {

    // Recipe class to store a recipe
    public static class Recipe {
        String name;
        String ingredients;

        public Recipe(String name, String ingredients) {
            this.name = name;
            this.ingredients = ingredients;
        }
        
        // Getters and Setters
        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public String getIngredients() {
            return ingredients;
        }

        public void setIngredients(String ingredients) {
            this.ingredients = ingredients;
        }
    }

    // Storage for recipes (Map with recipe name as key and recipe object as value)
    private Map<String, Recipe> recipeBook = new HashMap<>();

    // 1. Add New Recipe
    public void addRecipe(String name, String ingredients) {
        if (name == null || name.isEmpty() || ingredients == null || ingredients.isEmpty()) {
            throw new IllegalArgumentException("Recipe name and ingredients cannot be empty");
        }
        if (recipeBook.containsKey(name)) {
            throw new IllegalArgumentException("Recipe already exists");
        }
        recipeBook.put(name, new Recipe(name, ingredients));
    }

    // 2. Edit Existing Recipe
    public void editRecipe(String name, String newIngredients) {
        Recipe recipe = recipeBook.get(name);
        if (recipe == null) {
            throw new IllegalArgumentException("Recipe does not exist");
        }
        recipe.setIngredients(newIngredients);
    }

    // 3. Delete Recipe
    public void deleteRecipe(String name) {
        if (!recipeBook.containsKey(name)) {
            throw new IllegalArgumentException("Recipe does not exist");
        }
        recipeBook.remove(name);
    }

    // Utility method to check if a recipe exists (for validation in test cases)
    public boolean recipeExists(String name) {
        return recipeBook.containsKey(name);
    }
    }
    
testcase

    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.Test;
    import static org.junit.jupiter.api.Assertions.*;

    public class RecipeManagerTest {

    private RecipeManager recipeManager;

    // Setup before each test
    @BeforeEach
    public void setUp() {
        recipeManager = new RecipeManager();
    }

    // 1. Test adding a new recipe
    @Test
    public void testAddNewRecipe() {
        // Valid input case
        recipeManager.addRecipe("Pasta", "Noodles, Tomato Sauce, Garlic");
        assertTrue(recipeManager.recipeExists("Pasta"));
    }

    // 2. Test adding a recipe with missing ingredients
    @Test
    public void testAddRecipeWithMissingIngredients() {
        // Invalid input case: Missing ingredients
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            recipeManager.addRecipe("Cake", "");
        });
        assertEquals("Recipe name and ingredients cannot be empty", exception.getMessage());
    }

    // 3. Test editing an existing recipe
    @Test
    public void testEditExistingRecipe() {
        // Add a recipe first
        recipeManager.addRecipe("Pasta", "Noodles, Tomato Sauce, Garlic");
        
        // Edit the recipe
        recipeManager.editRecipe("Pasta", "Noodles, Pesto Sauce, Garlic");
        
        // Ensure the ingredients were updated
        RecipeManager.Recipe recipe = recipeManager.recipeBook.get("Pasta");
        assertEquals("Noodles, Pesto Sauce, Garlic", recipe.getIngredients());
    }

    // 4. Test deleting an existing recipe
    @Test
    public void testDeleteRecipe() {
        // Add a recipe
        recipeManager.addRecipe("Pizza", "Dough, Tomato Sauce, Cheese");
        
        // Delete the recipe
        recipeManager.deleteRecipe("Pizza");
        
        // Ensure the recipe is no longer in the system
        assertFalse(recipeManager.recipeExists("Pizza"));
    }

    // Test for editing a non-existing recipe (negative case)
    @Test
    public void testEditNonExistingRecipe() {
        // Trying to edit a recipe that doesn't exist should throw an exception
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            recipeManager.editRecipe("NonExistent", "Some Ingredients");
        });
        assertEquals("Recipe does not exist", exception.getMessage());
    }

    // Test for deleting a non-existing recipe (negative case)
    @Test
    public void testDeleteNonExistingRecipe() {
        // Trying to delete a recipe that doesn't exist should throw an exception
        Exception exception = assertThrows(IllegalArgumentException.class, () -> {
            recipeManager.deleteRecipe("NonExistent");
        });
        assertEquals("Recipe does not exist", exception.getMessage());
    }
    }

white box calculator

    public class Calculator {

    // Method for addition
    public int add(int a, int b) {
        return a + b;
    }

    // Method for subtraction
    public int subtract(int a, int b) {
        return a - b;
    }

    // Method for multiplication
    public int multiply(int a, int b) {
        return a * b;
    }

    // Method for division with exception handling for division by zero
    public int divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Cannot divide by zero");
        }
        return a / b;
    }
    }
JUnit Test Class

    import static org.junit.jupiter.api.Assertions.*;
    import org.junit.jupiter.api.Test;

    public class CalculatorTest {

    // Create an instance of the Calculator class
    Calculator calculator = new Calculator();

    // Test addition
    @Test
    public void testAddition() {
        assertEquals(5, calculator.add(2, 3));
        assertEquals(0, calculator.add(0, 0));
        assertEquals(-1, calculator.add(-2, 1));
    }

    // Test subtraction
    @Test
    public void testSubtraction() {
        assertEquals(1, calculator.subtract(3, 2));
        assertEquals(0, calculator.subtract(0, 0));
        assertEquals(-3, calculator.subtract(-2, 1));
    }

    // Test multiplication
    @Test
    public void testMultiplication() {
        assertEquals(6, calculator.multiply(2, 3));
        assertEquals(0, calculator.multiply(0, 100));
        assertEquals(-6, calculator.multiply(-2, 3));
    }

    // Test division with valid inputs
    @Test
    public void testDivision() {
        assertEquals(2, calculator.divide(6, 3));
        assertEquals(0, calculator.divide(0, 1));
        assertEquals(-2, calculator.divide(-6, 3));
    }

    // Test division by zero to handle the exception
    @Test
    public void testDivisionByZero() {
        Exception exception = assertThrows(ArithmeticException.class, () -> {
            calculator.divide(1, 0);
        });
        assertEquals("Cannot divide by zero", exception.getMessage());
    }
    }
White Box ToDoList Manager

    import java.util.ArrayList;
    import java.util.List;

    public class ToDoListManager {
    private List<String> tasks;

    // Constructor initializes the task list
    public ToDoListManager() {
        tasks = new ArrayList<>();
    }

    // Method to add a task
    public void addTask(String task) {
        if (task == null || task.trim().isEmpty()) {
            throw new IllegalArgumentException("Task cannot be null or empty");
        }
        tasks.add(task);
    }

    // Method to remove a task by index
    public void removeTask(int index) {
        if (index < 0 || index >= tasks.size()) {
            throw new IndexOutOfBoundsException("Invalid task index");
        }
        tasks.remove(index);
    }

    // Method to view all tasks
    public List<String> viewTasks() {
        return new ArrayList<>(tasks);  // Return a copy of the task list
    }
    }

ToDoListManagerTest.java (JUnit Test Class)

    import static org.junit.jupiter.api.Assertions.*;
    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.Test;

    import java.util.List;
    
    public class ToDoListManagerTest {

    private ToDoListManager manager;

    // Set up a new ToDoListManager instance before each test
    @BeforeEach
    public void setUp() {
        manager = new ToDoListManager();
    }

    // Test adding valid tasks
    @Test
    public void testAddTask() {
        manager.addTask("Buy groceries");
        manager.addTask("Walk the dog");
        List<String> tasks = manager.viewTasks();
        
        assertEquals(2, tasks.size());
        assertEquals("Buy groceries", tasks.get(0));
        assertEquals("Walk the dog", tasks.get(1));
    }

    // Test adding a null or empty task (invalid input)
    @Test
    public void testAddTaskInvalid() {
        Exception exception1 = assertThrows(IllegalArgumentException.class, () -> {
            manager.addTask(null);
        });
        assertEquals("Task cannot be null or empty", exception1.getMessage());

        Exception exception2 = assertThrows(IllegalArgumentException.class, () -> {
            manager.addTask("");
        });
        assertEquals("Task cannot be null or empty", exception2.getMessage());
    }

    // Test removing tasks by valid index
    @Test
    public void testRemoveTask() {
        manager.addTask("Buy groceries");
        manager.addTask("Walk the dog");

        manager.removeTask(1); // Remove the second task
        List<String> tasks = manager.viewTasks();

        assertEquals(1, tasks.size());
        assertEquals("Buy groceries", tasks.get(0));
    }

    // Test removing a task with an invalid index
    @Test
    public void testRemoveTaskInvalidIndex() {
        manager.addTask("Buy groceries");

        Exception exception = assertThrows(IndexOutOfBoundsException.class, () -> {
            manager.removeTask(1);  // Invalid index
        });
        assertEquals("Invalid task index", exception.getMessage());
    }

    // Test viewing tasks when the list is empty
    @Test
    public void testViewTasksEmpty() {
        List<String> tasks = manager.viewTasks();
        assertTrue(tasks.isEmpty());
    }

    // Test viewing tasks when the list has multiple tasks
    @Test
    public void testViewTasks() {
        manager.addTask("Buy groceries");
        manager.addTask("Walk the dog");

        List<String> tasks = manager.viewTasks();
        assertEquals(2, tasks.size());
        assertEquals("Buy groceries", tasks.get(0));
        assertEquals("Walk the dog", tasks.get(1));
    }
    }
White Box Recipe Manager

    import java.util.ArrayList;
    import java.util.List;

    public class RecipeManager {

    private List<String> recipes;

    // Constructor to initialize the recipe list
    public RecipeManager() {
        recipes = new ArrayList<>();
    }

    // Method to add a recipe
    public void addRecipe(String recipe) {
        if (recipe == null || recipe.trim().isEmpty()) {
            throw new IllegalArgumentException("Recipe cannot be null or empty");
        }
        recipes.add(recipe);
    }

    // Method to remove a recipe by index
    public void removeRecipe(int index) {
        if (index < 0 || index >= recipes.size()) {
            throw new IndexOutOfBoundsException("Invalid recipe index");
        }
        recipes.remove(index);
    }

    // Method to retrieve all recipes
    public List<String> getRecipes() {
        return new ArrayList<>(recipes); // Return a copy of the list to prevent external modification
    }
    }
RecipeManagerTest.java (JUnit Test Class)
    
    import static org.junit.jupiter.api.Assertions.*;
    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.Test;
    import java.util.List;

    public class RecipeManagerTest {

    private RecipeManager recipeManager;

    // Setup method to initialize RecipeManager before each test
    @BeforeEach
    public void setUp() {
        recipeManager = new RecipeManager();
    }

    // Test valid recipe addition and retrieval
    @Test
    public void testAddAndGetRecipes() {
        recipeManager.addRecipe("Pancakes");
        recipeManager.addRecipe("Spaghetti Carbonara");

        List<String> recipes = recipeManager.getRecipes();
        assertEquals(2, recipes.size());
        assertEquals("Pancakes", recipes.get(0));
        assertEquals("Spaghetti Carbonara", recipes.get(1));
    }

    // Test adding null or empty recipes (should throw IllegalArgumentException)
    @Test
    public void testAddNullOrEmptyRecipe() {
        assertThrows(IllegalArgumentException.class, () -> recipeManager.addRecipe(null));
        assertThrows(IllegalArgumentException.class, () -> recipeManager.addRecipe(""));
        assertThrows(IllegalArgumentException.class, () -> recipeManager.addRecipe("   "));
    }

    // Test removing a recipe by valid index
    @Test
    public void testRemoveRecipe() {
        recipeManager.addRecipe("Pancakes");
        recipeManager.addRecipe("Spaghetti Carbonara");

        recipeManager.removeRecipe(0); // Remove "Pancakes"
        List<String> recipes = recipeManager.getRecipes();
        assertEquals(1, recipes.size());
        assertEquals("Spaghetti Carbonara", recipes.get(0));
    }

    // Test removing recipe with an invalid index (should throw IndexOutOfBoundsException)
    @Test
    public void testRemoveRecipeWithInvalidIndex() {
        recipeManager.addRecipe("Pancakes");

        assertThrows(IndexOutOfBoundsException.class, () -> recipeManager.removeRecipe(-1));
        assertThrows(IndexOutOfBoundsException.class, () -> recipeManager.removeRecipe(1)); // No recipe at index 1
    }

    // Test retrieving recipes when the list is empty
    @Test
    public void testGetRecipesWhenEmpty() {
        List<String> recipes = recipeManager.getRecipes();
        assertTrue(recipes.isEmpty());
    }
    }

    
    

