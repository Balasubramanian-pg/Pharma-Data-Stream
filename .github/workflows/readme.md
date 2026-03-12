# GitHub Actions Workflow Documentation

## `data-generator.yml`

## 1. Purpose

The workflow file `data-generator.yml` automates the generation of synthetic pharmaceutical prescription data. It executes a Python script that generates new prescription records and appends them to the repository dataset.

The workflow runs on a schedule and can also be triggered manually. After the data is generated, the workflow commits the updated dataset back to the repository.

This automation simulates a continuously updating pharmaceutical data stream.

---

# 2. Workflow Location

GitHub only executes workflows stored inside the following directory:

```
.github/workflows/
```

The workflow file path in this repository is:

```
.github/workflows/data-generator.yml
```

---

# 3. Workflow File Structure

The workflow is composed of several sections.

| Section     | Purpose                                           |
| ----------- | ------------------------------------------------- |
| name        | Defines the workflow name shown in GitHub Actions |
| on          | Specifies when the workflow runs                  |
| permissions | Defines access permissions for the workflow       |
| jobs        | Defines tasks executed by the workflow            |
| steps       | Individual commands executed within the job       |

---

# 4. Workflow File Contents

The workflow file used in this repository is shown below.

```yaml
name: Generate Pharma Data

on:
  schedule:
    - cron: "*/35 * * * *"
  workflow_dispatch:

permissions:
  contents: write

jobs:
  generate-data:
    runs-on: ubuntu-latest

    steps:

    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Setup Python
      uses: actions/setup-python@v5
      with:
        python-version: "3.11"

    - name: Install dependencies
      run: |
        pip install -r requirements.txt

    - name: Run data generator
      run: |
        python generate_data.py

    - name: Commit and push generated data
      run: |
        git config --global user.name "github-actions"
        git config --global user.email "actions@github.com"
        git add prescriptions_stream.csv
        git commit -m "Auto generated pharma prescription data" || echo "No changes"
        git push
```

---

# 5. Workflow Trigger Events

The workflow is triggered by two events.

## Scheduled Execution

The workflow runs automatically based on a cron schedule.

```
cron: "*/35 * * * *"
```

This expression instructs GitHub Actions to execute the workflow every **35 minutes**.

---

## Manual Execution

The workflow can also be run manually.

```
workflow_dispatch
```

This enables a **Run workflow** button in the GitHub Actions interface.

Manual execution allows developers to trigger the data generator at any time.

---

# 6. Workflow Permissions

GitHub workflows use a temporary authentication token called `GITHUB_TOKEN`.

By default, this token has limited permissions.

This workflow requires write access to commit new data.

The following permission enables repository write access:

```
permissions:
  contents: write
```

Without this configuration, the workflow cannot push changes back to the repository.

---

# 7. Job Definition

Jobs define groups of steps executed by GitHub Actions.

```
jobs:
  generate-data:
```

This repository defines one job called `generate-data`.

---

# 8. Execution Environment

The job runs inside a GitHub-hosted Linux environment.

```
runs-on: ubuntu-latest
```

GitHub automatically provisions a virtual machine with the latest Ubuntu environment.

---

# 9. Workflow Steps

Each job consists of sequential steps.

## Step 1 — Checkout Repository

```
uses: actions/checkout@v4
```

This step downloads the repository source code into the runner environment.

The workflow requires the repository files in order to execute the generator script.

---

## Step 2 — Setup Python

```
uses: actions/setup-python@v5
```

This step installs Python inside the runner environment.

The workflow specifies the Python version:

```
python-version: "3.11"
```

---

## Step 3 — Install Dependencies

```
pip install -r requirements.txt
```

This step installs Python libraries required by the generator script.

Dependencies include:

* pandas
* numpy
* faker

These libraries enable synthetic data generation.

---

## Step 4 — Run Data Generator

```
python generate_data.py
```

This step executes the generator script.

The script produces a random number of prescription records between:

Minimum rows:

```
29
```

Maximum rows:

```
1700
```

The generated rows are appended to the dataset file.

---

## Step 5 — Commit Generated Data

The workflow commits the updated dataset back to the repository.

Commands executed:

```
git config --global user.name "github-actions"
git config --global user.email "actions@github.com"
git add prescriptions_stream.csv
git commit -m "Auto generated pharma prescription data"
git push
```

These commands configure Git identity and push the updated dataset.

---

# 10. Error Handling

The workflow uses the following safeguard.

```
git commit -m "Auto generated pharma prescription data" || echo "No changes"
```

This prevents the workflow from failing when no changes are detected.

If no rows are generated or the file remains unchanged, the workflow continues successfully.

---

# 11. Workflow Runtime Behavior

Each execution performs the following sequence:

1. GitHub provisions a runner environment.
2. Repository source code is downloaded.
3. Python is installed.
4. Python dependencies are installed.
5. The generator script executes.
6. Synthetic prescription rows are appended to the dataset.
7. The updated dataset is committed back to the repository.

---

# 12. Expected Workflow Frequency

The scheduled interval is **35 minutes**.

Total minutes in one day:

```
24 × 60
```

Digit-by-digit calculation:

```
24 × 60 = 1440 minutes
```

Minutes between executions:

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

# 13. Data Growth Per Day

Minimum rows generated per day:

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

Maximum rows generated per day:

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

# 14. Monitoring Workflow Runs

Workflow execution history can be viewed in the repository.

Steps:

1. Open the repository.
2. Navigate to the **Actions** tab.
3. Select **Generate Pharma Data**.

Each run displays:

* execution time
* logs
* status (success or failure)

---

# 15. Known Limitations

GitHub scheduled workflows may not execute exactly at the scheduled minute. Execution may be delayed by several minutes depending on runner availability.

Repository size growth must also be monitored, as GitHub repositories are not intended for very large datasets.

---

# 16. Summary

The `data-generator.yml` workflow automates the generation of synthetic pharmaceutical prescription data by scheduling periodic execution of a Python generator script. Each workflow run produces new prescription records and commits them back to the repository, enabling the repository to function as a continuously growing synthetic data stream.
