# 🧠 MindSpace: Student Burnout Analytics

<p align="center">
  <img src="https://img.shields.io/badge/Made%20with-Python-blue.svg" alt="Made with Python">
  <img src="https://img.shields.io/badge/License-Apache%202.0-green.svg" alt="License: Apache 2.0">
  <img src="https://img.shields.io/badge/Version-2.0.0-orange.svg" alt="Version">
  <img src="https://img.shields.io/badge/Flask-Ready-blueviolet.svg" alt="Flask">
</p>

MindSpace is a comprehensive data analytics platform designed to help educators and students understand mental health patterns, burnout risks, and sentiment trends. By analyzing study habits, sleep patterns, and subjective feedback, MindSpace provides actionable insights via an intuitive web dashboard.

> **🎓 Academic Project**: MindSpace was developed as a minor project for analyzing student burnout in higher education settings using sentiment analysis and data visualization techniques.

## 📋 Table of Contents

- [🚀 Features](#-features)
- [🛠️ Tech Stack](#️-tech-stack)
- [📦 Installation](#-installation)
- [⚙️ Setup Guide](#️-setup-guide)
- [⚡ Quick Start](#-quick-start)
- [📊 Data Requirements](#-data-requirements)
- [💡 Usage](#-usage)
- [🎨 UI/UX Design](#-uiux-design)
- [📁 Project Structure](#-project-structure)
- [👥 Team](#-team)
- [🔮 Future Enhancements](#-future-enhancements)
- [📚 Research References](#-research-references)
- [🤝 Contributing](#-contributing)
- [🛡️ Security](#-security)
- [📄 License](#-license)
- [📧 Contact](#-contact)

## 🚀 Features

MindSpace offers a comprehensive suite of features for student burnout analysis:

- **📈 Data-Driven Insights**: Automatically calculates a "Burnout Score" based on study-to-sleep ratios and stress levels using a weighted algorithm that normalizes results to a 0-100 scale.

- **💬 Sentiment Analysis**: Utilizes Natural Language Processing (NLTK VADER Sentiment Analyzer) to analyze text feedback and gauge student sentiment, providing compound scores ranging from -1 (very negative) to +1 (very positive).

- **⚠️ Risk Classification**: Categorizes students into 'Low', 'Medium', and 'High' risk tiers for proactive intervention:
  - **🟢 Low Risk**: Burnout score ≤ 33
  - **🟡 Medium Risk**: Burnout score 34-66
  - **🔴 High Risk**: Burnout score > 66

- **📊 Interactive Dashboard**: Visualizes data through 10 charts covering:
  - Burnout score histogram, risk pie chart, stress vs burnout bar chart
  - Correlation heatmap, scatter plots (sleep, study, and stress vs burnout)
  - Burnout boxplot by risk tier, sentiment distribution, sentiment vs burnout
  - Each chart includes a plain-English explanation of what the pattern means

- **🔍 Searchable Data Table**: Full dataset shown with no row limit. Live search by any column and a risk-level dropdown filter.

- **🤖 Automatic Model Evaluation**: A Random Forest classifier trains on every uploaded dataset automatically. The Evaluation tab shows accuracy, precision, recall, F1, ROC-AUC, per-class breakdown, and confusion matrix. No CLI steps needed.

- **🔄 Dataset Comparison**: Upload a second CSV from the Compare tab to get a side-by-side breakdown including a delta table, risk tier cards, 4 comparison charts, and an auto-generated written verdict.

- **✏️ Dynamic Dataset Management**: 
  - Upload custom CSV files with real-time processing
  - Edit data directly through the web interface
  - Automatic recalculation of all metrics upon data changes

- **⚛️ React Query Data Management**:

  * Uses React Query for frontend server-state management and data fetching
  * Handles dataset upload and session-reset operations through mutations
  * Automatically invalidates the history query after successful uploads or session resets
  * Keeps history-related UI data synchronized with the latest server state


- **🔒 Robust Error Handling**: 
  - Division by zero protection in burnout calculations
  - Automatic data type conversion for numeric fields
  - Missing directory creation for visualizations
  - CSV format validation

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | React, TypeScript, React Query |
| **Backend** | Python, Flask |
| **Data Processing** | Pandas, NumPy |
| **Machine Learning** | scikit-learn (Random Forest) |
| **NLP** | NLTK (VADER Sentiment Analyzer) |
| **Visualization** | Matplotlib, seaborn |
| **Fonts** | Outfit (Google Fonts) |

## 📦 Installation

### Prerequisites

- Python 3.7 or higher
- pip (Python package installer)

### Step 1: Clone the Repository

```
bash
git clone <repository-url>
cd MindSpace
```

### Step 2: Create a Virtual Environment (Recommended)

```
bash
# On Windows
python -m venv venv
venv\Scripts\activate

# On macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies

```
bash
pip install -r requirements.txt
```

This will install the following packages:
- **Flask** - Lightweight web framework
- **pandas** - Data manipulation and analysis
- **numpy** - Numerical computing
- **nltk** - Natural language processing
- **matplotlib** - Data visualization
- **seaborn** - Statistical chart styling
- **scikit-learn** - Machine learning (Random Forest classifier and evaluation metrics)

### Step 4: Download NLTK Data

The application automatically downloads the VADER lexicon on first run. If you encounter issues, you can manually download it:

```
python
import nltk
nltk.download('vader_lexicon')
```

## ⚙️ Setup Guide

A walkthrough of getting MindSpace running end to end.

### 1. Clone and enter the project

```bash
git clone <repository-url>
cd mindspace
```

### 2. Create a virtual environment

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install all dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the app

```bash
python app.py
```

Then open `http://127.0.0.1:5000` in your browser.

### 5. Upload your first dataset

- From the Home page, click the upload area and select a `.csv` file
- The app processes it immediately: burnout scores are calculated, 10 charts are generated, and a Random Forest model trains on the data
- You are redirected to the Dashboard automatically

### 6. Explore the tabs

| Tab | What it does |
|---|---|
| Dashboard | Full charts, searchable data table, key statistics |
| Evaluation | Live model metrics for the uploaded dataset |
| Compare | Upload a second CSV for a side-by-side breakdown |
| Edit | Modify any row and regenerate everything |

### 7 (Optional). Run scripts manually

If you want to train or evaluate the model outside the web app:

```bash
cd scripts
python train_model.py   # trains and saves to ../models/
python evaluate.py      # evaluates from ../models/ and saves confusion matrix
```

---

## ⚡ Quick Start

1. **Run the application:**

```bash
python app.py
```

2. **Access the dashboard:**
   Open your browser and navigate to `http://127.0.0.1:5000`

3. **Upload your data:**
   - Prepare a CSV file with the required columns (see Data Requirements)
   - Use the web interface to upload the file
   - View the generated analytics on the dashboard

4. **Edit data (optional):**
   - Navigate to the Edit page
   - Modify any values directly
   - All calculations update automatically

## 📊 Data Requirements

The application expects a CSV file with the following columns:

| Column Name | Type | Description | Example Value |
| :--- | :--- | :--- | :--- |
| `student_id` (optional) | String | Unique student identifier | `S001` |
| `student_name` (optional) | String | Student full name | `Aanya Sharma` |
| `sleep_hours` | Float | Average nightly sleep hours | `7.5` |
| `study_hours` | Float | Daily study hours | `5` |
| `stress_level` | Integer | Stress rating on a scale of 1-10 | `6` |
| `feedback` | String | Qualitative text comments | `Feeling a bit overwhelmed.` |

### Sample Data Format

```
csv
sleep_hours,study_hours,stress_level,feedback
8,4,7,"I feel overwhelmed with assignments"
6,6,9,"Too much pressure, can't sleep"
9,3,4,"Managing okay, but tired"
7,5,8,"Burning out, need help"
10,2,3,"Feeling good, balanced"
5,7,10,"Exhausted, high stress"
```

### Sample Data Files

The repository includes sample datasets for testing:
- `data/sample_data.csv` - Example dataset with 6 records
- `data/test_data.csv` - Additional test data with diverse scenarios

## 💡 Usage

## ⚛️ Frontend Data Management

The MindSpace frontend uses React Query to manage server state and asynchronous data operations.

- **History fetching**: `useHistory()` uses React Query's `useQuery` to retrieve history data.
- **File uploads**: `useUploadFile()` uses `useMutation` to handle dataset uploads.
- **Session reset**: `useResetSession()` uses `useMutation` to reset the current session.
- **Automatic synchronization**: After a successful upload or session reset, the `history` query is invalidated automatically so the latest history data can be fetched without requiring a manual page reload.

This approach keeps frontend data synchronized with the backend while centralizing asynchronous operations in reusable hooks.


### Workflow

1. **Landing Page**: Start at the index page where you can upload a CSV file
2. **Data Upload**: Select and upload your CSV file containing student data
3. **Dashboard**: View the analytics dashboard with:
   - Burnout score distribution histogram
   - Risk category pie chart
   - Summary statistics (average burnout, high-risk count)
   - Individual student records with calculated metrics
4. **Edit Data**: Make changes to existing records and see real-time updates

### Burnout Score Calculation

The burnout score is calculated using the formula:

```
Burnout Score = (study_hours / sleep_hours) * stress_level * 10
```

- Clamped to a range of 0-100
- Higher scores indicate greater burnout risk
- Division by zero is handled gracefully (defaults to 0)

### Risk Classification

| Risk Level | Score Range | Action Recommended |
| :--- | :--- | :--- |
| 🟢 Low | 0-33 | Monitor periodically |
| 🟡 Medium | 34-66 | Consider intervention |
| 🔴 High | 67-100 | Immediate attention required |

### Sentiment Analysis

The application uses VADER (Valence Aware Dictionary and sEntiment Reasoner) for sentiment analysis:
- **Compound Score**: -1 (most negative) to +1 (most positive)
- **Positive**: Compound score ≥ 0.05
- **Negative**: Compound score ≤ -0.05
- **Neutral**: Compound score between -0.05 and 0.05

## 🎨 UI/UX Design

MindSpace features a modern, professional design:

- **Typography**: Inter font family for clean, readable text
- **Color Scheme**: 
  - Primary: Blue gradient (#1e3a8a → #3b82f6)
  - Background: White with subtle transparency
  - Cards: White with soft shadows
- **Responsive Design**: Works on desktop, tablet, and mobile devices
- **Interactive Elements**: Hover effects, smooth transitions
- **Data Visualization**: Clean charts with proper labeling

### Dashboard Features

- **Statistics Cards**: Display key metrics (Average Burnout, Total Students, High Risk Count)
- **Data Table**: Sortable, scrollable table with all student records
- **Charts**: Histogram for score distribution, Pie chart for risk categories
- **Navigation**: Easy access to edit functionality

## 📁 Project Structure

```
mindspace/
├── app.py                          # Flask routes, data processing, auto-training
├── requirements.txt                # Project dependencies
├── README.md                       # Project documentation
├── LICENSE                         # Apache License 2.0
├── SECURITY.md                     # Security policy
├── CODE_OF_CONDUCT.md              # Community standards
├── CONTRIBUTING.md                 # Contribution guidelines
├── Code-Explanation.md             # Detailed code documentation
├── scripts/                        # Standalone ML scripts
│   ├── train_model.py              # Manual training script
│   └── evaluate.py                 # Manual evaluation script
├── models/                         # Saved model artifacts (auto-generated)
│   ├── model.pkl
│   └── eval_data.pkl
├── data/                           # Dataset files
│   ├── sample_data.csv             # Sample dataset
│   └── updated_sample.csv          # Written after edits via web interface
├── documents/                      # Project documentation
│   ├── synopsis.md
│   ├── Workload-Distribution.md
│   └── Research-Paper/
├── templates/                      # HTML templates (all extend base.html)
│   ├── base.html                   # Shared sidebar layout and theme toggle
│   ├── index.html                  # Upload page
│   ├── dashboard.html              # Analytics dashboard with 10 charts
│   ├── evaluation.html             # Model metrics page
│   ├── compare.html                # Dataset comparison page
│   └── edit.html                   # Data editor
└── static/                         # Static assets
    ├── styles.css                  # Dark/light theme, all component styles
    └── plots/                      # Auto-generated chart images
        ├── score_hist.png
        ├── risk_pie.png
        ├── correlation_heatmap.png
        ├── confusion_matrix.png
        ├── cmp_burnout_hist.png    # Comparison charts (cmp_*.png)
        └── ...                     # (10 dashboard + 5 comparison charts total)
```

## 👥 Team

This project was developed by a team of 6 members:

| Role | Responsibilities |
| :--- | :--- |
| **Backend Developer** | Flask routes, data processing, error handling |
| **Frontend Templates Specialist** | HTML templates, Jinja2 integration |
| **Styling & UI/UX Designer** | CSS styling, responsive design |
| **Data & Testing Specialist** | Test data creation, end-to-end testing |
| **Deployment & Integration Lead** | Dependencies, integration, deployment |
| **Documentation & QA** | Documentation, code reviews, quality assurance |

## 🔮 Future Enhancements

We have exciting plans for MindSpace's future:

- [ ] **User Authentication**: Add login functionality for personalized dashboards
- [ ] **Export Options**: Export analyzed data to PDF/Excel reports
- [ ] **API Development**: RESTful API for programmatic access
- [ ] **Real-time Analytics**: Live data streaming and processing
- [ ] **Enhanced Visualizations**: Interactive charts with drill-down capabilities
- [ ] **Database Integration**: Support for SQL databases (PostgreSQL, MySQL)
- [ ] **Multi-language Support**: Localization for international users
- [ ] **Mobile Responsive Design**: Improved mobile experience
- [ ] **Email Notifications**: Automated alerts for high-risk students
- [ ] **LMS Integration**: Connect with learning management systems
- [ ] **Advanced NLP**: More nuanced sentiment analysis models
- [x] **Machine Learning**: Predictive burnout modeling (Random Forest, auto-trains on every upload)
- [x] **Dataset Comparison**: Side-by-side analysis of two cohorts
- [x] **Model Evaluation Dashboard**: Live accuracy, F1, confusion matrix in the web UI

## 📚 Research References

The MindSpace project is grounded in established research:

1. **Maslach, C., & Leiter, M. P. (2016)**. Understanding the burnout experience: recent research and its implications for psychiatry. *World Psychiatry*, 15(2), 103-111.

2. **Hutto, C. J., & Gilbert, E. E. (2014)**. VADER: A Parsimonious Rule-based Model for Sentiment Analysis of Social Media Text. *Proceedings of the Eighth International Conference on Weblogs and Social Media (ICWSM-14)*.

3. **Grinberg, M. (2018)**. *Flask Web Development: Developing Web Applications with Python*. O'Reilly Media.

4. **McKinney, W. (2017)**. *Python for Data Analysis*. O'Reilly Media.

5. **Bird, S., Klein, E., & Loper, E. (2009)**. *Natural Language Processing with Python*. O'Reilly Media.

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) for details on:

- Reporting bugs and suggesting features
- Development workflow
- Coding style guidelines
- Testing procedures

## 🛡️ Security

For information on how to report security vulnerabilities, please see our [Security Policy](SECURITY.md).

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0
```

## 📧 Contact

For questions, suggestions, or collaborations, please reach out:

- **Project Lead**: [@eOzkull/council](https://github.com/orgs/eOzkull/teams/council)
- **Email**: <inkeskdozing@gmail.com>
- **GitHub Issues**: [issues](https://github.com/Inkesk-Dozing/MindSpace/issues)

---

<p align="center">
  Made with ❤️ for student well-being<br>
  🧠 MindSpace - Understanding Student Burnout
</p>


## 📊 Evaluation Metrics

Metrics are computed automatically on each uploaded dataset. The values below reflect a run on the included `sample_data.csv`.

| Metric    | Score  |
|-----------|--------|
| Accuracy  | ~95%   |
| Precision | ~96%   |
| Recall    | ~95%   |
| F1 Score  | ~95%   |
| ROC-AUC   | computed per dataset |

> **Why these metrics?**
> F1-Score is the primary metric because class sizes can be uneven across datasets.
> Precision and Recall are both shown so you can see whether the model errs toward false alarms or missed cases.
> ROC-AUC reflects how well the model distinguishes between all three risk classes together.

The full per-class breakdown and confusion matrix are visible live in the app under the Evaluation tab after any upload.

![Confusion Matrix](static/plots/confusion_matrix.png)