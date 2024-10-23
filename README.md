# Sea Ice Segmentation : Convolutional neural Networks(CNN) - Gray Level Co-occurence Matrix(GLCM)
Classification of sea ice and water in SAR imagery using a CNN and K-Means clustering for pseudo-labeling. It extracts image windows and GLCM features, normalizes and quantizes images, and generates a prediction map for visualization. This approach enhances the analysis and understanding of ice and water regions in the Sentinel-1 Satellite Imagery.

## 1. User Authentication
- **Purpose:** Establishes a connection with NASA's data services.
- **Details:** 
  - Utilizes the `grimp.NASALogin` class to create a login session.
  - Authenticates the user with a username and password, allowing access to remote datasets for subsequent data requests.

## 2. Data Retrieval
- **Purpose:** Fetches SAR imagery based on specific filters. 
- **Details:**
  - We utilize Greenland Ice sheet Mapping Project (GrIMP)-data products hosted by Nasa Snow and Ice Data Center(NSIDC) to retrieve SAR images
  - Uses the `grimp.cmrUrls` class to search for images within specified time frames (e.g., 2019 and 2022) and product types (e.g., sigma0 or gamma0).
  - We obtain an nisar image series according to bounding box,product type and time range specified

## 3. Geospatial Processing
- **Purpose:** Clips the retrieved images to focus on specific geographical areas.
- **Details:**
  - Creates a bounding box using geometrical bounds defined in a GeoDataFrame .
  - Employs the `subset` and `clip` methods to restrict images to the area of interest, enhancing data relevance for further analysis.

## 4. Feature Extraction & Label Mapping - GLCM & KMeans Clustering
- **Purpose:** extracts meaningful features for analysis.
- **Details:**
  - Utilizes the `extract_windows_and_features` function to create smaller windows for detailed analysis, calculating texture features using the Gray-Level Co-occurrence Matrix (GLCM).
  - Extracts properties such as contrast, dissimilarity, homogeneity, ASM (Angular Second Moment), energy, and correlation.
  - Organizes extracted windows and features into arrays suitable for training machine learning models using the `prepare_dataset` function.
  - K-Means Clustering: The code concatenates image windows and GLCM features into a single dataset and applies K-Means clustering with two clusters. It assumes that cluster 0 represents water and cluster 1 represents sea ice, mapping the labels accordingly.

## 5. Model Definition
- **Purpose:** Constructs a convolutional neural network (CNN) model for classification tasks.
- **Details:**
  - Creates two input branches: one for image windows and another for extracted GLCM features.
  - The image branch consists of several convolutional layers, max pooling layers, and dropout layers, flattening into a dense layer.
  - The GLCM features branch contains dense layers to process the numerical feature set.
  - Outputs from both branches are concatenated and passed through additional dense layers, culminating in a final output layer with a sigmoid activation function for binary classification.

## 6. Model Compilation and Summary
- **Purpose:** Finalizes the model setup and prepares it for training.
- **Details:**
  - Compiles the model using the Adam optimizer and binary cross-entropy loss function, suitable for binary classification tasks.
  - Prints a summary of the model architecture, providing insights into the number of layers
