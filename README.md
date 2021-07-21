# E-Commerce Data Quality with Great Expectations

- [E-Commerce Data Quality with Great Expectations](#e-commerce-data-quality-with-great-expectations)
  - [Introduction](#introduction)
  - [Instructions for Use](#instructions-for-use)
    - [Python Environment Setup](#python-environment-setup)
    - [Initialize Great Expectations Workspace](#initialize-great-expectations-workspace)
      - [Key Definitions](#key-definitions)
      - [Steps to Deploy Great Expectations](#steps-to-deploy-great-expectations)

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

### Python Environment Setup

Clone this repo and set up a Python virtual environment, where Great Expectations
can be installed.

    git clone https://github.com/ismaildawoodjee/Great-Expectations-for-CSV
    cd Great-Expectations-for-CSV

Create a virtual environment,

    python -m venv .venv

and activate it (first line for Windows, second for Linux).

    .\.venv/Scripts/Activate.ps1

    source .venv/bin/activate

Install Great Expectations with

    pip install great_expectations

### Initialize Great Expectations Workspace

Now that the Python environment has been set up, we can initialize a new
GE (Great Expectations) workspace using the CLI commands for GE Version 3. There
are several definitions that we need to understand when navigating the GE workspace:

#### Key Definitions

- **Expectations**: are unit tests for data. They are assertions about data, and
they always start with the prefix `expect_`.

- **Data Context**: manages the deployment and configuration of your GE project.
Creating a Data Context is the first step in making use of Great Expectations.

- **Datasource**: a connection to a data directory. This can be a filesystem on
your local machine, a SQL database, or some other storage on the cloud, such as
S3, Redshift, or BigQuery.

- **Expectation Suite**: an Expectation Suite is just a collection of Expectations.

- **Profiling**: a method for GE to quickly generate Expectations based on a sample
batch of data.

- **Validating**: a method to verify the data quality of a batch of data, according
to a set of Expections. This can be done in standalone fashion, without prior profiling,
or can be done after profiling with a sample batch of data.

- **Data Doc**: the Data Doc is a static HTML page, where all the expectations
stored in the `expectations/example_expectation_suite.json` file are rendered into an easily-readable
format for assessing data quality.

#### Steps to Deploy Great Expectations

The steps to deploy GE are as follows:

1. Create a Data Context by running the CLI command:

        great_expectations --v3-api init

2. After creating a data context, we want to configure a Datasource.
Run the CLI command and follow the instructions:

        great_expectations --v3-api datasource new

3. After completing the previous step, a Jupyter Notebook will be opened in your
web browser, which will contain boilerplate code to customize and configure the
Datasource. Here, we want to **give our Datasource a name**, because there may be
many sources of data in your project, and we want to identify them easily.

    ![Give a name to your Datasource](./assets/images/datasource_configuration.png)

    Run all the cells in order before closing the Notebook.

4. Next, we need to create an Expectation Suite. This is the step where we can
specify whether we want to write expectations manually or automatically infer
them from a sample dataset. Create an Expectation Suite with the command below,
and run the Jupyter Notebook cells in order.

        great_expectations --v3-api suite new

    - Option 1: Manual creation of Expectations without a sample batch of data.
    Choose this option if you wish to write Expectations from scratch, and without
    a sample batch of data (for instance, if you have just one file that you want to validate).

    - Option 2: Manual creation of Expectations with a sample batch of data.
    Choose this option if you wish to write Expectations from scratch, but with
    a sample batch of data. The sample batch is used for profiling.

        This option is suitable if you want custom arguments to be passed when
        reading the CSV file. For instance, if I want my file to be encoded with
        "ISO-8859-1", then I can write the arguments in a JSON file and pass it as
        a command line argument, e.g.

            great_expectations --v3-api suite new --batch-request path/to/batch_request.json

        Alternatively, upon completing the set up of a new Expectation Suite, a
        Jupyter Notebook to edit it can be opened, and the `batch_request` arguments
        can be provided there.

        ![Passing in arguments for read_csv via `reader_options`](./assets/images/batch_request.png)

    - Option 3: Automatic profiling. Choose this option if you want GE to quickly
    come up with a list of Expectations based on a sample batch of data. This initial
    profiling will need to be modified according to your requirements about the data.

        ![Choose columns to create expectations](./assets/images/automatic_profiling.png)

        However, this option will not work if custom arguments are required to
        read the CSV file. For example, when encoding is not UTF-8, or when the
        delimiter is something other than comma, e.g. pipe or tab delimited data.

        After choosing the method to create an Expectation Suite, choose

            1. default_inferred_data_connector_name

    Run the all the cells in order to open up a Data Doc.

5. After running the last cell in the Jupyter Notebook produced from the previous step,
GE will open up a Data Doc in a new browser tab. The initial Expectations in this
Data Doc will not be very ideal for your requirements, so we can edit them by running

        great_expectations --v3-api suite edit ecommerce_suite --interactive

    Note: If you are using Version 2, you can run the following command instead:

        great_expectations suite edit ecommerce_suite

    Here, in the Jupyter Notebook, Expectations can be added, removed, or modified.
    First, rename the new suite of Expectations:

    ![Rename Expectation Suite name](./assets/images/rename_new_expectation_suite.png)

    Then, I removed the Expectations for max, mean and median values.

    ![Editing Expectations in Notebook](./assets/images/editing_expectations.png)

    Ensure that datetime is in a specific format, e.g. `%Y-%m-%d %H:%M:%S`. I added
    the `expect_column_values_to_match_strftime_format` here and ran the cell.

    ![Expect datetime to be in specific format](./assets/images/datetime_format.png)

    After running the final cell, a new Data Doc will be opened, which contains the
    rendered Expectations from the new Expectation Suite.

    For more info, refer to the [official documentation](https://docs.greatexpectations.io/en/latest/guides/how_to_guides/creating_and_editing_expectations/how_to_edit_an_expectation_suite_using_a_disposable_notebook.html)

6. Validating
