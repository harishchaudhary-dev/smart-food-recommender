# 🥗 Smart Food Recommender System

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.28%2B-FF4B4B.svg)](https://streamlit.io/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3%2B-F7931E.svg)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An end-to-end Machine Learning-powered recommendation engine that delivers personalized dietary and food item recommendations based on full-spectrum nutritional profiling (35+ macro and micronutrients), categorical clustering, and natural language intent parsing.

The system utilizes a **Content-Based Filtering** approach powered by $k$-Nearest Neighbors ($k\text{-NN}$) over a unified, multi-modal vector space.

---

## 📌 Table of Contents

- [Architecture & Design Philosophy](#-architecture--design-philosophy)
- [Key Features](#-key-features)
- [Machine Learning Pipeline](#-machine-learning-pipeline)
- [Dataset Overview](#-dataset-overview)
- [Project Directory Structure](#-project-directory-structure)
- [Source Code & Implementation](#-source-code--implementation)
- [Getting Started](#-getting-started)
- [Usage Guide](#-usage-guide)
- [Future Enhancements](#-future-enhancements)
- [License](#-license)

---

## 🏗️ Architecture & Design Philosophy

Traditional food recommendation engines rely heavily on simple filtering (e.g., "High Protein only"). This project solves the **multi-objective optimization problem** in personalized nutrition by combining:

1. **Unstructured Text Processing (TF-IDF)**: Extracts semantic meaning from free-text user preferences.
2. **One-Hot Categorical Encoding**: Prevents implicit order assumptions across discrete food categories.
3. **Continuous Feature Standardization (Z-score Scaling)**: Normalizes 35+ micro/macronutrients with vastly different scales (e.g., grams vs. micrograms).
4. **Dynamic Quantile Vector Modification**: Dynamically adjusts query vector parameters according to fitness goals (e.g., shifting target protein to the 90th percentile of the dataset) before metric evaluation.

---

## 🔥 Key Features

- **🎯 Goal-Driven Dynamic Profiling**: Supports preset health goals including *Muscle Gain*, *Weight Loss*, *Heart Health*, and *Immunity Boosting*.
- **🥗 Custom Dietary Filters**: Easily apply constraints for Low Sugar, Low Fat, or Low Sodium diets.
- **💬 Natural Language Query Parsing**: Translates user inputs like *"light, healthy, easy to digest"* into term frequency vectors.
- **⚡ Dual Interface Capabilities**:
  - **Streamlit Web Dashboard**: Sleek UI featuring responsive recommendation cards and caching (`@st.cache_data` & `@st.cache_resource`) for fast vector lookup.
  - **CLI Mode**: Terminal-based script for quick integration and debugging.

---

## 🔬 Machine Learning Pipeline

```text
                                  ┌───────────────────────────┐
                                  │ Description (Text Input)  │ ──► TF-IDF Vectorizer (max_features=600)
                                  └───────────────────────────┘
                                                │
                                  ┌───────────────────────────┐
                                  │ Category (Discrete Group) │ ──► One-Hot Encoder (handle_unknown='ignore')
                                  └───────────────────────────┘
                                                │
┌──────────────────────┐                        │                        ┌───────────────────────────────────┐
│ Raw Input Data       │ ──► [ ColumnTransformer Preprocess Engine ] ──► │ Unified Dense/Sparse Matrix       │
│ (food.csv)           │                        │                        └───────────────────────────────────┘
└──────────────────────┘                        │                                          │
                                  ┌───────────────────────────┐                            ▼
                                  │ 35+ Nutrient Numerical    │ ──► StandardScaler         [ k-NN Engine ]
                                  │ Metrics                   │                            Metric: Cosine
                                  └───────────────────────────┘                            Neighbors: k=10
