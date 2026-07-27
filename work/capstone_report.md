## Refresh / Content Opportunity Scoring Using FlyRank Search Intelligence

## Vaishali Chatnalkar

FlyRank ML Internship — Refresh / Content Opportunity Scoring Lane

schatnalkar@gmail.com

Repository :https://github.com/vaishali27-c/flyrank-ml-internship

Abstract— Content teams managing large editorial catalogs face a persistent operational challenge: identifying which of thousands of published pages should be refreshed before search visibility declines. This project presents a repeatable and auditable scoring framework that ranks pages based on their refresh opportunity using historical search and engagement signals from the FlyRank ML Internship Warehouse. The proposed workflow follows an end-to-end applied machine learning pipeline, including problem framing, a data-safety review excluding client-identifying fields, a transparent rule-based baseline with interpretable reason codes, a Logistic Regression model trained on eight search and engagement features, and a validation protocol designed to detect potential data leakage and reduce the influence of random or seasonal variation. The target is explicitly framed as a decision-support proxy rather than a verified business outcome, and no causal claims are made regarding search engine ranking behavior. Model performance is evaluated against the rule-based baseline using the same validation split and evaluation metrics, enabling a fair comparison. Finally, the study presents an editorial action playbook that maps model outputs to four ranked content-refresh recommendations, each accompanied by a reason code and confidence level to support practical decision making.

Index Terms— Content Refresh Scoring, SEO Analytics, Logistic Regression, Decision Support, Google Search Console, Google Analytics 4, Leakage Audit, Editorial Prioritization, Applied Machine Learning.

## I. INTRODUCTION

Search-driven content portfolios decline unevenly over time. Some pages lose visibility because competitors publish more current or comprehensive resources, while others experience declines for reasons that are not directly observable in search or engagement analytics. As content portfolios grow to thousands of pages, manually reviewing every page on a regular basis becomes impractical. The key editorial question therefore shifts from "Is this page declining?" to "Given a limited refresh budget, which pages should be prioritized for review, and why?"

This project addresses that challenge as a ranking and decision-support problem rather than a strict prediction task. The primary deliverable is a prioritized list of content pages,

where each recommendation is accompanied by an interpretable reason code and an editorial action that can be

quickly reviewed by content teams. A transparent rule-based baseline is developed first, serving both as a sanity check and as an interpretable benchmark. The Logistic Regression model is then evaluated against this baseline using the same dataset, feature set, validation strategy, and evaluation metrics to ensure a fair comparison.

Two constraints influenced every stage of the methodology. First, the dataset is fully anonymized at the client level; therefore, no component of the pipeline attempts to reconstruct or infer client identity. Pseudonymous identifiers are used only for grouping observations during validation and are never included as predictive features. Second, search and engagement metrics are inherently noisy at the page level. Consequently, the feature engineering process emphasizes aggregated historical signals and stable behavioral metrics rather than short-term fluctuations, reducing the likelihood that temporary variation is interpreted as meaningful long- term content decline.

Fig. 1 summarizes the end-to-end workflow described in the remainder of this paper.

## II. METHODOLOGY

The unit of analysis in this project is an individual content page, using historical search and engagement data collected over a specific time period. The goal is to assign each page a refresh opportunity score along with a reason code that helps editors understand why a page has been recommended. This allows content teams to focus their limited time and resources on the pages that are most likely to benefit from an update.


The cost of making an incorrect recommendation is not the same in every case. If a page that genuinely needs a refresh is missed, it may continue to lose search visibility and organic traffic. On the other hand, recommending a healthy page for review usually results only in some extra editorial effort. For this reason, the project is designed to identify as many genuinely declining pages as possible, even if it occasionally recommends pages that do not require immediate action. This approach better reflects how content teams prioritize their work in real-world editorial workflows.

## A. Data Contract and Safety

The analysis in this project uses data exclusively from the fact_daily_sample table in the FlyRank ML Internship Warehouse. The dataset combines search performance metrics from Google Search Console (GSC) with user engagement metrics from Google Analytics 4 (GA4). These features were selected because they provide a balanced view of both search visibility and user behavior. Table I summarizes the eight features used in this study.

| Feature | Description |
|:--------|:------------|
| `gsc_impressions` | Daily search impressions |
| `gsc_clicks` | Daily search clicks |
| `gsc_avg_position` | Average search ranking position |
| `ga4_pageviews` | Total page views |
| `ga4_sessions` | Number of sessions containing the page |
| `ga4_users` | Number of unique users |
| `ga4_engaged_sessions` | Sessions meeting the GA4 engagement criteria |
| `scroll_events` | User scroll interaction events |

**Table I. Feature Set and Sources**

To protect privacy, client names, page URLs, credentials, and any other client-identifying information were excluded from both the analysis and all project artifacts. Pseudonymous client identifiers were used only for grouping records during data splitting and were never included as model features.

Special attention was given to preventing data leakage. Features that directly or indirectly represented the target outcome, such as precomputed trend labels or percentage changes, were removed before model training. A final review of the dataset and project outputs confirmed that no client- identifying information or leakage-prone features were included, ensuring that the analysis remained both privacy- preserving and methodologically sound.

## B. Baseline

Before training the machine learning model, a simple rule- based baseline was developed using the same eight features

selected for this study. The baseline assigns each page to one of four reason codes: STALE_CONTENT (declining impressions and clicks without a corresponding increase in engagement), LOW_CTR (stable or increasing impressions but a low click-through rate), WATCH (minor changes that do not yet require immediate action), and NO_ACTION (no significant change in performance). These rules provide a clear and interpretable way to prioritize pages for review.

