# Student Exam Score Predictor - Anish Abhyankar

## Introduction About the Data :

**The dataset**  
The goal is to **predict math scores** for students based on demographic and academic factors (Regression Analysis).  
There are **7 independent variables**:

**Input Features:**
- `gender`: Male/Female
- `race_ethnicity`: Group A/B/C/D/E
- `parental_level_of_education`: Education level of parents
- `lunch`: Standard/Free-reduced lunch type
- `test_preparation_course`: Course completion status
- `reading_score`: Reading test score (0-100)
- `writing_score`: Writing test score (0-100)

**Target variable:**
- `math_score`: Math test score to predict (0-100)

**Key Observation:** Categorical variables are ordinal - parental education levels follow hierarchy

## Localhost Deployment Link :

**Flask Web App:** http://127.0.0.1:5000/


## API Testing (Local)

**Prediction Endpoint:** http://127.0.0.1:5000/predictdata

## Approach for the project

### Data Ingestion :
- CSV data loaded with pandas DataFrame
- Train/test split created and saved as artifacts
- Custom logging and exception handling implemented

### Data Transformation :
**ColumnTransformer Pipeline:**

Numeric Pipeline:
├── SimpleImputer(strategy='median')
└── reading_score, writing_score → StandardScaler

Categorical Pipeline:
├── SimpleImputer(strategy='most_frequent')
├── OrdinalEncoder (parental_education hierarchy)
└── gender, race_ethnicity, lunch, test_prep → OneHotEncoder

**Saved:** preprocessor.pkl

### Model Training :
- **Base models tested:** LinearRegression, RandomForestRegressor, XGBoost
- **Best model:** XGBoost Regressor after hyperparameter tuning
- **Evaluation:** R² Score, RMSE on validation set
- **Model saved:** model.pkl + preprocessor.pkl

### Prediction Pipeline :
- Converts form inputs → DataFrame with exact column order
- Loads preprocessor → transforms to model format
- Loads trained model → predicts math score
- Returns formatted prediction result

### Flask App Creation :
- **Home route** (`/`): Landing page with project info
- **Predict route** (`/predictdata`): Form inputs + score prediction
- Responsive HTML templates with Bootstrap styling
- Form validation and error handling



## Model Performance Results

| Feature Importance | Impact on Math Score |
|--------------------|---------------------|
| reading_score | High |
| writing_score | High |
| test_preparation_course | Medium |
| parental_education | Medium |
| lunch_type | Low |

**Current Performance:** R² = 0.88, RMSE = 4.2

## Future Improvements Planned

1. **Feature Engineering:** Score combinations, parental education encoding
2. **Ensemble Methods:** VotingRegressor (XGBoost + CatBoost)
3. **Cross-validation:** 5-fold time series CV
4. **Hyperparameter tuning:** RandomizedSearchCV expansion

**Target R²: 0.92+**
