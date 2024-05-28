## 1. Why PHP is known as scripting language

PHP is known as a scripting language primarily because of the following reasons:

1. **Interpreted Execution**: Unlike compiled languages (such as C or Java), which are transformed into machine code before execution, PHP code is executed directly by an interpreter. This means that PHP scripts are interpreted at runtime, which is a characteristic of scripting languages.

2. **Server-Side Processing**: PHP is designed for server-side scripting. It is embedded within HTML and is primarily used to create dynamic web pages. When a user requests a PHP page, the server processes the PHP code and generates HTML, which is then sent to the user's browser.

3. **Ease of Integration**: PHP can be easily embedded within HTML code. Scripts written in PHP are typically used to add functionality to web pages, such as form handling, database interactions, and session management, which are common uses for scripting languages.

4. **Simplicity and Flexibility**: PHP is known for its simplicity and ease of use. It allows developers to quickly write scripts without the need for complex setups or configurations. This makes it ideal for web development where rapid development and deployment are often required.

5. **Dynamic Typing**: PHP, like other scripting languages, is dynamically typed. This means that variables do not need to be explicitly declared, and their type can change at runtime.

6. **Interactivity and Automation**: Scripting languages are often used to automate repetitive tasks and handle interactive user inputs. PHP fits this mold well by allowing web developers to create interactive web applications that can respond to user actions.

In summary, PHP is known as a scripting language because it is interpreted, server-side oriented, easy to integrate with HTML, simple to use, dynamically typed, and well-suited for creating interactive and automated web applications.

<br/><br/>

## 2. Explain difference between include and require.

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

<br><br>

## 3. Explain different loops used in PHP in detail.
PHP provides several types of loops to perform repetitive tasks efficiently. Each type of loop is used in different scenarios based on the requirements. Here's a detailed explanation of the different loops available in PHP:

### 1. `while` Loop

The `while` loop executes a block of code as long as the specified condition is true. It is typically used when the number of iterations is not known beforehand.

#### Syntax:
```php
while (condition) {
    // Code to be executed
}
```

#### Example:
```php
$i = 0;
while ($i < 5) {
    echo "The value of i is: $i\n";
    $i++;
}
```

### 2. `do-while` Loop

The `do-while` loop is similar to the `while` loop, but it guarantees that the block of code is executed at least once. This is because the condition is checked after the block of code is executed.

#### Syntax:
```php
do {
    // Code to be executed
} while (condition);
```

#### Example:
```php
$i = 0;
do {
    echo "The value of i is: $i\n";
    $i++;
} while ($i < 5);
```

### 3. `for` Loop

The `for` loop is used when the number of iterations is known beforehand. It consists of three parts: initialization, condition, and increment/decrement.

#### Syntax:
```php
for (initialization; condition; increment) {
    // Code to be executed
}
```

#### Example:
```php
for ($i = 0; $i < 5; $i++) {
    echo "The value of i is: $i\n";
}
```

### 4. `foreach` Loop

The `foreach` loop is specifically designed for iterating over arrays. It is a convenient way to access each element in an array without needing to manage the array's length or index manually.

#### Syntax:
```php
foreach ($array as $value) {
    // Code to be executed
}
```
For associative arrays, you can use:
```php
foreach ($array as $key => $value) {
    // Code to be executed
}
```

#### Example:
```php
$colors = array("red", "green", "blue", "yellow");

foreach ($colors as $color) {
    echo "The color is: $color\n";
}

// Associative array example
$age = array("Peter" => "35", "Ben" => "37", "Joe" => "43");

foreach ($age as $name => $age) {
    echo "$name is $age years old.\n";
}
```

### Comparison and Use Cases

1. **`while` Loop**:
   - **Use Case**: When you need to repeat a block of code while a condition remains true and the number of iterations is not known.
   - **Example**: Reading data from a file until the end of the file is reached.

2. **`do-while` Loop**:
   - **Use Case**: When you need to ensure that a block of code is executed at least once regardless of the condition.
   - **Example**: Displaying a menu at least once and asking for user input.

3. **`for` Loop**:
   - **Use Case**: When you know the exact number of iterations.
   - **Example**: Iterating over a range of numbers, like looping from 1 to 100.

4. **`foreach` Loop**:
   - **Use Case**: When you need to iterate over each element in an array or associative array.
   - **Example**: Accessing and processing each item in a list of user data.

