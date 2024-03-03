
### 1. Imports:

```python
import requests, csv, mysql.connector
import os
import numpy as np
import tensorflow as tf

from flask import Flask, request, render_template, redirect, url_for, session, send_from_directory, jsonify
from werkzeug.security import generate_password_hash, check_password_hash
from io import StringIO
from datetime import datetime, timedelta
from decimal import Decimal
```

- `requests`: Used for making HTTP requests to external APIs.
- `csv`: Utilized for parsing CSV data.
- `mysql.connector`: Facilitates communication with MySQL database.
- `os`: Provides functions for interacting with the operating system, such as file operations.
- `numpy`: Library for numerical computing, often used for array operations.
- `tensorflow`: Framework for building and training machine learning models.
- `Flask`: Web framework for building web applications in Python.
- `render_template`, `redirect`, `url_for`: Functions for rendering HTML templates and managing URL redirection.
- `session`: Module for managing user sessions in Flask.
- `send_from_directory`: Function for serving static files.
- `generate_password_hash`, `check_password_hash`: Functions for password hashing and verification.
- `StringIO`: Allows manipulation of string data as file objects.
- `datetime`, `timedelta`: Classes for manipulating dates and times.
- `Decimal`: Class for handling decimal numbers.

### 2. Configuration:

```python
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '3'  # Suppress TensorFlow messages

app = Flask(__name__)
app.secret_key = os.urandom(24)

db = mysql.connector.connect(
    host="localhost",
    user="nitesh",
    password="root@123",
    database="stocks"
)
```

- Sets environment variable to suppress TensorFlow logging messages.
- Initializes a Flask application and sets a secret key for session management.
- Establishes a connection to a MySQL database named "stocks" with specified credentials.

### 3. Routes:

#### Static File Routes:

```python
@app.route('/img/<path:filename>')
@app.route('/lib/<path:filename>')
@app.route('/css/<path:filename>')
@app.route('/js/<path:filename>')
def get_static(filename):
    return send_from_directory('templates', filename)
```

- These routes serve static files (images, libraries, CSS, and JavaScript) from the "templates" directory.

#### HTML Page Rendering Routes:

```python
@app.route('/<page>')
def render_page(page):
    if 'email' in session:
        email = session['email']
        return render_template(f'{page}', email=email)
    return redirect(url_for('signin'))
```

- Renders HTML pages using Jinja2 templates. If a user is logged in (checked via session), it passes the user's email to the template. Otherwise, it redirects to the sign-in page.

#### User Authentication Routes:

```python
@app.route('/signup', methods=['GET', 'POST'])
def signup():
    if request.method == 'POST':
        email = request.form['email']
        password = request.form['password']
        hashed_password = generate_password_hash(password)
        cursor = db.cursor()
        cursor.execute("INSERT INTO logins (email, password) VALUES (%s, %s)", (email, hashed_password))
        db.commit()
        cursor.close()
    return redirect(url_for('index'))
```

- Handles user sign-up. It accepts POST requests with email and password, hashes the password, and stores the user credentials in the database.

```python
@app.route('/signin', methods=['GET', 'POST'])
def signin():
    if request.method == 'POST':
        email = request.form['email']
        password = request.form['password']
        cursor = db.cursor()
        cursor.execute("SELECT password FROM logins WHERE email = %s", (email,))
        result = cursor.fetchone()
        cursor.close()
        if result:
            stored_password = result[0]
            if check_password_hash(stored_password, password):
                session['email'] = email
                return redirect(url_for('index'))
        return 'Invalid username/password'
    return render_template('signin.html')
```

- Handles user sign-in. It checks the provided email and password against the database and starts a session if authentication is successful.

```python
@app.route('/logout')
def logout():
    session.pop('email', None)
    return redirect(url_for('signin'))
```

- Logs out the user by removing their email from the session.

#### Other Routes:

There are several other routes for various functionalities such as fetching stock symbols and data, updating stock data from an external API, processing and fetching stock data for analysis, and making predictions using machine learning models.

These routes involve interacting with the database, making HTTP requests to external APIs, processing data, and returning JSON responses.

### 4. Database Operations:

The code interacts with a MySQL database to store and retrieve user authentication information, stock symbols, and market data.

#### 5. External API Calls:

