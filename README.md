Appendix 1: Trip Time Predict System Set Up
The trip time prediction system integrates several libraries and models to implement the prediction service. This table provides a comprehensive overview of the main code segments and their functionalities within the project, detailing their purpose to the overall system.
Steps
Description
Core Library Imports
Imports fundamental Python libraries such as pandas (data manipulation), numpy (fonumerical operations), matplotlib and seaborn (data visualization), and folium (interactive mapping). These are the necessary libraries for data processing, analysis, and presentation throughout the project.
Unzip and load the Raw Dataset
Unzips the compressed raw dataset to make the raw train.csv file accessible, and loads the CSV file into a pandas dataframe named taxidata.
Data Preprocessing and Feature Engineering
Performs crucial data cleaning and transformation. This includes converting UNIX timestamps into human-readable date and time components (hour, day, month, year), extracting geographical pickup and drop-off coordinates, and calculating the polyline length for each trip.
Save Processed Data
Stores the dataframe to the preprocessed taxi trip data pickle file named taxidata_processed.pkl. It could reload the same processed data into a dataframe when restarting the system, without retraining the raw data, reducing reprocessing time.
Initialize OSMnx Graph Networks
Install two OpenStreetMap graph objects using the OSMnx library for 'Porto, Portugal'. G is created for driving routes, and G_ds is created for a more comprehensive network including public transport routes. These graphs are used for route calculation, distance measurement, and visualization.
Estimate Trip Time Function Definition
Defines the estimate_trip_time function, which is the core logic for predicting trip duration. It attempts to find an estimate based on historical data within a given proximity and temporal context. If the historical data is missing, it falls back to a machine learning model for prediction.
Calculate Distance and Direction Function
Defines the calculate_distance_and_direction_osm function. This function utilizes OSMnx and NetworkX to determine the shortest route distance, the initial bearing, and direction between two specified geographic coordinates using either the G (driving route) or G_ds (public transport route) graphs.
Machine Learning Data Preparation
Prepares the preprocessed data for machine learning by defining the feature set, such as length, hours, and day,  and the target variable like trip time. It then applies StandardScaler to normalize the feature matrix, which is an important step for optimal performance.
Split Training and Testing Data
Divides the prepared feature matrix (X) and target vector (y) into training and testing subsets using the train_test_split command. I have split the data into 70% for training and 30% for testing to enable robust model training and evaluation.
Train Machine Learning model
Trains a different machine learning regressor using the X_train and y_train datasets to compare results and select the model with higher accuracy (Linear Regression and Random Forest).
Save Trained Machine Learning Models
Saves the trained machine learning model and the fitted StandardScaler to pickle files. This allows the Flask application to load and use these pre-trained models without retraining them each time the application starts.
Flask Web App Setup (Linear Regression)
Sets up a Flask web application and defines various routes. This version integrates prediction and mapping functionality into a user-friendly interface, primarily using historical data and OpenStreetMap results for trip-time estimation. Linear Regression model as a fallback
Flask Web App Setup (Random Forest)
Another version of the Flask web application setup that largely mirrors the Linear Regression version but integrates the Random Forest model and a StandardScaler to replace Linear Regression for enhanced trip time prediction.

Appendix 2: System Usage Instructions
This system provides functionalities for estimating taxi trip times, calculating distances and directions, visualizing trip data, and analyzing trip factors. The core interaction is through a Flask web interface.
Accessing the Web Interface:
Run the Flask application cells.
A URL will be displayed in the Python code output, using the Google proxy port if the system is running on Google Colaboratory. Click this link to open the web interface in a new browser tab.
Main Page:
Estimate Trip Time: Navigate to the form for predicting trip duration.
Calculate Distance and Direction: Navigate to the form to calculate route specifics and visualize routes.
Show Map: Navigate to the form for generating heatmaps or marker maps of trip pickup locations.
Estimate Trip Time:
Input: Provide either start/end latitude/longitude coordinates or place names in Portugal.
Output: Displays the estimated trip time in seconds, the number of historical trips used for estimation, standard deviation, and the source of the prediction, whether historical data or a machine learning model. It also includes tips on the busiest/least busy hours, and a Folium map showing the driving and public transport routes.
Calculate Distance and Direction:
Input: Provide either start/end latitude/longitude coordinates or place names in Portugal.
Output: Provides the estimated trip distance, initial bearing, and direction for both driving routes and public transport routes. A Folium map visualizes both routes.
Show Map:
Input: provide a day (0 to 6), call_type (A, B, or C), and type_map (heatmap and markers map).
Output: Displays an interactive Folium map showing either a heatmap of pickup locations or individual markers for a specified number of trips based on the chosen filters.