### Practical Considerations

- **Performance**: `foreach` is generally faster than `for` when iterating over arrays because it is optimized for this purpose.
- **Readability**: `foreach` is often more readable and simpler to use for arrays, especially associative arrays.
- **Flexibility**: `while` and `do-while` provide more flexibility in terms of loop control and are not limited to arrays.

Understanding these loops and when to use them is crucial for efficient and readable PHP programming. Each loop serves a particular purpose and selecting the appropriate one can significantly affect the clarity and performance of your code.

<br><br>
##  4. How can we create links in PHP pages?

Creating links in PHP pages is straightforward and involves embedding HTML anchor (`<a>`) tags within your PHP code. The HTML anchor tag is used to create hyperlinks that navigate to other pages or resources. Here are several methods to create links in PHP:

### 1. **Basic Static Link**
A simple static link can be included directly in the HTML code within a PHP file.

#### Example:
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Static Link Example</title>
</head>
<body>
    <a href="https://www.example.com">Visit Example.com</a>
</body>
</html>
```

### 2. **Dynamic Links Using PHP Variables**
You can use PHP variables to dynamically create links.

#### Example:
```php
<?php
$page = "contact.php";
$text = "Contact Us";
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Dynamic Link Example</title>
</head>
<body>
    <a href="<?php echo $page; ?>"><?php echo $text; ?></a>
</body>
</html>
```

### 3. **Generating Links Based on Conditions**
You can create links dynamically based on certain conditions or logic.

#### Example:
```php
<?php
$is_logged_in = true;
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Conditional Link Example</title>
</head>
<body>
    <?php if ($is_logged_in): ?>
        <a href="logout.php">Logout</a>
    <?php else: ?>
        <a href="login.php">Login</a>
    <?php endif; ?>
</body>
</html>
```

### 4. **Links with Query Parameters**
You can create links with query parameters to pass information between pages.

#### Example:
```php
<?php
$user_id = 42;
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Link with Query Parameters Example</title>
</head>
<body>
    <a href="profile.php?id=<?php echo $user_id; ?>">View Profile</a>