The application interacts with an external API provided by the Bombay Stock Exchange (BSE) to fetch stock price data and other relevant information. This section primarily focuses on making HTTP requests to the BSE API, processing the responses, and handling errors gracefully.

##### 1. **Fetching Stock Price Data:**

```python
@app.route('/update_data/<symbol>')
def update_data(symbol):
    if 'email' in session:
        # Retrieving necessary data from the database
        cursor = db.cursor()
        cursor.execute(f"SELECT Symbol_id FROM `{symbol}`Limit 1")
        data = cursor.fetchone()
        symbol_id = data[0]
        cursor.close()
        
        # URL and headers for making the API request
        url = "https://api.bseindia.com/BseIndiaAPI/api/StockPriceCSVDownload/w"
        headers = {
            "User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0",
            "Referer": "https://www.bseindia.com/",
        }
        
        # Payload data for the API request
        fromDate = '01/02/2024'
        toDate = datetime.now().strftime('%d/%m/%Y')
        payload = {
            "pageType": "0",
            "rbType": "D",
            "Scode": symbol_id,
            "FDates": fromDate,
            "TDates": toDate
        }

        try:
            # Sending the API request
            response = requests.get(url, headers=headers, params=payload, timeout=5)
            
            # Handling timeout errors
            except requests.exceptions.Timeout:
                return jsonify({"error": "Request timed out. Please try again later."}), 504
                
            if response.status_code == 200:
                # Processing the CSV response
                csv_buffer = StringIO(response.text)
                csv_reader = csv.DictReader(csv_buffer, delimiter='\t')
                
                # Iterating over CSV rows and updating the database
                for row in csv_reader:
                    data = list(row.values())[0].split(',')
                    date_str = data[0]
                    date_obj = datetime.strptime(date_str, "%d-%B-%Y")
                    mysql_date_format = date_obj.strftime("%Y-%m-%d")
                    open_price = data[1]
                    high_price = data[2]
                    low_price = data[3]
                    close_price = data[4]
                    
                    # Inserting data into the respective table
                    cursor.execute(
                        f"INSERT IGNORE INTO {symbol} (Symbol_id, Date, Open_Price, High_Price, Low_Price, Close_Price) VALUES (%s, %s, %s, %s, %s, %s)",
                        (symbol_id, mysql_date_format, open_price, high_price, low_price, close_price))
                    db.commit()
                    
                cursor.close()    
                return jsonify({'success': True})
            else:
                return jsonify({"error": "Failed to fetch data"}), response.status_code
        except Exception as e:
            return jsonify(error=str(e)), 500
    return redirect(url_for('signin'))
```

**Explanation:**
- This route (`/update_data/<symbol>`) is responsible for updating the stock price data for a given symbol in the database.
- It first retrieves the symbol's ID from the database.
- Then it constructs the URL, headers, and payload for the API request to fetch the stock price data.
- It makes the API request using `requests.get()` and handles timeout errors gracefully.
- If the request is successful (status code 200), it processes the CSV response, extracts relevant data, and updates the database accordingly.
- Any exceptions encountered during the process are caught, and appropriate error responses are returned.

##### 2. **Fetching Stock Data for Analysis:**

```python
@app.route('/process_script_data', methods=['POST'])
def process_script_data():
    if 'email' in session:        
        if request.method == 'POST':
            stock_data = request.form['stockData']
            fromDate = datetime.strptime(request.form['fromDate'], '%Y-%m-%d').strftime('%d/%m/%Y')
            toDate = datetime.strptime(request.form['toDate'], '%Y-%m-%d').strftime('%d/%m/%Y')
            
            # Constructing the URL, headers, and payload for the API request
            url = "https://api.bseindia.com/BseIndiaAPI/api/StockPriceCSVDownload/w"
            headers = {
                "User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0",
                "Referer": "https://www.bseindia.com/",
            }
            payload = {
                "pageType": "0",
                "rbType": "D",
                "Scode": script_code,
                "FDates": fromDate,
                "TDates": toDate
            }
            
            try:
                # Sending the API request
                response = requests.get(url, headers=headers, params=payload, timeout=5)
                response.raise_for_status()
                
                # Processing the CSV response
                # ...
                # (Code for processing CSV response)
                
                return jsonify({'success': True})
            except requests.exceptions.Timeout:
                return jsonify({"error": "Request timed out. Please try again later."}), 504
            except requests.exceptions.RequestException as e:
                return jsonify({"error": f"An error occurred: {e}"}), 500
    return redirect(url_for('signin'))
```

