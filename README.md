# 🎮 Steam Game Recommendation System

### Machine Learning Project — BRAC University CSE427 

A machine learning–based **Steam Game Recommendation System** developed for **CSE427: Machine Learning** at BRAC University by Ibrahim, Nahiyan and Hamim. 

The project investigates multiple recommendation approaches for personalized game discovery using large-scale Steam user–game interaction data. We compare traditional baselines, collaborative filtering, learning-to-rank models, hybrid approaches, and cold-start techniques to determine how effectively different methods can recommend games to users.

---

## 📌 Project Overview

Steam contains a massive amount of user-generated interaction data, making it possible to learn users' gaming preferences from their historical activity.

The main goal of this project is to answer:

> **Can machine learning models use users' historical Steam activity to generate relevant personalized game recommendations?**

To investigate this, we developed and evaluated **13 different recommendation approaches**, including:

* Popularity-based recommendation
* SVD-based collaborative filtering
* Implicit ALS
* LambdaRank-based Learning-to-Rank models
* Tag-to-factor cold-start mapping
* Negative-feedback-aware ALS
* Hybrid recommendation models
* Two-stage retrieval and ranking

The models were evaluated using ranking-based metrics such as **Recall@10** and **NDCG@10**.

---

## 📊 Dataset

The project uses a large-scale public Steam recommendation dataset.

### Dataset Statistics

| Statistic                 |      Value |
| ------------------------- | ---------: |
| Recommendation records    | 41,154,794 |
| Games                     |     50,872 |
| Users                     | 14,306,064 |
| Modelling rows            |  3,200,557 |
| Modelling users           |     55,898 |
| Modelling games           |     10,738 |
| Held-out evaluation users |        572 |

A full-file preprocessing pipeline was used before constructing the modelling dataset.

The modelling data uses a cutoff date of:

**2021-11-02**

---

## ⚙️ Machine Learning Pipeline

The overall workflow of the project is:

```text
Steam Recommendation Dataset
            │
            ▼
      Data Processing
            │
            ▼
      Data Cleaning
            │
            ▼
    Feature Engineering
            │
            ▼
   Train / Test Construction
            │
            ▼
 ┌───────────────────────────┐
 │   Multiple ML Models      │
 │                           │
 │ • Popularity              │
 │ • SVD                     │
 │ • Implicit ALS            │
 │ • LambdaRank              │
 │ • Hybrid Models           │
 │ • Cold-Start Models       │
 │ • Two-Stage Retrieval     │
 └───────────────────────────┘
            │
            ▼
     Top-N Recommendations
            │
            ▼
       Model Evaluation
            │
            ▼
 Recall@10 / NDCG@10
```

---

## 🤖 Models Evaluated

A total of **13 recommendation methods** were implemented and evaluated.

### Baselines

* **Popularity-based recommender**
* **SVD collaborative filtering**
* Additional baseline recommendation approach

### Collaborative Filtering

* **Implicit ALS**
* ALS using confidence weighting based on user activity
* ALS incorporating negative feedback

### Learning-to-Rank

Several **LambdaRank** variants were evaluated to determine whether explicit ranking objectives could improve recommendation quality.

### Cold-Start

A **tag-to-factor mapping** approach was developed to generate recommendations for situations where sufficient user interaction history was unavailable.

### Hybrid Approaches

Multiple hybrid models were evaluated by combining signals from different recommendation approaches.

### Two-Stage Retrieval

A two-stage recommendation pipeline was also investigated, separating candidate retrieval from final ranking.

---

## ⭐ Final Model

The final selected model was:

### **M1 — Implicit ALS with Hours × Helpfulness Confidence Weighting**

The model uses implicit user feedback and assigns confidence based on interaction signals such as:

* Playtime
* Recommendation helpfulness

This allows stronger user interactions to contribute more strongly to the learned user and game representations.

---

## 📈 Results

The final model achieved:

| Metric        |      Score |
| ------------- | ---------: |
| **Recall@10** | **0.0400** |
| **NDCG@10**   | **0.1218** |

### Comparison with SVD

Compared with the SVD baseline:

* **Recall@10:** +0.0119

* 95% CI: `[+0.0045, +0.0204]`

* Holm-adjusted p-value: **0.006**

* **NDCG@10:** +0.0254

* 95% CI: `[+0.0138, +0.0377]`

* Holm-adjusted p-value: **0.0004**

### Comparison with Popularity Baseline

Compared with the popularity baseline:

* **Recall@10:** +0.0153

* 95% CI: `[+0.0070, +0.0244]`

* Holm-adjusted p-value: approximately **2 × 10⁻⁵**

* **NDCG@10:** +0.0452

* 95% CI: `[+0.0313, +0.0594]`

* Holm-adjusted p-value: approximately **5 × 10⁻⁸**

These results indicate that the learned recommendation model captured useful personalized signals beyond simply recommending globally popular games.

---

## 🧪 Evaluation Methodology

The recommendation systems were evaluated using **full-catalogue Top-N recommendation** rather than restricting evaluation to a small candidate sample.

