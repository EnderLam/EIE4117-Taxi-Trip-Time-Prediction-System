# Trip Time Prediction System

An end-to-end trip time prediction system for taxi routes in **Porto, Portugal**. This project integrates historical spatial-temporal analysis, OpenStreetMap graph routing, machine learning regression models (Linear Regression and Random Forest), and an interactive Flask web interface.

---

## 🛠️ System Architecture & Workflow

The system processes raw taxi trip datasets, builds graph networks for routing, trains machine learning models, and serves predictions via a web application.

| Step | Component / Action | Description |
| :--- | :--- | :--- |
| **1** | **Core Library Imports** | Imports foundational libraries for data manipulation (`pandas`, `numpy`), visual analytics (`matplotlib`, `seaborn`), interactive mapping (`folium`), and spatial graphs (`osmnx`, `networkx`). |
| **2** | **Data Loading** | Unzips the raw dataset to extract `train.csv` and loads it into a Pandas DataFrame (`taxidata`). |
| **3** | **Preprocessing & Feature Engineering** | Converts UNIX timestamps to temporal attributes (hour, day, month, year), extracts pickup/drop-off coordinates, and calculates total polyline lengths. |
| **4** | **Data Persistence** | Serializes the processed DataFrame to `taxidata_processed.pkl` to bypass raw data processing on system restarts. |
| **5** | **OSMnx Graph Initialization** | Builds two OpenStreetMap graph objects for Porto, Portugal using `OSMnx`:<br>• `G`: Driving route network<br>• `G_ds`: Public transport route network |
| **6** | **Historical Trip Estimation** | Implements `estimate_trip_time()` to search for historical trips matching temporal and spatial proximity constraints. |
| **7** | **Distance & Bearing Calculation** | Implements `calculate_distance_and_direction_osm()` using `OSMnx` and `NetworkX` to compute shortest route distances, initial bearings, and directional headings across both graph networks. |
| **8** | **ML Data Preparation** | Constructs feature sets (trip length, hour of day, day of week) and normalizes them using `StandardScaler`. |
| **9** | **Train/Test Split** | Splits the feature matrix ($X$) and target vector ($y$) into **70% Training** and **30% Testing** subsets. |
| **10** | **Model Training** | Trains and evaluates two regression models for performance comparison:<br>• **Linear Regression** (Baseline)<br>• **Random Forest Regressor** (Advanced) |
| **11** | **Model Serialization** | Saves trained ML models and the fitted `StandardScaler` as `.pkl` files for instant loading within the web application. |
| **12** | **Flask Web Application** | Exposes the estimation engine and interactive mapping utilities via web interfaces (available in Linear Regression fallback and Random Forest variants). |

---

## 💻 System Usage Instructions

### 1. Launching the Application
1. Execute the Flask web application script/cells.
2. Locate the generated server URL in the console output (includes Google Colab proxy port mapping if running on Colaboratory).
3. Click the link to launch the application in your browser.

---

### 2. Features & Interfaces

#### ⏱️ Estimate Trip Time
* **Input:**
  * Start and End geographic coordinates (Latitude / Longitude) **OR** Place names within Portugal.
* **Output:**
  * Estimated trip duration (in seconds).
  * Number of historical trips referenced, standard deviation, and prediction source indicator (Historical Data vs. ML Fallback Model).
  * Traffic insight tips (busiest and least busy hours).
  * Interactive `Folium` map displaying both driving and public transport routes.

#### 🗺️ Calculate Distance and Direction
* **Input:**
  * Start and End geographic coordinates (Latitude / Longitude) **OR** Place names within Portugal.
* **Output:**
  * Estimated route distance, initial bearing, and compass direction for driving (`G`) and public transit (`G_ds`) routes.
  * Interactive `Folium` map rendering both routes side-by-side.

#### 📍 Show Map (Spatial Visualization)
* **Input Filters:**
  * **Day of Week:** `0` (Monday) to `6` (Sunday)
  * **Call Type:** `A`, `B`, or `C`
  * **Map Type:** `Heatmap` or `Markers Map`
* **Output:**
  * An interactive `Folium` map rendering either a density heatmap of pickup locations or individual trip marker pins based on your selected criteria.
