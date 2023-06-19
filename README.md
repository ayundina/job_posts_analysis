# Most common requirements of Data Science vacancies

In this project:
1. Scrape vacancies in the Netherlands
2. Extract key phrases related to education level, experience, tools, and technologies
3. Visualize the findings

## Scraping

Google Jobs collects information from the internet about open positions.
We can scrape the job results based on request query and filter by location and language.

```python
params = {
    "query": "Data Scientist",
    "location": "Netherlands",
    "hl": "en",
    "start": 0,
    "engine": "google_jobs"
}
```
The library for scraping is [SerpApi](https://serpapi.com/)
```python
from serpapi import GoogleSearch

client = GoogleSearch(params)
response = client.get_dict()
```
Which returns:
```python
"jobs_results": [
    {
        "title": "Graduate Data Scientist",
        "company_name": "Optiver",
        "location": "Amsterdam, Netherlands",
        "description": "Can you solve this puzzle?...",
    },
    {
        "title": "Data Scientist",
        "company_name": "Adyen",
        "location": "Amsterdam, Netherlands",
        "description": "Adyen provides payments, data, and financial products..."
    }
]
```

## Extracting phrases

For the phrase extraction task, multiple plug-and-play approaches were tried and tested. For example, pre-trained models from spacy with pytextrank, transformers, and openai. Openai can be prompted to extract and group phrases from the given text:
```python
import openai

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=prompts_and_replies
)
```
Where ```prompts_and_replies``` are natural language instructions followed by the text to process. In our case, it's a job description:
```python
job_description = """
Who you are
• MSc/MA/PhD in a technical discipline related to Data Science (Data Science, Physics, Computer Science, Econometrics, Bioinformatics, etc.)
• You have 3+ years of experience as Data Scientist.
• You have experience developing, training, validating, benchmarking, and monitoring machine learning algorithms. 
• You have experience leveraging a big data framework to create the pipelines needed to feed the models with appropriate data. 
• You approach problems with a statistical mindset, keeping inference as a product of data science. Experience with statistical testing (confidence, p-value, A/B testing, Bayesian) is a plus. 
• You can communicate complex outcomes with clarity over a wide range of audiences. 
• Technologies: Extensive knowledge of data science and statistics techniques, toolsets and algorithms, such as e.g. Spark, Scikit-Learn, TensorFlow, PyTorch, XGBoost, Pandas, Airflow, SQL. 
• Mentality: An experimental mindset with a launch fast and iterate mentality. A strong statistics/mathematics background is a plus.
"""
```
Where extracted keywords will look like this:
```python
skills = {
    "EDU": {
        "ma",
        "msc",
        "phd"
    },
    "EXP": {
        "3+"
    },
    "TOOL": {
        "airflow",
        "tensorflow",
        "spark",
        "pandas",
        "scikit-learn",
        "pytorch",
        "sql",
        "xgboost"
    },
    "TECH": {
        "statistical mindset",
        "machine learning",
        "p-value",
        "computer science",
        "physics",
        "a/b testing",
        "bioinformatics",
        "statistics",
        "data science",
        "big data framework",
        "confidence",
        "econometrics",
        "mathematics",
        "bayesian"
    }
}
```

## Visualisation

Finally! Combining, counting, sorting, and visualizing the results gives us bar plots with the most required skills and experience in the Data Science industry.\
\
![data_science_requirements.png](https://github.com/ayundina/job_posts_analysis/blob/main/data_science_requirements.png)

## Polishing

As rightly noticed, the results require more cleaning and filtering... This project is an initial attempt to see what is what in the Data Science field. In the current state, it roughly outlines the most popular requirements. 