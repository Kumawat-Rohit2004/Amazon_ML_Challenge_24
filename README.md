# Amazon Product Attribute Extraction System 🛒✨
![image](https://private-user-images.githubusercontent.com/123946762/369492982-92e82681-f450-4cd8-b5b3-7405a4a72fef.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjkwODY5OTEsIm5iZiI6MTc2OTA4NjY5MSwicGF0aCI6Ii8xMjM5NDY3NjIvMzY5NDkyOTgyLTkyZTgyNjgxLWY0NTAtNGNkOC1iNWIzLTc0MDVhNGE3MmZlZi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYwMTIyJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MDEyMlQxMjU4MTFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT01OWYwMGZkNTlkYzVlMTliNDY0YjNjNjkzNTllZmYyZDA1Y2JkZTA5YzhkYjQ2ZDZmMmM0ZTE1NjcwZDc4NjM4JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.1aCJj7kwVt041hontKHSpcgEZFq__aU4CAiEo1RM0Dk)

### AIR-200
This repository contains a robust system (built from absolute scratch without the use of any pre-trained model) for extracting and predicting key product attributes such as weight, volume, length, voltage, wattage, etc., from product images using Optical Character Recognition (OCR) based on a given query. The system efficiently predicts the correct units and numerical values associated with these attributes, providing a scalable solution for automated product data extraction.

## Problem Statement ❓
The dataset, provided by Amazon, includes product images containing information such as weight, length, volume, voltage, wattage, etc., with varying units within the same query (e.g., weight - kg, gm, mg, pounds). The challenge is to extract this information from the images based on the given query and accurately predict the correct units and numerical values.

## Approach 🚀
The system follows a multi-stage approach:

### 1. OCR for Text Extraction
EasyOCR is used to extract text from the product images. The extracted text includes a mixture of numeric values and units (e.g., "15.2 cm", "250 gm").
### 2. Text Cleaning and Preprocessing 🧹
* The extracted text is cleaned and split into two sequences:<br>
  * **Unit Sequence:** Extracts the units (e.g., cm, gm, mg, etc.).<br>
  * **Value Sequence:** Extracts the corresponding numerical values (e.g., 15.2, 250).
### 3. Unit Prediction with LSTM Encoder-Decoder 🔄
* **LSTM Encoder:** Takes in the TF-IDF embeddings of the cleaned text sequence.<br>
* **LSTM Decoder:** Uses one-hot encoded sequences of units to predict the correct unit associated with each product.<br>
* **Softmax:** The output of the LSTM decoder is passed through a softmax layer to finalize the prediction of the unit.
### 4. Attention Mechanism for Unit-Value Association 🎯
* **Bahdanau Attention:** Applied to identify the most contributing unit within the sequence. The attention score is calculated for each unit.<br>
* **Unit with Highest Attention Score:** The numerical value corresponding to the unit with the maximum attention weight is chosen.
### 5. Numerical Value Prediction using GRU Encoder-Decoder and Self-Attention 🔍
* To ensure robustness in selecting the correct numerical value, we represent the correct value as a sequence (1 at the correct index and 0 elsewhere).<br>
  * **GRU Encoder:** Takes the attention weight sequence as input.<br>
  * **GRU Decoder:** Outputs the correct sequence (a sequence of zeroes and ones), where '1' indicates the correct index of the numerical value.<br>
* This step ensures that even if the unit prediction is slightly off, the system can still identify the correct numerical value.
## Model Architecture 🏗️
The model integrates multiple components to ensure high accuracy:<br>
* **LSTM Encoder-Decoder:** For unit prediction.<br>
* **Bahdanau Attention:** To associate units with their respective numerical values.<br>
* **GRU Encoder-Decoder:** For robust numerical value prediction using attention scores.
## System Flow 📈
1. **Text Extraction:** OCR is used to extract text from the product images.<br>
2. **Text Cleaning:** The extracted text is split into units and values.<br>
3. **Unit Prediction:** Using an LSTM Encoder-Decoder network, the correct unit is predicted from the cleaned text sequence.<br>
4. **Attention Scoring:** Attention scores are computed to associate units with their respective numerical values.<br>
5. **Numerical Value Prediction:** A GRU-based Encoder-Decoder system refines the prediction of the correct numerical value, ensuring robustness.
## Example Workflow 🔄
1. **Input Image:** A product image contains the text "15.2 cm, 250 gm".<br>
2. **OCR Output:** Extracts "15.2 cm, 250 gm".<br>
3. **Text Cleaning:** Separates into sequences [15.2, 250] for values and [cm, gm] for units.<br>
4. **LSTM Prediction:** Predicts the unit 'gm' for the product.<br>
5. **Attention Mechanism:** Identifies that 'gm' has the highest attention score.<br>
6. **GRU Prediction:** Predicts that '250' is the correct numerical value for 'gm'.
