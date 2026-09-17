# Stock Market Analysis — Hadoop MapReduce

This repository contains my implementation of the **Stock Market Analysis Hadoop MapReduce assignment** using Java and Hadoop MapReduce.

The project focuses on analyzing historical stock-price data using distributed MapReduce processing on Hadoop and Amazon EMR.

---

## Repository Contents

```text
Stock-market-analysis-Hadoop/
│
├── AnalyzeStock.java
├── AnalyzeStockAdvanced.java
└── README.md
```

---

# Part A — Stock Price Analysis

## AnalyzeStock

### Objective

The `AnalyzeStock` MapReduce program analyzes historical stock-price data and calculates an aggregation for every stock ticker over a user-specified date range.

The supported aggregation operations are:

```text
avg
min
max
```

The supported stock-price fields are:

```text
close
low
high
```

### Input Format

The input CSV contains the following fields:

```text
ticker,date,open,high,low,close,volume,ex-dividend,split_ratio,adj_open,adj_high,adj_low,adj_close,adj_volume
```

Example:

```text
AAPL,1999-11-18,42.5,43.0,41.5,42.0,...
```

### Command-Line Arguments

The program accepts six positional arguments:

```text
1. Start date
2. End date
3. Aggregation operation
4. Price field
5. HDFS input path
6. HDFS output path
```

Dates are specified in:

```text
MM/DD/YYYY
```

### Example — Average Low Price

```bash
hadoop jar AnalyzeStock.jar AnalyzeStock \
10/11/1990 \
12/09/2015 \
avg \
low \
/user/cloudera/samplestockdata.csv \
/user/cloudera/stockoutput
```

### Example — Minimum Closing Price

```bash
hadoop jar AnalyzeStock.jar AnalyzeStock \
02/20/2006 \
10/09/2015 \
min \
close \
/user/cloudera/samplestockdata.csv \
/user/cloudera/stockoutput
```

### Example — Maximum Closing Price

```bash
hadoop jar AnalyzeStock.jar AnalyzeStock \
01/01/1995 \
01/01/2000 \
max \
close \
/user/cloudera/samplestockdata.csv \
/user/cloudera/stockoutput
```

### Output

The output contains the aggregated value for each stock ticker:

```text
ticker    aggregated_value
```

Example:

```text
A       46.5
AA      64.1
AAN     16
AAON    8.3
AAPL    33.6
```

### View Output

```bash
hdfs dfs -cat /user/cloudera/stockoutput/*
```

### Remove Output Before Re-running

Hadoop requires the output directory to not already exist.

```bash
hdfs dfs -rm -r /user/cloudera/stockoutput
```

---

# Part B — Amazon EMR

## AnalyzeStockAdvanced

`AnalyzeStockAdvanced` is the advanced MapReduce implementation designed to run on Hadoop/Amazon EMR.

### Objective

For every year in the stock-price dataset, the program finds the stock ticker with the **largest yearly price fluctuation**.

The fluctuation is calculated as:

```text
Difference = High Price - Low Price
```

### Program

```text
AnalyzeStockAdvanced.java
```

### Command-Line Arguments

The program accepts two positional arguments:

```text
1. Input path
2. Output path
```

The paths can be HDFS or Amazon S3 locations.

### HDFS Example

```bash
hadoop jar AnalyzeStockAdvanced.jar AnalyzeStockAdvanced \
/user/cloudera/stockdata.csv \
/user/cloudera/advancedOutput
```

### Amazon S3 Example

```text
s3://<bucket>/input/stockdata.csv
```

Output:

```text
s3://<bucket>/output/
```

### Output Format

```text
Year    Ticker    Low    High    Difference
```

Example:

```text
1995    AAPL    12.8    79.2    66.4
1996    ABC     15.1    92.4    77.3
```

The output identifies the ticker with the maximum yearly difference between its high and low prices.

---

# Dataset

The assignment uses historical stock-price data obtained from Quandl.

The input dataset contains the following fields:

```text
ticker
date
open
high
low
close
volume
ex-dividend
split_ratio
adj_open
adj_high
adj_low
adj_close
adj_volume
```

The date format in the original dataset is:

```text
yyyy-mm-dd
```

A smaller sample dataset can be used for local development and testing before processing the full dataset.

---

# Technologies Used

* Java
* Apache Hadoop
* Hadoop MapReduce
* HDFS
* Amazon EMR
* Amazon S3
* Cloudera VM / Pseudo-distributed Hadoop

---

# MapReduce Concepts Demonstrated

This project demonstrates:

* Mapper and Reducer implementation
* Key-value based MapReduce processing
* Multiple reducers
* HDFS input/output
* Command-line argument handling
* CSV parsing
* Date-range filtering
* `avg`, `min`, and `max` aggregation
* Intermediate key-value design
* Sorting and grouping by keys
* Multi-stage MapReduce processing
* Hadoop execution on Amazon EMR
* HDFS and S3 input/output

---

# Compilation

The Java programs can be compiled using the Hadoop libraries available in the Hadoop environment.

### AnalyzeStock

```bash
javac -classpath "$(hadoop classpath)" -d . AnalyzeStock.java
```

Create the JAR:

```bash
jar cf AnalyzeStock.jar *.class
```

### AnalyzeStockAdvanced

```bash
javac -classpath "$(hadoop classpath)" -d . AnalyzeStockAdvanced.java
```

Create the JAR:

```bash
jar cf AnalyzeStockAdvanced.jar *.class
```

The exact compilation command may vary depending on the Hadoop/Cloudera installation.

---

# Useful HDFS Commands

List files:

```bash
hdfs dfs -ls <path>
```

View output:

```bash
hdfs dfs -cat <path>/*
```

Remove an output directory:

```bash
hdfs dfs -rm -r <output-path>
```

---

# Project Status

| Component            | Status    |
| -------------------- | --------- |
| AnalyzeStock         | Completed |
| AnalyzeStockAdvanced | Completed |
| HDFS Testing         | Completed |
| MapReduce Testing    | Completed |
| EMR Exercise         | Completed |

---

## Author

**Kavya Nair Puthiyedath**

B.Tech Computer Science & Engineering
IIIT Kottayam