The rule-based baseline also served as the primary benchmark for evaluating the Logistic Regression model. Both approaches were developed using the same feature set, observation window, and validation strategy to ensure a fair comparison. Rather than focusing only on overall accuracy, the baseline provided an interpretable reference for understanding how manually defined rules compare with a machine learning approach. Since the primary objective of this project was to develop a practical decision-support framework, the comparison emphasizes recommendation quality and transparency rather than benchmark optimization.

## C. Model / Analysis

Logistic Regression was selected as the primary machine learning model because it provides a good balance between performance and interpretability. The dataset contains a relatively small set of numerical features, making Logistic Regression an appropriate choice for learning relationships between search and engagement metrics without introducing unnecessary model complexity. Another advantage is that the model coefficients can be interpreted easily, helping editors understand which factors contribute most to a refresh recommendation. Compared with more complex models, Logistic Regression also reduces the risk of overfitting, especially when working with a limited number of features and an anonymized dataset.

The prediction target in this study is a decision-support proxy rather than a confirmed business outcome. A page is assigned a positive label if it matches the STALE_CONTENT or LOW_CTR patterns defined by the baseline rules. This approach allows the model to learn meaningful refresh opportunities while avoiding unsupported claims that the recommendations represent verified editorial decisions.

Feature engineering was intentionally kept simple to maintain transparency. Two ratio-based features were created: Click-Through Rate (CTR), calculated as gsc_clicks ÷ gsc_impressions, and Engagement Rate, calculated as ga4_engaged_sessions ÷ ga4_sessions. In addition, time- based changes in search impressions and average position were included to capture the direction of page performance over time. No page content, metadata, URLs, or client- identifying information were used during model training.

For a feature vector x, the Logistic Regression model estimates the probability that a page belongs to the positive class using the sigmoid function:


where β₀ represents the intercept term and β represents the vector of learned coefficients. The model was trained using L2 regularization, which helps prevent overfitting by limiting excessively large coefficient values and improving the model's ability to generalize to unseen data.

## D. Evaluation

The proposed model was evaluated using both a standard train-test split and a time-aware validation strategy. Since the available sample contained only a single reporting date, a fully independent temporal validation was not possible, and the evaluation reverted to the standard split. The same feature set, observation window, and validation procedure were used for both the rule-based baseline and the Logistic Regression model to ensure a fair comparison. A leakage audit was also performed to verify that no feature was derived from the target label or future information. The evaluation showed that the proposed framework provides a reliable and transparent approach for prioritizing content refresh opportunities while maintaining consistency with the baseline methodology.

## E. Interpretation

The analysis showed that content refresh opportunities are best identified by considering multiple search visibility and user engagement signals together rather than relying on a single metric. Pages with declining impressions, lower click- through rates, and reduced engagement were generally assigned higher refresh priority. The Logistic Regression model captured these combined patterns more effectively than the rule-based baseline while remaining easy to interpret. Some recommendations for pages with limited historical data were less reliable, highlighting the importance of human review before making editorial decisions. Overall, the results demonstrate that the proposed approach can support content teams by providing practical, transparent, and evidence-based refresh recommendations.

## III. RECOMMENDATIONS: ACTION PLAYBOOK

Table II maps model output to a fixed action vocabulary so acting on the top of the ranked list requires no further interpretation.

| Priority | Action | Reason Code | Confidence |
|:--------:|:-------|:------------|:----------:|
| 1 | Refresh Content | `STALE_CONTENT` | High |
| 2 | Review Metadata | `LOW_CTR` | Medium |
| 3 | Monitor | `WATCH` | Medium |
| 4 | Keep As-Is | `NO_ACTION` | Low |

**Table II. Editorial Action Playbook**

In practice, an editor would pull the top N pages by score each review cycle, read the reason code as a starting hypothesis rather than a verdict, and use the confidence qualifier to

decide how much independent verification a recommendation needs before acting.

## IV. REPRODUCIBILITY

The project can be reproduced using the notebooks available in the GitHub repository. Follow the steps below:

1. Clone the project repository.

2. Install the required Python packages:

   ```bash
   pip install -r requirements.txt
   ```

3. Configure your Hugging Face Read Token as the `HF_TOKEN` secret (required for accessing the FlyRank dataset).

4. Execute the notebooks in the following order:

   1. `w01_research_question.ipynb`
   2. `w02_ml_task_framing.ipynb`
   3. `w03_data_contract.ipynb`
   4. `w04_baseline_score.ipynb`
   5. `w05_model.ipynb`
   6. `w06_validation_audit.ipynb`
   7. `w07_action_playbook.ipynb`
   8. `capstone.ipynb`

The implementation uses Python together with **Pandas, NumPy, DuckDB, Scikit-learn,** and **Matplotlib**. A random seed of **42** was used wherever applicable to improve reproducibility. All outputs and figures can be regenerated by rerunning the notebooks in the sequence above.

## ACKNOWLEDGMENT

The author sincerely thanks FlyRank for providing the anonymized FlyRank ML Internship Warehouse dataset and the structured internship program that made this project possible. The guidance provided through the weekly assignments, notebooks, and research-oriented workflow helped shape the development of this capstone. Appreciation is also extended to the open-source communities behind Python, DuckDB, Pandas, NumPy, Scikit-learn, and Matplotlib, whose tools supported the implementation and evaluation of this work.
