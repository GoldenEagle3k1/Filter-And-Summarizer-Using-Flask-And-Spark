# Filter-And-Summarizer-Using-Flask-And-Spark

PYSPARK & FLASK STUDENT PERFORMANCE API

This repository contains a Flask-based REST API powered by Apache Spark (PySpark). The application loads student performance data, calculates summary statistics, filters records, and serves predictions for final grades using a trained Spark MLlib Linear Regression model.

The code is specifically configured to run within a Google Colab environment, utilizing Colab's proxy port feature to expose the local Flask server to the public web.

FEATURES

- Big Data Processing: Uses PySpark DataFrames and Spark SQL to process and aggregate dataset queries.
- Machine Learning: Trains a PySpark MLlib Linear Regression model on startup to predict a student's final grade (G3) based on historical performance and attendance features.
- REST API Endpoints: Provides distinct endpoints for data summarization, customized filtering, and real-time inference.

PREREQUISITES

To run this application, you need:
- A Google Colab notebook environment.
- The maths1.csv dataset uploaded to the root directory of your Colab instance. The dataset must contain the following columns: studytime, failures, absences, G1, G2, G3, internet, sex, school, and age.
- Required Python packages: flask, pyspark.

SETUP AND EXECUTION

1. Install Dependencies: Run the following command in a Colab cell if PySpark is not already installed:
   pip install pyspark flask

2. Upload Data: Upload your maths1.csv file into the Colab file explorer.

3. Run the Script: Paste the provided Python code into a cell and execute it.

4. Access the API: The console will output a proxy URL. Click this link to interact with the API.

API ENDPOINTS

1. Health Check
- URL: /
- Method: GET
- Description: Verifies that the API is running.
- Response: Plain text confirming the server status.

2. Student Summary
- URL: /api/students/summary
- Method: GET
- Description: Returns aggregated student statistics using Spark SQL. It calculates the total number of students and their average final grade (G3), grouped and ordered by studytime.
- Response Format: JSON
- Example Response:
  {
    "status": "success",
    "data": [
      {"studytime": 1, "total_students": 105, "avg_final_grade": 10.04},
      {"studytime": 2, "total_students": 198, "avg_final_grade": 10.17}
    ]
  }

3. Filter Students
- URL: /api/students/filter
- Method: GET
- Description: Filters the dataset based on internet access and gender, returning a subset of columns (school, sex, age, internet, studytime, G3).
- URL Parameters:
  - internet (String, Optional): Filter by internet access. Default is yes.
  - sex (String, Optional): Filter by gender. Default is F.
- Example Request: /api/students/filter?internet=yes&sex=M
- Response Format: JSON

4. Predict Final Grade
- URL: /api/predict
- Method: POST
- Description: Accepts student features and returns a predicted final grade (G3). The application automatically bounds the predicted score between 0 and 20.
- Request Headers: Content-Type: application/json
- Request Body JSON:
  {
    "studytime": 3,
    "failures": 0,
    "absences": 2,
    "G1": 14,
    "G2": 15
  }
  Note: If parameters are omitted, the API defaults to studytime=2, failures=0, absences=0, G1=10, G2=10.
- Response Format: JSON
- Example Response:
  {
    "status": "success",
    "predicted_G3_grade": 15.34
  }

TECHNICAL DETAILS

- Model Features: studytime, failures, absences, G1 (Period 1 Grade), G2 (Period 2 Grade).
- Target Variable: G3 (Final Grade).
- Vectorization: Features are combined into a single vector using PySpark's VectorAssembler before model training and inference.
