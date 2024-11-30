# Amazon-ML-Challenge-2024
Our team's approach in Amazon ML Challenge 2024
[For detailed documentation, click here](./Amazon%20ML%20Challenge%202024%20Documentation.pdf)
> Team Aloo Parathaaa
> - Snehasish Haldar
> - Yash Desai
> - Rahul Pamnani
> - Pranith Prasad Poojary
## Problem Statement: 
### Feature Extraction from Images

In this hackathon, the goal is to create a machine learning model that extracts entity values from images. This capability is crucial in fields like healthcare, e-commerce, and content moderation, where precise product information is vital. As digital marketplaces expand, many products lack detailed textual descriptions, making it essential to obtain key details directly from images. These images provide important information such as weight, volume, voltage, wattage, dimensions, and many more, which are critical for digital stores.

### Dataset: 
The dataset is divided into two main files:

- **train.csv**: Contains over 310,000 image links along with metadata.
- **test.csv**: Contains over 132,000 image links along with metadata.
  
Each dataset has the following columns:

1. **index:** An unique identifier (ID) for the data sample
2. **image_link**: Public URL where the product image is available for download. Example link - https://m.media-amazon.com/images/I/71XfHPR36-L.jpg
3. **group_id**: Category code of the product
4. **entity_name:** Product entity name. For eg: “item_weight” 
5. **entity_value:** Product entity value. For eg: “34 gram”
    Note: For test.csv, you will not see the column `entity_value` as it is the target variable.

### File Descriptions:

*source files*

1. **src/sanity.py**: Sanity checker to ensure that the final output file passes all formatting checks. Note: the script will not check if less/more number of predictions are present compared to the test file. See sample code in `src/test.ipynb` 
2. **src/utils.py**: Contains helper functions for downloading images from the image_link.
3. **src/constants.py:** Contains the allowed units for each entity type.
4. **sample_code.py:** We also provided a sample dummy code that can generate an output file in the given format. Usage of this file is optional. 

*Dataset files*

1. **dataset/train.csv**: Training file with labels (`entity_value`).
2. **dataset/test.csv**: Test file without output labels (`entity_value`). Generate predictions using your model/solution on this file's data and format the output file to match sample_test_out.csv (Refer the above section "Output Format")
3. **dataset/sample_test.csv**: Sample test input file.
4. **dataset/sample_test_out.csv**: Sample outputs for sample_test.csv. The output for test.csv must be formatted in the exact same way. Note: The predictions in the file might not be correct

## Model Pipeline

### ⚙️ <span style="color:#2E8B57;">Approach Overview</span>

---

1. **🧹 Preprocessing**

   - Images are first converted to grayscale using OpenCV’s cvtColor() function. Grayscale conversion reduces complexity and enhances contrast, ensuring better OCR performance.
   - For images containing dimension-related entities (e.g., height, width, depth), a line detection algorithm is applied. Using OpenCV's cv2.Canny and cv2.HoughLinesP, bounding boxes are filtered to locate and align relevant numeric values and their units.

2. **🔍 Text Extraction (OCR)**

   - Multiple OCR tools were evaluated, including Tesseract, EasyOCR, and PaddleOCR. EasyOCR was selected for its speed and practicality, despite occasional blank outputs (~40% of the dataset).
   - OCR is applied to extract textual data from images, with each process optimized to complete in ~0.2 seconds per image.

3. **📑 Postprocessing**

   - ***Text Parsing:*** Custom regular expressions (regex) are employed to filter and extract numeric values alongside their respective units (e.g., kilograms, volts). These regex patterns account for decimals, commas, and multiple units in a sentence.
   - ***Unit Standardization:*** Extracted values are normalized to a common unit (e.g., converting cm to mm) for consistency in output.

4. **🧮 Rule-based Recognition**

   - A rule-based approach using regex ensures the identification of critical entities. For instance:
      - ***Weight:*** Extracts values with units like grams, kilograms, or pounds.
      - ***Voltage:*** Identifies values in volts, millivolts, or kilovolts.
      - ***Dimensions:*** Detects numeric values associated with units like cm, mm, or inches.
   - This method ensures accurate extraction without requiring a machine-learning-based model.

5. **✅ Final Output**

   - Extracted entities are finalized and stored in a structured CSV file, enabling seamless integration and analysis.
   - Each row contains the image index and corresponding entity values in a standardized format.

## Results

Our model achieved the following results:

- **F1 Score**: 0.4 (highest)
- **Rank**: 290 out of 2500+ teams (74,824 Registrations)

![teamAlooParathaa](https://github.com/user-attachments/assets/8ec5cee7-4a51-4aa9-8923-8303dd85d7d6)


## Conclusion 
To conclude, by utilizing EasyOCR for text extraction and implementing Custom Regex and Line Detection Algorithms, we efficiently addressed resource limitations while extracting key dimensions and values. This streamlined approach, with minimal GPU dependency, reduced processing time to ~0.2 seconds per image, enabling large-scale data generation in a practical and resource-efficient manner without relying on heavy vision models.  
The four-day hackathon, the longest we’ve participated in so far, posed significant challenges, particularly in processing the test.csv file, which required downloading over 52 GB of images and running them through an OCR model, a task that collectively took more than 20 hours. Despite these obstacles, this was an invaluable experience to work on a real-world problem faced by Amazon.  
We are immensely grateful to Amazon for hosting such an engaging hackathon and eagerly look forward to competing again next year!
