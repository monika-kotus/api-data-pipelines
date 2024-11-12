# api-data-pipelines
# **Building a Data Pipeline with APIs: Weather and Flight Data**

This project demonstrates how to create a data pipeline that collects, processes, and stores weather and flight data using APIs. The goal is to automate the process of fetching data daily, transforming it, and securely storing it in a cloud database (Google Cloud SQL).

## **Project Overview**

The pipeline consists of several stages, including:

1. **Collecting Data**: Data is fetched from APIs (OpenWeather API for weather data and Aerodatabox API for flight data).
2. **Cleaning and Transforming Data**: Data is cleaned and transformed to match the format required for storage.
3. **Storing Data**: The processed data is inserted into a MySQL database hosted on Google Cloud SQL.
4. **Automating the Pipeline**: The entire process is automated using Google Cloud Functions, with a daily schedule to refresh the data.

Throughout the project, I encountered several challenges, particularly with handling missing data, troubleshooting issues with GCP Cloud Functions, and ensuring smooth deployment and automation.

## **Key Features**
- **Weather Data Collection**: Fetching temperature, humidity, wind speed, and rain data from the OpenWeather API for multiple cities.
- **Flight Data Collection**: Fetching scheduled arrival times and flight details from the Aerodatabox API.
- **MySQL Database**: Storing the collected data in Google Cloud SQL using MySQL.
- **Cloud Functions**: Deploying the process to Google Cloud as a serverless function that runs on a daily schedule.
- **Automation**: The data pipeline runs automatically, ensuring that the data is up-to-date at all times.

## **Technologies Used**
- **Python**: For writing scripts to interact with APIs and handle data.
- **Google Cloud Platform (GCP)**: Used for Cloud Functions, Cloud SQL, and Cloud Scheduler.
- **MySQL**: For structured storage of weather and flight data.
- **pandas**: For data manipulation and transformation.
- **requests**: For making HTTP requests to the APIs.
- **functions-framework**: For running Google Cloud Functions locally before deployment.

## **Challenges & Solutions**

### **1. Handling Missing Data**
One of the key challenges was managing missing data from the APIs. Not all flight or weather records had the same structure, and fields like `arrival_time` or `flight_number` were occasionally missing. I used Python's `.get()` method to handle missing values gracefully, ensuring the process wouldn't fail if certain fields were missing.

### **2. Connecting to Google Cloud SQL**
When I attempted to connect my Python code to Google Cloud SQL, I encountered an issue related to permissions. This was resolved by configuring the right Cloud SQL Admin API permissions and enabling the correct API settings in Google Cloud.

### **3. Deploying to Google Cloud Functions**
The process of setting up Google Cloud Functions was initially frustrating. One critical issue was troubleshooting Cloud Functions deployment errors, particularly the connection between my local development environment and the Cloud SQL instance. After correctly configuring the permissions and connection settings, the function worked as expected.

### **4. Automating the Pipeline**
I automated the pipeline using Google Cloud Scheduler to ensure the functions run daily without manual intervention. This process involved setting the correct trigger interval and ensuring the Cloud Functions were deployed and connected properly.


## Future Improvements

- **Error Handling**: Improving error handling to handle different types of API failures.
- **More Data Sources**: Adding more data sources to the pipeline, such as airport information or additional weather metrics.
- **Performance**: Optimizing data processing and function execution times for better performance and cost efficiency.

## Conclusion

Building this data pipeline project has not only helped me automate the process of gathering and storing weather and flight data, but it has also given me a deeper understanding of cloud infrastructure, API handling, and automation. It’s been a challenging yet rewarding experience that pushed me to solve problems, learn new technologies, and streamline workflows.

By combining data from APIs, cloud storage, and automation, I’ve created a system that continuously refreshes data on a daily basis, saving time and effort, and ensuring that the information I work with is always up to date.

