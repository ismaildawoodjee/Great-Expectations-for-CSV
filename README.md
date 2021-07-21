# Ensuring E-Commerce Data Quality with Great Expectations

## Introduction

Ensuring data quality is a crucial step in any data engineering project. Ultimately, the end
goal of data is for stakeholders to make important business decisions. If
the quality of data is not up to scratch, then inaccurate values can impact
the data pipeline/engineering process, and also lead to wrong decisions being
made due to incorrect and inaccurate data.

Here, I will be using an [e-commerce dataset](https://www.kaggle.com/carrie1/ecommerce-data)
to illustrate how [Great Expectations](https://greatexpectations.io/) can be used to ensure
and monitor data quality, for instance, by validating that the latest batch of data
has the expected values or falls within the expected range of numbers. Great Expectations
is a way of providing automated testing, for the purpose of validating, profiling and
documenting your data.

## Instructions for Use

### Environment Setup

Clone this repo and set up a Python virtual environment, where Great Expectations
can be installed.

    git clone https://github.com/ismaildawoodjee/Great-Expectations-for-CSV
    cd Great-Expectations-for-CSV

Create a virtual environment,

    python -m venv .venv

and activate it (first line for Windows, second for Linux).

    .venv/Scripts/Activate.ps1

    source .venv/bin/activate

Install Great Expectations with

    pip install great_expectations

### Initialize Great Expectations Workspace

Now that the Python environment has been set up, we can initialize a new
GE (Great Expectations) workspace using the CLI commands for GE Version 3. There
are several terms that we need to understand when navigating the GE Workspace:

- **Expectations**: are unit tests for data. They are assertions about data, and
they always start with the prefix `expect_`.

- **Data Context**: manages the deployment and configuration of your GE project.
Creating a Data Context is the first step in making use of Great Expectations.

- **Datasource**:
