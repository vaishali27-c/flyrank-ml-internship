# Refresh / Content Opportunity Scoring Using FlyRank Search Intelligence

### AI & Machine Learning Capstone Research

**Author:** Vaishali Chatnalkar

**FlyRank ML Internship**

---

## 📄 Research Paper

➡️ **[Read the Complete Research Paper](work/capstone_report.md)**

⬇️ **[Download Research Paper (PDF)](work/capstone_report.pdf)**

---

## 💻 GitHub Repository

➡️ **[View Source Code on GitHub](https://github.com/vaishali27-c/flyrank-ml-internship)**

---

# Project Summary

This project presents a machine learning–based content opportunity scoring system that helps identify web pages requiring editorial review. Using anonymized Google Search Console (GSC) and Google Analytics 4 (GA4) metrics from the FlyRank ML Internship Warehouse, the project develops a repeatable workflow for prioritizing content refresh opportunities.

A transparent rule-based baseline is first established and then compared with a Logistic Regression model trained on historical search visibility and engagement signals. The resulting system generates ranked recommendations together with interpretable reason codes, enabling editors to focus their efforts on the pages most likely to benefit from a content refresh.

The project emphasizes reproducibility, transparency, leakage prevention, and responsible use of anonymized search analytics data.

---

# Project Workflow

<p align="center">
<img src="work/figures/workflow.png" width="900">
</p>

---

# Methodology

The project follows an end-to-end applied machine learning workflow:

1. Define the editorial decision problem.
2. Collect anonymized search and engagement metrics.
3. Perform data cleaning and feature engineering.
4. Build a transparent rule-based baseline.
5. Train a Logistic Regression classifier.
6. Validate using grouped and time-aware evaluation.
7. Generate ranked editorial recommendations.
8. Export the Action Playbook.

---

# Technologies Used

- Python
- DuckDB
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Google Search Console Metrics
- Google Analytics 4 Metrics
- GitHub Pages

---

# Features Used

| Feature | Description |
|---------|-------------|
| gsc_impressions | Daily search impressions |
| gsc_clicks | Daily search clicks |
| gsc_avg_position | Average search ranking position |
| ga4_pageviews | Total page views |
| ga4_sessions | Sessions containing the page |
| ga4_users | Number of unique users |
| ga4_engaged_sessions | Engaged sessions |
| scroll_events | Scroll interaction events |

---

# Key Results

- Built a rule-based baseline for refresh opportunity scoring.
- Developed a Logistic Regression model for editorial prioritization.
- Generated ranked recommendations with interpretable reason codes.
- Performed grouped validation and leakage auditing.
- Produced an Action Playbook supporting editorial decision-making.

---

# Editorial Action Playbook

| Priority | Recommendation | Reason Code | Confidence |
|----------|---------------|-------------|------------|
| 1 | Refresh Content | STALE_CONTENT | High |
| 2 | Review Metadata | LOW_CTR | Medium |
| 3 | Monitor Performance | WATCH | Medium |
| 4 | Keep As-Is | NO_ACTION | Low |

---

# Project Structure

```
flyrank-ml-internship/
│
├── work/
│   ├── notebooks/
│   ├── figures/
│   ├── outputs/
│   ├── capstone_report.md
│   └── capstone_report.pdf
│
├── submission/
│   └── paper_url.txt
│
└── README.md
```

---

# Reproducibility

To reproduce this project:

1. Clone the repository.
2. Install dependencies from `requirements.txt`.
3. Configure the Hugging Face `HF_TOKEN`.
4. Run notebooks in order:

- w01_research_question.ipynb
- w02_ml_task_framing.ipynb
- w03_data_contract.ipynb
- w04_baseline_score.ipynb
- w05_model.ipynb
- w06_validation_audit.ipynb
- w07_action_playbook.ipynb
- capstone.ipynb

---

# Acknowledgments

This project was completed as part of the **FlyRank ML Internship**.

The work uses the anonymized **FlyRank ML Internship Warehouse** dataset for educational and research purposes. Special thanks to the FlyRank team for providing the internship structure, learning materials, and anonymized search intelligence dataset that made this capstone possible.

---

# Contact

**Vaishali Chatnalkar**

📧 schatnalkar@gmail.com

💻 GitHub: https://github.com/vaishali27-c

---

© 2026 Vaishali Chatnalkar | FlyRank ML Internship Capstone
