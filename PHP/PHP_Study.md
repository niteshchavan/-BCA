### 1. Why PHP is known as scripting language

PHP is known as a scripting language primarily because of the following reasons:

1. **Interpreted Execution**: Unlike compiled languages (such as C or Java), which are transformed into machine code before execution, PHP code is executed directly by an interpreter. This means that PHP scripts are interpreted at runtime, which is a characteristic of scripting languages.

2. **Server-Side Processing**: PHP is designed for server-side scripting. It is embedded within HTML and is primarily used to create dynamic web pages. When a user requests a PHP page, the server processes the PHP code and generates HTML, which is then sent to the user's browser.

3. **Ease of Integration**: PHP can be easily embedded within HTML code. Scripts written in PHP are typically used to add functionality to web pages, such as form handling, database interactions, and session management, which are common uses for scripting languages.

4. **Simplicity and Flexibility**: PHP is known for its simplicity and ease of use. It allows developers to quickly write scripts without the need for complex setups or configurations. This makes it ideal for web development where rapid development and deployment are often required.

5. **Dynamic Typing**: PHP, like other scripting languages, is dynamically typed. This means that variables do not need to be explicitly declared, and their type can change at runtime.

6. **Interactivity and Automation**: Scripting languages are often used to automate repetitive tasks and handle interactive user inputs. PHP fits this mold well by allowing web developers to create interactive web applications that can respond to user actions.

In summary, PHP is known as a scripting language because it is interpreted, server-side oriented, easy to integrate with HTML, simple to use, dynamically typed, and well-suited for creating interactive and automated web applications.

<br/><br/>

### 2. Explain difference between include and require.

In PHP, `include` and `require` are both used to include and evaluate the specified file. However, there are important differences in how they handle errors and the flow of execution:

### `include`

1. **Behavior on Failure**:
   - If the specified file is not found or there is an error in including the file, `include` will emit a warning (E_WARNING), but the script will continue executing.
   
2. **Use Case**:
   - `include` is typically used when the file being included is not essential to the main application logic, allowing the application to continue running even if the file is missing.

3. **Example**:
   ```php
   include 'optional-file.php';
   echo 'This will still be executed even if the file is missing.';
   ```

### `require`

1. **Behavior on Failure**:
   - If the specified file is not found or there is an error in including the file, `require` will emit a fatal error (E_COMPILE_ERROR), and the script will stop executing immediately.
   
2. **Use Case**:
   - `require` is used when the file is essential for the application to run correctly. If the file is missing, the script cannot and should not continue.

3. **Example**:
   ```php
   require 'essential-file.php';
   echo 'This will not be executed if the file is missing.';
   ```

### Summary

- **`include`**: Emits a warning on failure and the script continues executing. Use it when the file is not critical to the application.
- **`require`**: Emits a fatal error on failure and stops script execution. Use it when the file is critical to the application's functioning.

### Practical Example

```php
// Using include
include 'header.php';  // If header.php is missing, the script will still run
echo 'This will be displayed even if header.php is not found.';

// Using require
require 'config.php';  // If config.php is missing, the script stops running
echo 'This will not be displayed if config.php is not found.';
```

In the example above, if `header.php` is missing, a warning will be shown, but "This will be displayed even if header.php is not found." will still be printed. Conversely, if `config.php` is missing, a fatal error occurs and "This will not be displayed if config.php is not found." will not be printed.
