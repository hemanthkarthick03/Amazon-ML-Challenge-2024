# 🏆 Amazon ML Hackathon 2024 - Live

## 🚀 **Feature Extraction from Images**

### 📝 **Problem Statement**

In this hackathon, the goal is to create a machine learning model that **extracts entity values from images**. This capability is crucial in fields like healthcare, e-commerce, and content moderation, where precise product information is vital.

As digital marketplaces expand, many products lack detailed textual descriptions. Hence, it becomes essential to obtain key details directly from images. These images provide important information such as:

- Weight 🏋️‍♂️
- Volume 💧
- Voltage ⚡
- Wattage 💡
- Dimensions 📏

This data is **critical for digital stores** to ensure accurate listings.

---

### 📊 **Data Description**

The dataset consists of the following columns:

- **index**: A unique identifier (ID) for the data sample.
- **image_link**: Public URL where the product image is available for download. 
  Example link: [https://m.media-amazon.com/images/I/71XfHPR36-L.jpg](https://m.media-amazon.com/images/I/71XfHPR36-L.jpg)  
  To download images, use the `download_images` function from `src/utils.py`. See the sample code in `src/test.ipynb`.
- **group_id**: Category code of the product.
- **entity_name**: Product entity name (e.g., `item_weight`).
- **entity_value**: Product entity value (e.g., `34 gram`).  
  **Note**: For `test.csv`, the `entity_value` column is absent as it is the target variable.

---

### 🔑 **Output Format**

The output file should be a CSV with **2 columns**:

- **index**: The unique identifier (ID) of the data sample, matching the test record index.
- **prediction**: A string formatted as `x unit`, where `x` is a float and `unit` is one of the allowed units (mentioned in the Appendix).

📌 **Example of valid predictions**:
- `"2 gram"`
- `"12.5 centimetre"`
- `"2.56 ounce"`

🚫 **Invalid cases**:
- `"2 gms"`
- `"60 ounce/1.7 kilogram"`
- `"2.2e2 kilogram"`

If no value is found in the image, return an empty string (`""`). Ensure that the output file contains the **same number of records as `test.csv`**.

---

### 📂 **File Descriptions**

#### 🔧 **Source Files**:

- **`src/sanity.py`**: Sanity checker to ensure the final output file passes formatting checks.
- **`src/utils.py`**: Contains helper functions for downloading images from the `image_link`.
- **`src/constants.py`**: Contains the allowed units for each entity type.
- **`sample_code.py`**: Sample code to generate an output file in the required format. Usage is optional.

#### 🗄️ **Dataset Files**:

- **`dataset/train.csv`**: Training file with labels (`entity_value`).
- **`dataset/test.csv`**: Test file without `entity_value`. Use this file to generate predictions and format the output to match `sample_test_out.csv`.
- **`dataset/sample_test.csv`**: Sample test input file.
- **`dataset/sample_test_out.csv`**: Sample outputs for `sample_test.csv` (predictions may not be correct).

---

### ⚠️ **Constraints**

You must format your output file exactly like the sample output file. Pass it through the **sanity checker** (`src/sanity.py`) to ensure its validity.

If the file does not pass, it won't be evaluated. You should receive a message like:
```bash
Parsing successful for file: ...csv
```

📋 Your outputs **must use the allowed units** specified in `src/constants.py` and the Appendix. Any other units will be considered invalid.

---

### 📈 **Evaluation Criteria**

Submissions will be evaluated based on the **F1 score**, a standard measure of prediction accuracy.

Given:
- **GT** = Ground truth value for a sample
- **OUT** = Output prediction from the model for a sample

The predictions will be classified into the following categories:

- **True Positives**: If `OUT != ""` and `GT != ""` and `OUT == GT`
- **False Positives**: If `OUT != ""` and `GT != ""` and `OUT != GT`
- **False Positives**: If `OUT != ""` and `GT == ""`
- **False Negatives**: If `OUT == ""` and `GT != ""`
- **True Negatives**: If `OUT == ""` and `GT == ""`

#### 📊 **F1 Score Formula**:
\[
F1 \, score = \frac{2 \cdot Precision \cdot Recall}{Precision + Recall}
\]
Where:
- **Precision** = True Positives / (True Positives + False Positives)
- **Recall** = True Positives / (True Positives + False Negatives)