The evaluation was performed on:

**572 held-out users**

Two primary ranking metrics were used:

### Recall@10

Measures how many of the relevant games were successfully retrieved within the top 10 recommendations.

```text
Recall@10 =
Relevant games recommended in Top 10
-------------------------------------
Total relevant games
```

### NDCG@10

Measures recommendation quality while giving greater importance to relevant games appearing near the top of the recommendation list.

---

## 📊 Statistical Testing

To make model comparisons more robust, paired statistical analysis was performed.

The evaluation included:

* **10,000-resample paired bootstrap**
* Confidence intervals
* **Wilcoxon signed-rank tests**
* **Holm multiple-comparison correction**

This allowed differences between recommendation models to be evaluated across the same held-out users rather than relying only on individual metric values.

---

## 🥶 Cold-Start Recommendation

One of the challenges addressed by the project is the **cold-start problem**.

A new user may have little or no interaction history, making traditional collaborative filtering difficult.

The project investigated cold-start recommendations using a user's initial preferences.

The cold-start user setup supports recommendations from as few as:

**5 seed liked games**

Cold-start games were handled through a dedicated recommendation approach using game metadata/tag information.

---

## 📈 Long-Tail Coverage

The project also investigated whether recommendations were limited to only the most popular games.

The final recommendation system achieved approximately:

| System              | Long-Tail Coverage |
| ------------------- | -----------------: |
| Popularity Baseline |              0.002 |
| Proposed Model      |              0.116 |

This demonstrates substantially broader coverage of less-popular games compared with popularity-based recommendation.

---

## 🔍 Key Findings

The experiments provided evidence for several important observations:

* Personalized collaborative filtering can outperform simple popularity-based recommendation.
* Implicit user interactions provide useful signals for learning game preferences.
* Ranking-based approaches can improve recommendation quality in some settings.
* Hybrid recommendation strategies can combine complementary recommendation signals.
* Cold-start users can be supported using a small number of initial preferences.
* Personalized recommendations provide substantially greater long-tail coverage than popularity-based recommendations.
* Explicit negative feedback was investigated, but incorporating it did **not** produce a measurable ranking improvement in the tested setup.

---

## 🛠️ Technologies Used

The project was developed using Python and common machine learning/data science tools.

### Programming

* Python

### Machine Learning

* Implicit Collaborative Filtering
* Alternating Least Squares (ALS)
* Singular Value Decomposition (SVD)
* LambdaRank
* Learning-to-Rank
* Hybrid Recommendation
* Matrix Factorization

### Evaluation

* Recall@K
* NDCG@K
* Bootstrap Confidence Intervals
* Wilcoxon Signed-Rank Test
* Holm Correction

### Data Processing

* Large-scale CSV processing
* Feature engineering
* User–item interaction modelling
* Train/test construction

---

## 📁 Project Structure

The repository contains the implementation, experiments, preprocessing pipeline, evaluation scripts, and project documentation.

```text
Steam-Game-Recommendation/
│
├── data/
│   └── ...
│
├── notebooks/
│   └── ...
│
├── src/
│   └── ...
│
├── models/
│   └── ...
│
├── results/
│   └── ...
│
├── requirements.txt
├── README.md
└── ...
```

> The exact contents of each directory depend on the files included in this repository.

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/IFBAK/Steam-Game-Recommendation--A-Machine-learning-project-BRACU-CSE427-.git
```

### 2. Navigate to the project

```bash
cd Steam-Game-Recommendation--A-Machine-learning-project-BRACU-CSE427-
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the project

Follow the relevant notebooks/scripts in the repository for:

1. Data preprocessing
2. Feature engineering
3. Model training
4. Recommendation generation
5. Evaluation

---

## 👥 Contributors

This project was developed as a group project for:

**CSE427 — Machine Learning**
**BRAC University**

* **IFBAK**
* **Hamim504**
* **mohammadnahiyanrahman**


---

## 🎓 Academic Context

This project was developed as part of the **CSE427: Machine Learning** course at **BRAC University**.

The project focuses on applying machine learning concepts to a real-world recommendation problem using large-scale user–item interaction data.

---

## 📚 References

The project builds upon concepts from:

* Collaborative Filtering
* Matrix Factorization
* Implicit Feedback Recommendation
* Learning-to-Rank
* Cold-Start Recommendation
* Information Retrieval
* Recommender System Evaluation

---

## 📄 License

This repository is intended primarily for **academic and educational purposes** as part of the BRAC University CSE427 Machine Learning course project.
# Steam-Game-Recommendation--A-Machine-learning-project-BRACU-CSE427-
A Comparative Analysis of Multi-Signal and Cold-Start Models on Steam Games Recomendations using Machine Learning 

 **DATASET LINK**
 https://www.kaggle.com/datasets/antonkozyriev/game-recommendations-on-steam


 
 **PROJECT PRESENTATION**
 https://www.youtube.com/watch?v=uRJKa9q61CI&list=LL&index=4&t=555s
 
