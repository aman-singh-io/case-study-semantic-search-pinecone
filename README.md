# Semantic Search Engine: Granular Text Indexing with Pinecone & SentenceTransformers

> Built by **Amandeep SINGH** ([@aman-singh-io](https://github.com/aman-singh-io))

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Sentence Transformers](https://img.shields.io/badge/Sentence%20Transformers-Hugging%20Face-yellow?style=for-the-badge&logo=huggingface&logoColor=white)](https://www.sbert.net/)
[![Pinecone](https://img.shields.io/badge/Pinecone-VectorDB-black?style=for-the-badge&logo=pinecone)](https://www.pinecone.io/)
[![Pandas](https://img.shields.io/badge/Pandas-Data-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)

---

## 📌 Overview

This repository showcases a practical implementation of a high-performance **Semantic Search Engine** designed to retrieve educational content with high contextual accuracy. The project demonstrates the massive performance gains achieved by shifting from macro-level data indexing to granular, chunked data structures.

Using a custom dataset of online courses, the engine leverages dense vector embeddings and a serverless vector database to match user search intent rather than just matching raw keywords.

---

## 🚀 Key Features

- **Semantic Retrieval:** Uses deep learning to understand the contextual meaning behind user queries (e.g., searching for "regression in Python" returns specific statistical modules, not just generic Python intros).
- **Vector Embeddings:** Transforms raw text data into high-dimensional vector spaces using a pre-trained transformer model.
- **Cloud Vector DB Architecture:** Implements a fast, scalable, and serverless vector indexing pipeline utilizing Pinecone.
- **Granularity Performance Comparison:** Evaluates how text chunking and granularity impact cosine similarity scores and user search experience.

---

## 🛠️ Tech Stack & Frameworks

- 🐍 **Programming Language:** `Python 3`
- 🐼 **Data Manipulation:** `pandas`
- 🤖 **Machine Learning & NLP:** `sentence-transformers` (Model: `sentence-transformers/all-MiniLM-L6-v2`)
- 💾 **Vector Database:** `pinecone` (Serverless Spec on AWS `us-east-1`, 384 Dimensions, Cosine Similarity Metric)
- ⚙️ **Environment Management:** `python-dotenv`

---

## 📊 Methodology & Architectural Comparison

The core objective of this study was to compare how the scope of data preprocessing impacts search precision. We built and evaluated two distinct pipelines:

### Case 1: Course-Level Indexing (Broad)
- **Input Data:** `courses_data.csv` (Encoded in ANSI)
- **Strategy:** Text was aggregated at the macroeconomic level, combining general course titles, technologies, and broad overviews (`course_name`, `course_slug`, `course_technology`, `course_topic`, `course_description`) into a single string embedding via a custom function.
- **Limitation:** Dilutes highly specific topics within large blocks of text, making precise retrieval difficult.

### Case 2: Section-Level Indexing (Granular)
- **Input Data:** `courses_section_data.csv` (Encoded in ANSI)
- **Strategy:** Data was broken down into individual course modules. Strings were generated combining the course context *with* the exact section names and section descriptions (`course_name + course_technology + course_description + section_name + section_description`).
- **Advantage:** Preserves narrow semantic nuances, allowing the vector database to pinpoint precise topics. Unique compound IDs were engineered using `course_id` and `section_id` (e.g., `37-369`).

---

## 🎯 Results & Benchmark Analysis

To stress-test the two approaches, both systems were queried with the exact same input string: `“regression in Python”`.

### Case 1: Top Search Matches
- **Top Result:** `machine-learning-in-python` (Machine Learning in Python) — **Score: 0.50**
- **Second Result:** `machine-learning-with-ridge-and-lasso-regression` — **Score: 0.46**
- **Third Result:** `python-for-finance` — **Score: 0.45**

### Case 2: Top Search Matches (Filtered with Score Threshold >= 0.4)
- **Top Result (ID 37-369):** *Linear Regression with sklearn* within the **Machine Learning in Python** course — **Score: 0.7539**
- **Second Result (ID 36-363):** *Using Regressions for Financial Analysis* within the **Python for Finance** course — **Score: 0.6764**
- **Third Result (ID 37-368):** *Linear Regression* within the **Machine Learning in Python** course — **Score: 0.6374**


### Dynamic Comparison Table

| Metric | Case 1: Course-Level Indexing | Case 2: Section-Level Indexing |
| :--- | :--- | :--- |
| **Top Match Content** | *Machine Learning in Python* (Whole Course) | *Linear Regression with sklearn* (Specific Module) |
| **Highest Similarity Score** | **0.5000** | **0.7539** |
| **Precision Type** | Macro-relevance (General Target) | Micro-relevance (Exact Target Match) |

> 📌 **Key Takeaway:** Moving to a granular, section-level indexing architecture (Case 2) boosted our top similarity score from **0.50 to 0.7539** (a ~50% increase in retrieval confidence). This proves that for production-grade Semantic Search or RAG (Retrieval-Augmented Generation) applications, proper text chunking and metadata preservation are vital to delivering accurate results.

---