**Explanation:**
- This route (`/process_script_data`) is used to fetch stock price data for analysis within a specified date range.
- It receives the stock data, from date, and to date as POST parameters.
- It constructs the URL, headers, and payload for the API request to fetch the stock price data within the specified date range.
- It makes the API request using `requests.get()` and handles timeout errors gracefully.
- If the request is successful (status code 200), it processes the CSV response (not shown here) and returns a success response.


### 6. Machine Learning with TensorFlow:

The application utilizes TensorFlow, a popular machine learning framework, for training and deploying models for stock price prediction. Below are the relevant parts of the code:

#### Training Data Retrieval:

```python
@app.route('/train_data/<symbol>')
def train_data(symbol):
    if 'email' in session:    
        # Fetch x_train and y_train from the database
        cursor = db.cursor()
        cursor.execute(f"SELECT Open_Price, High_Price, Low_Price, Close_Price FROM `{symbol}`")

        data = cursor.fetchall()
        
        cursor.close()

        # Convert data to suitable Python data types
        data = [(float(row[0]), float(row[1]), float(row[2]), float(row[3])) for row in data]

        x_train = np.array([row[:-1] for row in data])  # Excludes last element (Close_Price) from the data
        y_train = np.array([row[3] for row in data])    # Extracting only the Close_Price, 4th object
```

- This route (`/train_data/<symbol>`) retrieves training data (features and labels) from the database for a given stock symbol.
- It queries the database to fetch historical stock price data (Open, High, Low, Close) for the specified symbol.
- The fetched data is then converted into NumPy arrays for further processing.

#### Model Definition and Training:

```python
@app.route('/train_data/<symbol>')
def train_data(symbol):
    if 'email' in session:    
        # Fetch x_train and y_train from the database
        # ... (Previous code)
        
        # Define Sequential model with 3 layers
        model = tf.keras.Sequential(
            [
                tf.keras.layers.Dense(64, activation='relu', input_shape=(x_train.shape[1],)),
                tf.keras.layers.Dense(32, activation='relu'),
                tf.keras.layers.Dense(1),
            ]
        )
        # Compile the model
        model.compile(optimizer='adam',
                      loss='mean_squared_error',  # Mean squared error loss for regression
                      metrics=['mae'])  # Mean absolute error metric

        # Train the model
        model.fit(x_train, y_train, epochs=10, batch_size=32)  # Adjust batch size and number of epochs as needed
```

- The code defines a Sequential model using Keras, a high-level API of TensorFlow, with three dense layers.
- The model architecture consists of two hidden layers with ReLU activation functions and one output layer.
- The model is compiled with an Adam optimizer, mean squared error loss function, and mean absolute error metric.
- Finally, the model is trained using the training data (features and labels) fetched from the database.

#### Model Saving:

```python
@app.route('/train_data/<symbol>')
def train_data(symbol):
    if 'email' in session:    
        # ... (Previous code)

        # Save the trained model
        model.save(f'models/{symbol}.keras')
        return jsonify({'success': True})
```

- After training, the trained model is saved to a file in the "models" directory with the symbol name as the filename.

### Prediction Using Trained Model:

```python
@app.route('/prediction/<symbol>')
def prediction(symbol):
    if 'email' in session:
        model_path = f'models\\{symbol}.keras'
        
        # Check if model is trained or not 
        if not os.path.exists(model_path):
            return jsonify(error='Model not available'), 404

        # Load the trained model
        model = tf.keras.models.load_model(model_path)
        
        # Fetch historical data for the stock from the database
        cursor = db.cursor()
        cursor.execute(f"SELECT Date, Open_Price, High_Price, Low_Price, Close_Price FROM {symbol} ORDER BY Date DESC Limit 20")
        historical_data = cursor.fetchall()
        cursor.close()

        # Convert decimal.Decimal values to float and format date
        historical_data = [(row[0].strftime('%Y-%m-%d'), float(row[1]), float(row[2]), float(row[3]), float(row[4])) for row in historical_data]
        historical_data.sort(key=lambda x: datetime.strptime(x[0], '%Y-%m-%d'))

        # Perform prediction using the trained model
        predicted_close_prices = []
        current_date = datetime.strptime(historical_data[-1][0], '%Y-%m-%d')  
        for _ in range(5):
            x_input = np.array([[historical_data[-1][1], historical_data[-1][2], historical_data[-1][3]]])
            predicted_close_price = model.predict(x_input)
            predicted_close_prices.append(float(predicted_close_price[0][0]))
            current_date += timedelta(days=1)
            historical_data.append((current_date.strftime('%Y-%m-%d'), float(predicted_close_price[0][0]), float(predicted_close_price[0][0]), float(predicted_close_price[0][0]), float(historical_data[-1][4])))
        
        # Prepare response data
        response_data = {
            "dates": [data_point[0] for data_point in historical_data],
            "historical_prices": [data_point[4] for data_point in historical_data],
            "predicted_prices": [float(price) for price in predicted_close_prices]
        }            
        return jsonify(response_data)
```

