# paperai: AI-powered literature discovery and review engine for medical/scientific papers

[![Version](https://img.shields.io/github/release/neuml/paperai.svg?style=flat&color=success)](https://github.com/neuml/paperai/releases)
[![GitHub Release Date](https://img.shields.io/github/release-date/neuml/paperai.svg?style=flat&color=blue)](https://github.com/neuml/paperai/releases)
[![GitHub issues](https://img.shields.io/github/issues/neuml/paperai.svg?style=flat&color=success)](https://github.com/neuml/paperai/issues)
[![GitHub last commit](https://img.shields.io/github/last-commit/neuml/paperai.svg?style=flat&color=blue)](https://github.com/neuml/paperai)
[![Build Status](https://github.com/neuml/paperai/workflows/build/badge.svg)](https://github.com/neuml/paperai/actions?query=workflow%3Abuild)
[![Coverage Status](https://img.shields.io/coveralls/github/neuml/paperai)](https://coveralls.io/github/neuml/paperai?branch=master)

paperai is an AI-powered literature discovery and review engine for medical/scientific papers. paperai is used to analyze the COVID-19 Open Research Dataset (CORD-19) dataset, winning multiple awards in the CORD-19 Kaggle challenge.

paperai builds an index over medical articles to assist in analysis and data discovery. With the CORD-19 challenge, a series of COVID-19 related research topics were explored to identify relevant articles and help find answers to key scientific questions. paperai can be applied to other medical and scientific research domains.

paperai and/or NeuML has been recognized in the following articles:

- [CORD-19 Kaggle Challenge Awards](https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge/discussion/161447)
- [Machine-Learning Experts Delve Into 47,000 Papers on Coronavirus Family](https://www.wsj.com/articles/machine-learning-experts-delve-into-47-000-papers-on-coronavirus-family-11586338201)
- [Data scientists assist medical researchers in the fight against COVID-19](https://cloud.google.com/blog/products/ai-machine-learning/how-kaggle-data-scientists-help-with-coronavirus)

## Installation
The easiest way to install is via pip and PyPI

    pip install paperai

You can also install paperai directly from GitHub. Using a Python Virtual Environment is recommended.

    pip install git+https://github.com/neuml/paperai

Python 3.6+ is supported

Check out [troubleshooting link](https://github.com/neuml/txtai#troubleshooting) to help resolve environment-specific install issues.

## Building a model
paperai indexes models previously built with [paperetl](https://github.com/neuml/paperetl). paperai currently supports querying SQLite databases.

To build an index for a SQLite articles database:

    # Can optionally use pre-trained vectors
    # https://www.kaggle.com/davidmezzetti/cord19-fasttext-vectors#cord19-300d.magnitude
    # Default location: ~/.cord19/vectors/cord19-300d.magnitude
    python -m paperai.vectors

    # Build embeddings index
    python -m paperai.index

The model will be stored in ~/.cord19

See the [CORD-19 Analysis with Sentence Embeddings](https://www.kaggle.com/davidmezzetti/cord-19-analysis-with-sentence-embeddings) notebook for a comprehensive example of paperai in action.

## Building a report file
A report file is simply a markdown file created from a list of queries. An example report call:

    python -m paperai.report tasks/risk-factors.yml

Once complete a file named tasks/risk-factors.md will be created.

## Running queries
The fastest way to run queries is to start a paperai shell

    paperai

A prompt will come up. Queries can be typed directly into the console.

## Tech Overview
The tech stack is built on Python and creates a sentence embeddings index with FastText + BM25. Background on this method can be found in this [Medium article](https://towardsdatascience.com/building-a-sentence-embedding-index-with-fasttext-and-bm25-f07e7148d240) and an existing repository using this method [codequestion](https://github.com/neuml/codequestion).

The model is a combination of the sentence embeddings index and a SQLite database with the articles. Each article is parsed into sentences and stored in SQLite along with the article metadata. FastText vectors are built over the full corpus. The sentence embeddings index only uses tagged articles, which helps produce most relevant results.

Multiple entry points exist to interact with the model.

- paperai.report - Builds a markdown report for a series of queries. For each query, the best articles are shown, top matches from those articles and a highlights section which shows the most relevant sections from the embeddings search for the query.
- paperai.query - Runs a single query from the terminal
- paperai.shell - Allows running multiple queries from the terminal