</body>
</html>
```

### 5. **Using Functions to Generate Links**
For more complex applications, you might want to use functions to generate links.

#### Example:
```php
<?php
function createLink($url, $text) {
    return '<a href="' . $url . '">' . $text . '</a>';
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Link Function Example</title>
</head>
<body>
    <?php echo createLink('https://www.example.com', 'Visit Example.com'); ?>
</body>
</html>
```

### Summary

Creating links in PHP pages is essentially about embedding HTML anchor tags within your PHP code. Depending on the complexity and dynamic nature of your application, you can:

1. **Include static links directly in HTML.**
2. **Use PHP variables to dynamically generate links.**
3. **Create conditional links based on application logic.**
4. **Pass data between pages using query parameters.**
5. **Encapsulate link generation in reusable functions.**

These methods allow you to create flexible and dynamic navigation options in your PHP applications.

<br><br>
## 5.  Create HTML form to enter one number. Write PHP code to display the message about number is odd or even.

Creating an HTML form to enter a number and then writing PHP code to determine whether the number is odd or even involves two main parts: the HTML form and the PHP script to process the form data.

### HTML Form

First, create an HTML form that allows the user to input a number:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Odd or Even Checker</title>
</head>
<body>
    <form action="check_number.php" method="post">
        <label for="number">Enter a number:</label>
        <input type="number" id="number" name="number" required>
        <input type="submit" value="Check">
    </form>
</body>
</html>
```

### PHP Script

Next, create the `check_number.php` file to handle the form submission and determine if the number is odd or even:

```php
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // Retrieve the number from the form
    $number = $_POST['number'];

    // Check if the number is odd or even
    if ($number % 2 == 0) {
        $message = "The number $number is even.";
    } else {
        $message = "The number $number is odd.";
    }
} else {
    $message = "Please submit the form with a number.";
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Odd or Even Result</title>
</head>
<body>
    <h1><?php echo $message; ?></h1>
    <a href="index.html">Go back</a>
</body>
</html>
```

### Explanation

1. **HTML Form (`index.html`)**:
    - This form has a single input field for the user to enter a number and a submit button.
    - The form uses the POST method to send data to `check_number.php` for processing.

2. **PHP Script (`check_number.php`)**:
    - The script first checks if the form has been submitted by verifying the request method.
    - It retrieves the number from the POST data.
    - It then determines if the number is odd or even using the modulus operator (`%`).
    - A message is generated based on the result.
    - The message is displayed to the user, and there is a link to go back to the form.

### Folder Structure

Ensure the HTML file and PHP file are in the same directory or adjust the form action accordingly. The typical folder structure might look like this:

```
/your-project-folder
    index.html
    check_number.php
```

With this setup, when a user enters a number in the form and submits it, they will be redirected to the `check_number.php` page, where they will see a message indicating whether the number is odd or even.

<br><br>

## 1. How does one prevent the following Warning ‘Warning : Cannot modify header information - headers already sent’ and why does it occur in the first place?

The "Warning: Cannot modify header information - headers already sent" warning in PHP occurs when you attempt to send HTTP headers (such as with `header()` or `setcookie()`) after output has already been sent to the browser. This can happen for several reasons:

1. **Output Before Header Functions**: Any HTML, echo statements, or whitespace outside of PHP tags before a call to `header()` will cause this warning.
2. **Whitespace or Newlines Before PHP Tags**: Even a single newline or space before the opening `<?php` tag or after the closing `?>` tag can cause this issue.
3. **File Inclusion Order**: Including files that have output (e.g., echo statements or HTML) before the header functions can also lead to this warning.

### How to Prevent the Warning

Here are steps and best practices to prevent the "headers already sent" warning:

1. **Ensure No Output Before Header Calls**: Make sure no HTML, echo statements, or print statements are used before you call header functions.

#### Example of Wrong Usage:
```php
echo "This will cause the warning";
header('Location: http://example.com');
```

#### Example of Correct Usage:
```php
header('Location: http://example.com');
exit; // Always good practice to exit after a redirect
```

2. **Avoid Closing PHP Tags**: In PHP files that are purely PHP code, avoid using the closing `?>` tag. This prevents accidental whitespace or newline issues.

#### Example:
```php
<?php
// Your PHP code here

// Do not close with '?>'
```

3. **Buffer Output**: Use output buffering functions if you need to ensure that headers can be sent even if there is some output.

#### Example:
```php
ob_start(); // Start output buffering

// Your code here, including possible outputs
echo "Some output";

// Now you can still send headers
header('Location: http://example.com');
ob_end_flush(); // End output buffering and flush the buffer
```

4. **Check Included Files**: Ensure that any included files do not generate output before you send headers.

#### Example:
```php
// If you include another PHP file
include('somefile.php');

// Ensure 'somefile.php' does not have any output before header function
header('Location: http://example.com');
exit;
```

### Common Causes and Fixes

1. **Accidental Whitespace**:
   - Check for and remove any whitespace or newlines before the opening `<?php` tag or after the closing `?>` tag in your PHP files.
   
2. **Misplaced HTML**:
   - Ensure that any HTML output is placed after any `header()` calls.

3. **Debugging Output**:
   - Avoid using echo, print, or any kind of direct output in files that handle header redirections or cookies.

### Example with Proper Practices

#### index.php:
```php
<?php
// Correct usage without accidental output

// Start session or other initializations if needed
session_start();

// Include files if needed
include('somefile.php');

// Perform some logic
if ($some_condition) {
    header('Location: http://example.com');
    exit;
}

// Rest of your PHP code or HTML output
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Example</title>
</head>
<body>
    <p>This is the rest of the HTML content.</p>
</body>
</html>
```

#### somefile.php:
```php
<?php
// Proper included file without unintended output

// Some PHP code
$some_condition = true; // Example variable setting

// No echo, print, or HTML output here
?>
```

By following these practices, you can avoid the "headers already sent" warning and ensure your PHP scripts work as intended.


<br><br>
## 2. Write a short note on POST method.

### The POST Method in PHP

The POST method is one of the HTTP request methods used by browsers to submit data to a web server. In PHP, the POST method is commonly used in form submissions to send data securely and efficiently from the client to the server for processing. Here are some key points about the POST method:

#### 1. **Data Transmission**
- **Data Handling**: Data sent via the POST method is included in the body of the HTTP request, not in the URL. This allows for a larger amount of data to be transmitted compared to the GET method.
- **Security**: Since the data is not appended to the URL, it is not visible in the browser's address bar, offering better security for sensitive information such as passwords and personal details. However, it should be noted that the POST data is not encrypted by default unless it is sent over HTTPS.

#### 2. **Form Submission**
- **HTML Forms**: The POST method is often used in HTML forms when the form data should not be visible in the URL or when large amounts of data need to be sent.
  ```html
  <form action="process.php" method="post">
      <label for="username">Username:</label>
      <input type="text" id="username" name="username">
      <label for="password">Password:</label>
      <input type="password" id="password" name="password">
      <input type="submit" value="Submit">
  </form>
  ```

#### 3. **Accessing POST Data in PHP**
- **Superglobal Array**: PHP provides the `$_POST` superglobal array to access data sent via the POST method. Each form field's name attribute corresponds to a key in the `$_POST` array.
  ```php
  <?php
  if ($_SERVER["REQUEST_METHOD"] == "POST") {
      $username = $_POST['username'];
      $password = $_POST['password'];
      
      echo "Username: " . htmlspecialchars($username) . "<br>";
      echo "Password: " . htmlspecialchars($password);
  }
  ?>
  ```

#### 4. **Advantages**
- **Large Data Handling**: Unlike the GET method, which has URL length limitations, the POST method can handle large amounts of data.
- **Data Privacy**: Data is not exposed in the URL, providing a level of privacy for sensitive information.

#### 5. **Disadvantages**
- **Caching**: POST requests are not cached by default, which means they can be less efficient for certain types of repeated requests.
- **URL Sharing**: Since POST data is not included in the URL, the exact data submitted cannot be bookmarked or shared directly.

### Example Scenario

A typical use case for the POST method is a login form where users enter their username and password. The form data is sent securely to the server for authentication, and since the data is not visible in the URL, it adds a layer of security.

### Summary

The POST method is a powerful and versatile tool for submitting data from client to server in web applications. It supports large data payloads and provides better security for sensitive information compared to the GET method. In PHP, accessing POST data is straightforward using the `$_POST` superglobal array, making it a common choice for handling form submissions and other data-intensive operations.

<br><br>

 ## 3. Explain File Create and write functions with syntax.
In PHP, you can create and write to files using built-in functions that provide a straightforward way to handle file operations. The primary functions used for creating and writing files are `fopen()`, `fwrite()`, and `fclose()`. Here is an explanation of these functions along with their syntax:

### 1. `fopen()`

The `fopen()` function is used to open a file or URL. It returns a file handle resource on success, or `false` on failure. The function requires two parameters: the name of the file to open, and the mode in which to open the file.

#### Syntax:
```php
$handle = fopen($filename, $mode);
```

#### Common Modes:
- `'r'` : Open for reading only; place the file pointer at the beginning of the file.
- `'w'` : Open for writing only; place the file pointer at the beginning of the file and truncate the file to zero length. If the file does not exist, attempt to create it.
- `'a'` : Open for writing only; place the file pointer at the end of the file. If the file does not exist, attempt to create it.
- `'x'` : Create and open for writing only; place the file pointer at the beginning of the file. If the file already exists, the `fopen()` call will fail.
- `'r+'`: Open for reading and writing; place the file pointer at the beginning of the file.

### 2. `fwrite()`

The `fwrite()` function writes data to the file opened by `fopen()`. It requires two parameters: the file handle and the string to write to the file. An optional third parameter specifies the maximum number of bytes to write.

#### Syntax:
```php
fwrite($handle, $string, $length);
```

- `$handle`: The file handle resource obtained from `fopen()`.
- `$string`: The string to write to the file.
- `$length`: (Optional) Maximum number of bytes to write. If omitted, `fwrite()` will write the entire string.

### 3. `fclose()`

The `fclose()` function closes an open file pointer. It requires one parameter: the file handle to close.

#### Syntax:
```php
fclose($handle);
```

### Example: Creating and Writing to a File

Here is a complete example demonstrating how to create a file and write to it using `fopen()`, `fwrite()`, and `fclose()`:

```php
<?php
$filename = 'example.txt';
$handle = fopen($filename, 'w'); // Open the file for writing

if ($handle) {
    $content = "Hello, World!\nThis is a test file.";
    
    // Write content to the file
    fwrite($handle, $content);
    
    // Close the file
    fclose($handle);
    
    echo "File created and written successfully.";
} else {
    echo "Failed to open the file.";
}
?>
```

### Explanation:

1. **Open the File**:
   - `fopen($filename, 'w')`: Opens the file `example.txt` for writing. If the file does not exist, it will be created. If it does exist, it will be truncated to zero length.

2. **Check if the File Opened Successfully**:
   - The `if ($handle)` check ensures that the file was opened successfully before proceeding.

3. **Write to the File**:
   - `fwrite($handle, $content)`: Writes the string `$content` to the file.

4. **Close the File**:
   - `fclose($handle)`: Closes the file, ensuring that all data is written and resources are freed.

By following this method, you can create and write to files in PHP efficiently. Proper error handling ensures that the file operations are performed safely.


<br><br>

## 4. Explain what cookies are. Explain how to set cookies in PHP with syntax.

### Cookies in PHP

Cookies are small pieces of data stored on the client-side (browser) and sent to the server with each subsequent request. They are used to remember information about the user, such as login status, preferences, and other session-related data. Cookies are an essential part of maintaining state in stateless HTTP protocol.

### Setting Cookies in PHP

In PHP, cookies are set using the `setcookie()` function. This function must be called before any output is sent to the browser (i.e., before any HTML, echo statements, or whitespace).

#### Syntax:
```php
bool setcookie ( string $name [, string $value = "" [, int $expires = 0 [, string $path = "" [, string $domain = "" [, bool $secure = false [, bool $httponly = false ]]]]]] )
```

#### Parameters:
1. **`name`**: The name of the cookie. This is the key used to access the cookie value.
2. **`value`**: The value of the cookie. This is the data stored in the cookie.
3. **`expires`**: The expiration time of the cookie. This is a Unix timestamp indicating when the cookie should expire. If set to 0, the cookie will expire at the end of the session (when the browser closes).
4. **`path`**: The path on the server where the cookie is available. If set to '/', the cookie will be available within the entire domain.
5. **`domain`**: The domain that the cookie is available to. If not specified, it defaults to the host portion of the current document location.
6. **`secure`**: If true, the cookie will only be transmitted over secure HTTPS connections.
7. **`httponly`**: If true, the cookie will only be accessible through the HTTP protocol and not by JavaScript, which helps prevent XSS attacks.

### Example: Setting a Cookie

Here’s a simple example of setting a cookie in PHP:

```php
<?php
// Set a cookie named "user" with the value "John Doe" that expires in one hour
setcookie("user", "John Doe", time() + 3600, "/");

// Check if the cookie is set
if(isset($_COOKIE["user"])) {
    echo "Cookie 'user' is set!<br>";
    echo "Value is: " . $_COOKIE["user"];
} else {
    echo "Cookie 'user' is not set!";
}
?>
```

### Explanation:

1. **Set the Cookie**:
   - `setcookie("user", "John Doe", time() + 3600, "/")`: Sets a cookie named "user" with the value "John Doe". The cookie will expire in one hour (`time() + 3600` seconds). The cookie is available within the entire domain (`"/"`).

2. **Check if the Cookie is Set**:
   - `isset($_COOKIE["user"])`: Checks if the "user" cookie is set.
   - `$_COOKIE["user"]`: Retrieves the value of the "user" cookie.

### Important Notes:

- **Headers**: The `setcookie()` function must be called before any other output (HTML, echo statements, etc.) is sent to the browser.
- **Expiration Time**: The expiration time is specified as a Unix timestamp. Use the `time()` function to get the current time and add the number of seconds for the desired expiration.
- **Security**: Use the `secure` and `httponly` parameters to enhance the security of the cookies.
- **Accessing Cookies**: Once a cookie is set, it can be accessed through the `$_COOKIE` superglobal array.

### Example with Additional Parameters

Here’s an example of setting a cookie with more parameters:

```php
<?php
// Set a secure cookie named "session_id" with the value "abc123" that expires in one day, available only over HTTPS and not accessible via JavaScript
setcookie("session_id", "abc123", time() + 86400, "/", "example.com", true, true);

// Check if the cookie is set
if(isset($_COOKIE["session_id"])) {
    echo "Cookie 'session_id' is set!<br>";
    echo "Value is: " . $_COOKIE["session_id"];
} else {
    echo "Cookie 'session_id' is not set!";
}
?>
```

In this example:
- The cookie named "session_id" is set with a value of "abc123".
- The cookie expires in one day (`time() + 86400` seconds).
- The cookie is available within the entire domain (`"/"`) and specific to "example.com".
- The cookie is marked as secure (`true`), meaning it will only be sent over HTTPS.
- The cookie is marked as HTTP-only (`true`), meaning it cannot be accessed via JavaScript.

By understanding and using these parameters, you can effectively manage cookies in PHP to maintain user sessions and store user-specific information securely.

<br><br>


## 5. Explain Basic Error Handling die () function. What will be the output of following code? 
```
<?php
   if(!file_exists("welcome.txt"))
{
    die("File not found");
}
   Else
{
   $file=fopen("welcom.txt","r");
}
?>
```
The `die()` function in PHP is used to display an error message and terminate the script execution if a certain condition is met. It's commonly used for basic error handling to gracefully handle situations where the script cannot continue due to an error.

In the provided code snippet:

```php
<?php
if (!file_exists("welcome.txt")) {
    die("File not found");
} else {
    $file = fopen("welcom.txt", "r");
}
?>
```

Here's what happens:

1. The `if` statement checks if the file "welcome.txt" does not exist using the `file_exists()` function.
2. If the file does not exist, the `die()` function is called with the message "File not found". This terminates the script execution immediately, and the message is displayed to the user.
3. If the file exists, the `else` block is executed. However, there is a typo in the filename passed to `fopen()`. It tries to open "welcom.txt" instead of "welcome.txt". This will result in an error if error reporting is enabled, but since it's not handled explicitly, it won't cause the script to terminate immediately.

The output of the code will be:

```
File not found
```

This output is displayed because the `file_exists()` function returns false since "welcome.txt" does not exist. Therefore, the script execution is halted by the `die()` function, and the message "File not found" is displayed to the user. The `fopen()` function in the `else` block is not executed due to the script termination caused by `die()`.

<br><br>

## 1.  What is difference between Interface and Abstract class?

Both interfaces and abstract classes are used in object-oriented programming to define a contract for classes to implement certain methods or properties. However, there are some key differences between them:

### Abstract Class:

1. **Definition**:
   - An abstract class is a class that cannot be instantiated on its own and may contain both abstract methods (methods without a body) and concrete methods (methods with a body).
   
2. **Implementation**:
   - An abstract class can provide partial implementations for methods, allowing subclasses to override or extend them as needed.
   
3. **Extensibility**:
   - A subclass can extend only one abstract class due to single inheritance in PHP.
   
4. **Example**:
   ```php
   abstract class Animal {
       abstract public function makeSound();
       
       public function sleep() {
           echo "Zzz...\n";
       }
   }
   ```

### Interface:

1. **Definition**:
   - An interface is a contract that defines a set of methods without specifying their implementation. It cannot contain any concrete methods, only method signatures.
   
2. **Implementation**:
   - A class that implements an interface must provide implementations for all methods defined in the interface.
   
3. **Extensibility**:
   - A class can implement multiple interfaces, allowing for multiple inheritance of type.
   
4. **Example**:
   ```php
   interface Shape {
       public function area();
       public function perimeter();
   }
   ```

### Key Differences:

1. **Instantiation**:
   - Abstract classes cannot be instantiated directly, while interfaces cannot be instantiated at all. They are only used to define a contract for classes.
   
2. **Implementation**:
   - Abstract classes can have both abstract and concrete methods with partial implementation, while interfaces can only have method signatures without implementation.
   
3. **Extensibility**:
   - Subclasses can extend only one abstract class, but a class can implement multiple interfaces.

### When to Use:

- **Abstract Class**:
  - Use when you need to provide a common base implementation for related classes.
  - Use when you want to enforce that subclasses implement certain methods while providing some default behavior.

- **Interface**:
  - Use when you want to define a contract for unrelated classes to ensure they have certain methods in common.
  - Use when you want to achieve multiple inheritance of type by allowing classes to implement multiple interfaces.

In summary, abstract classes provide a way to share code among related classes, while interfaces define a contract for unrelated classes to adhere to. Both serve different purposes and can be used depending on the specific requirements of your application's design.