- This route (`/prediction/<symbol>`) is responsible for making predictions for the future stock prices of the specified symbol using the trained model.
- It first checks if the trained model file exists for the given symbol.
- If the model exists, it loads the model from the saved file.
- It then fetches historical stock price data for the specified symbol from the database.
- Using the loaded model, it predicts the future stock prices for the next 5 days based on the historical data.
- Finally, it prepares and returns the prediction results in JSON format.


### 7. Error Handling:

The code handles errors gracefully by catching exceptions and returning appropriate HTTP responses with error messages.

### 8. Web Server Setup:

The web server setup is a crucial aspect of deploying the Flask application. It ensures that the application is accessible over the internet and handles incoming requests efficiently. Below, I'll explain the setup and configuration of the web server:


#### 1. Flask Development Server:

```python
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
```

- The `if __name__ == '__main__':` block ensures that the Flask development server is started only when the script is executed directly.
- `app.run()` method is used to run the Flask application. Here:
  - `host='0.0.0.0'` makes the server publicly accessible, allowing requests from any IP address.
  - `port=5000` specifies the port number on which the Flask server listens for incoming HTTP requests.
  - `debug=True` enables debug mode, providing detailed error messages and auto-restarting the server on code changes during development.

#### 2. Deployment on Production Server:

For deploying the Flask application on a production server, using a dedicated web server like Nginx or Apache in conjunction with a WSGI server (such as Gunicorn or uWSGI) is recommended. Here's a typical setup:

- **Nginx as Reverse Proxy:**
  - Nginx serves as a reverse proxy, forwarding incoming HTTP requests to the WSGI server.
  - It also handles static file serving and SSL termination.
  - Configuration involves setting up server blocks (virtual hosts) in Nginx configuration files (`nginx.conf` or separate files in `/etc/nginx/sites-available/`) to define how requests are processed.
  - Example server block for Flask application:

    ```
    server {
        listen 80;
        server_name example.com www.example.com;

        location / {
            include proxy_params;
            proxy_pass http://localhost:8000;  # Forward requests to Gunicorn
        }

        location /static {
            alias /path/to/static/files;
        }

        location /media {
            alias /path/to/media/files;
        }

        # Additional configuration for SSL termination, caching, etc.
    }
    ```

- **WSGI Server (Gunicorn):**
  - Gunicorn serves as the WSGI server, responsible for running the Flask application.
  - It manages multiple worker processes to handle concurrent requests efficiently.
  - Example command to start Gunicorn:

    ```
    gunicorn -w 4 -b localhost:8000 wsgi:app
    ```

    - `-w 4`: Number of worker processes.
    - `-b localhost:8000`: Bind address and port for the server.
    - `wsgi:app`: Entry point for the Flask application (`wsgi` is the Python module containing the Flask app object `app`).

#### 3. Deployment Considerations:

- **Security:** Ensure secure communication over HTTPS by configuring SSL/TLS certificates.
- **Performance:** Optimize server configuration and application code for performance.
- **Monitoring:** Implement logging and monitoring solutions to track server performance and diagnose issues.
- **Scalability:** Design the deployment architecture to scale horizontally or vertically based on demand.
- **Backup and Recovery:** Regularly back up data and implement disaster recovery strategies.

By following these best practices, you can deploy your Flask application securely and efficiently on a production server, making it accessible to users over the internet.
