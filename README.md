# Dry Bean Variety Classification using KNN

This project implements a K-Nearest Neighbors (KNN) classifier to categorize 7 different types of dry beans based on 16 morphological features (such as Area, Perimeter, and Shape Factors). The model achieves a robust average accuracy of 91.7%.

## 🚀 Key Highlights

* **Preprocessing Mastery**: Corrected a UnicodeDecodeError and handled a mislabeled `.xlsx` to `.csv` file.
* **The Scaling Factor**: Implemented `StandardScaler` to normalize feature ranges, boosting accuracy from 79.3% to 91.6%.
* **Reliability**: Validated results using 5-Fold Stratified Cross-Validation, ensuring the model generalizes well with a standard deviation of only 0.0043.

## 🛠️ The Debugging Journey: Challenges & Solutions

During this project, we encountered several "real-world" data problems that required technical troubleshooting. Documenting these is essential for understanding the data pipeline:

### 1. The Renaming Trap (Excel vs. CSV)

* **Problem**: The dataset was labeled as `.csv`, but attempting to load it via `pd.read_csv()` resulted in a `ParserError`.
* **Discovery**: By inspecting the file header, we discovered the file was actually an Excel XML structure (.xlsx) that had been manually renamed to `.csv`.
* **Solution**: We used the `openpyxl` engine and `pd.read_excel()` to correctly parse the binary data.

**Code Fix**:
```python
# Wrong approach that caused error
# df = pd.read_csv('Dry_Bean_Dataset.csv')

# Correct approach
df = pd.read_excel('Dry_Bean_Dataset.xlsx', engine='openpyxl')
```

### 2. Unicode & Encoding Errors

* **Problem**: Initial attempts to read the text versions of the data triggered a `UnicodeDecodeError`.
* **Discovery**: The file used Latin-1 encoding rather than the standard UTF-8, causing crashes when encountering special characters.
* **Solution**: Explicitly defined the encoding as `latin1` in the pandas loading function to ensure data integrity.

**Code Fix**:
```python
# Wrong approach that caused error 
# df = pd.read_csv('data.csv')

# Correct approach
df = pd.read_csv('data.csv', encoding='latin1')
```

## 📊 Performance Analysis: Why the Mispredictions?

While a 91.7% accuracy is excellent, the model occasionally "mispredicts" specific bean varieties. This isn't necessarily a failure of the code, but rather a reflection of the biological data.

### 1. The Similarity Trap (Sira vs. Dermason)

The most common error in this model occurs between the SIRA and DERMASON varieties.

* **The Reason**: These two bean types are morphologically almost identical. They share very similar surface areas, perimeters, and roundness scores. In our high-dimensional space, their data points exist in the same "neighborhood," leading the KNN algorithm to occasionally swap them.
* **Evidence**: The Confusion Matrix shows a specific "hotspot" of errors between these two labels.

### 2. The Bombay "Outlier"

Conversely, the model achieves near 100% accuracy on the BOMBAY variety.

* **The Reason**: Bombay beans are significantly larger and have distinct length-to-width ratios compared to all other varieties. Because they are so mathematically different, they sit in an isolated part of the coordinate system where no other beans interfere.

## 🛠️ Technical Implementation

* **Language**: Python
* **Key Libraries**: `pandas`, `scikit-learn`, `seaborn`, `matplotlib`, `openpyxl`
* **Algorithm**: `KNeighborsClassifier`
   * n_neighbors: 3
   * Metric: 'manhattan' (L1 Norm) — Chosen for its robustness in high-dimensional datasets.

## 📂 File Structure

```
Dry-Bean-Classification-KNN/
├── dry_bean_classification.ipynb    # Primary notebook with data cleaning, visualization, and model training
├── Dry_Bean_Dataset.xlsx            # Original source data
├── requirements.txt                 # Project dependencies
└── README.md                        # Project documentation
```

## ⚙️ Installation & Usage

1. **Clone the repository**:
   ```bash
   git clone https://github.com/uditbh123/Dry-Bean-Classification-KNN.git
   cd Dry-Bean-Classification-KNN
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Open the notebook and run all cells** to reproduce the results and visualize the confusion matrix heatmap:
   ```bash
   jupyter notebook dry_bean_classification.ipynb
   ```

## 📈 Model Performance

| Metric | Value |
|--------|-------|
| Average Accuracy | 91.7% |
| Cross-Validation Folds | 5 (Stratified) |
| Standard Deviation | 0.0043 |
| Accuracy Before Scaling | 79.3% |
| Accuracy After Scaling | 91.6% |

## 🔍 Key Insights

- **Feature Scaling is Critical**: Implementing StandardScaler increased accuracy by over 12 percentage points
- **Distance Metrics Matter**: Manhattan distance (L1 norm) proved more robust than Euclidean distance for this high-dimensional dataset
- **Biological Overlap**: Model errors reflect real-world morphological similarities between bean varieties, not algorithmic failures
- **Class Imbalance**: Some varieties are easier to classify due to distinct physical characteristics

## 🤝 Contributing

Contributions are welcome! Feel free to open issues or submit pull requests.

## 📝 Conclusion

This project demonstrates the critical importance of Feature Scaling in distance-based algorithms. By understanding the physical similarities between bean types, we can interpret the model's errors not as flaws, but as logical outcomes of biological overlap.

## 📧 Contact

Udit - [@uditbh123](https://github.com/uditbh123)

Project Link: [https://github.com/uditbh123/Dry-Bean-Classification-KNN](https://github.com/uditbh123/Dry-Bean-Classification-KNN)

---

**Built with 🫘 and KNN**