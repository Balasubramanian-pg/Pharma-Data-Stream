# Pharma-Data-Stream

# Pharma Data Stream Repository Documentation

## 1. Overview

This repository simulates a **continuous pharmaceutical prescription data stream** similar to commercial prescription data platforms. The repository automatically generates synthetic prescription records at regular intervals using a Python generator script and GitHub Actions automation.

The system continuously appends newly generated prescription records to a dataset stored within the repository. This enables teams to simulate a **live data ingestion pipeline** that can be used for:

* data engineering pipeline testing
* analytics development
* machine learning experimentation
* dashboard prototyping
* ETL pipeline demonstrations

The data produced is synthetic and randomly generated. It does not represent real patients, physicians, pharmacies, or prescription activity.

---

# 2. System Architecture

The repository consists of three main components.

## Data Generator

The Python script:

```
generate_data.py
```

creates synthetic prescription records.

Each execution produces a random number of rows and appends them to the main dataset file.

## Data Storage

Generated records are stored in:

```
prescriptions_stream.csv
```

Each execution adds new rows to this dataset rather than overwriting previous data.

## Automation Layer

GitHub Actions executes the generator automatically.

Workflow file:

```
.github/workflows/data-generator.yml
```

The workflow schedules the generator to run repeatedly.

---

# 3. Repository Structure

The repository is organized as follows.

```
pharma-data-stream
│
├── generate_data.py
├── prescriptions_stream.csv
├── requirements.txt
│
└── .github
    └── workflows
        └── data-generator.yml
```

### File Descriptions

**generate_data.py**

Python script that generates synthetic prescription data.

**prescriptions_stream.csv**

Accumulated dataset containing all generated records.

**requirements.txt**

Defines Python dependencies required by the generator.

**data-generator.yml**

GitHub Actions workflow responsible for running the generator automatically.

---

# 4. Data Generation Logic

The generator script creates synthetic prescription transactions that resemble typical pharmaceutical prescription datasets.

Each generated record contains fields describing:

* prescription date
* prescriber identifier
* drug identifier
* pharmacy identifier
* prescription counts
* payer type
* physician specialty
* territory assignment

The script generates randomized values using the Faker library and NumPy distributions.

---

# 5. Number of Rows Generated Per Execution

Each run produces a random number of rows.

Minimum rows:

```
29
```

Maximum rows:

```
1700
```

The number is selected using a random integer generator inside the script.

---

# 6. Execution Frequency

The workflow runs automatically every **35 minutes**.

Cron schedule:

```
*/35 * * * *
```

---

# 7. Estimated Data Growth

The number of workflow executions per day is approximately calculated as follows.

Total minutes per day:

```
24 × 60
```

Digit-by-digit calculation:

```
24 × 60 = 1440
```

Minutes per workflow execution:

```
35
```

Division:

```
1440 ÷ 35
```

Long division:

```
35 × 41 = 1435
```

Approximate executions per day:

```
41
```

---

## Minimum Daily Rows

```
29 × 41
```

Digit-by-digit calculation:

```
29 × 40 = 1160
29 × 1 = 29
```

Total:

```
1189 rows/day
```

---

## Maximum Daily Rows

```
1700 × 41
```

Digit-by-digit calculation:

```
1700 × 40 = 68000
1700 × 1 = 1700
```

Total:

```
69700 rows/day
```

---

# 8. Data Schema

The synthetic dataset contains the following fields.

| Column            | Description                             |
| ----------------- | --------------------------------------- |
| run_timestamp     | Timestamp when the record was generated |
| rx_date           | Prescription date                       |
| npi               | Synthetic prescriber identifier         |
| ndc               | Synthetic drug identifier               |
| pharmacy_id       | Pharmacy identifier                     |
| trx               | Total prescriptions                     |
| nrx               | New prescriptions                       |
| patient_age_group | Age category                            |
| payer_type        | Insurance category                      |
| zip               | Postal code                             |
| territory_id      | Sales territory identifier              |
| specialty         | Physician specialty                     |

---

# 9. Python Dependencies

Dependencies are defined in:

```
requirements.txt
```

The generator requires:

```
pandas
numpy
faker
```

GitHub Actions installs these dependencies automatically during workflow execution.

---

# 10. GitHub Workflow Process

Each workflow execution performs the following steps.

1. Checkout repository source code.
2. Install Python dependencies.
3. Execute the generator script.
4. Append generated rows to the dataset file.
5. Commit the updated dataset back to the repository.

Workflow commit message:

```
Auto generated pharma prescription data
```

---

# 11. Running the Generator Manually

The workflow can also be triggered manually.

Steps:

1. Open the repository in GitHub.
2. Navigate to the **Actions** tab.
3. Select the workflow named **Generate Pharma Data**.
4. Click **Run workflow**.

This allows on-demand dataset generation.

---

# 12. Typical Use Cases

This repository can be used to simulate streaming pharmaceutical data for the following scenarios.

### Data Engineering

* ETL pipeline testing
* Bronze/Silver/Gold pipeline simulation
* incremental ingestion testing

### Analytics Development

* dashboard prototyping
* SQL query performance testing
* data modeling

### Machine Learning

* prescription demand forecasting
* prescriber behavior models
* recommendation systems

---

# 13. Limitations

GitHub repositories are not designed for very large datasets.

GitHub recommends repositories remain below approximately:

```
1 GB
```

As the dataset grows over time, it may be necessary to migrate storage to:

* cloud object storage
* data lake environments
* database staging layers

---

# 14. Maintenance

If the workflow fails, check the following.

### Workflow Permissions

Repository settings must allow GitHub Actions to write to the repository.

Required configuration:

```
permissions:
  contents: write
```

### Dependency Issues

Ensure the required libraries remain available in `requirements.txt`.

---

# 15. Disclaimer

The dataset generated by this repository is entirely synthetic. All identifiers, prescription values, and entities are randomly generated and do not correspond to real individuals, physicians, pharmacies, or prescription activity.

---

# 16. Future Improvements

Potential enhancements include:

* simulating drug launches
* modeling physician adoption curves
* introducing prescription seasonality
* generating competitor market share dynamics
* exporting data directly into data lake storage systems

---

# 17. Summary

This repository provides a continuously growing synthetic pharmaceutical dataset generated automatically through GitHub Actions. The system demonstrates how automated workflows can simulate streaming data sources for analytics, engineering, and machine learning experimentation.
