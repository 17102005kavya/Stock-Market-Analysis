# Stock Market Analysis — Hadoop MapReduce

This repository contains my implementation of the **Stock Market Analysis Hadoop MapReduce Homework**. The assignment demonstrates MapReduce programming using Java on a pseudo-distributed Hadoop installation / Cloudera VM.

The homework consists of two mandatory MapReduce problems and one optional EMR exercise.

---

## Repository Contents

```text
Stock-market-analysis-Hadoop/
│
├── MyWordCount.java
├── AnalyzeStock.java
├── AnalyzeStockAdvanced.java
└── README.md
```

---

# Part A — MapReduce on Hadoop

## 1. MyWordCount — WordCount 2.0

### Objective

The standard Hadoop WordCount program was modified to:

* Count word frequencies.
* Remove punctuation.
* Remove stop words.
* Support case-sensitive and case-insensitive processing.
* Support multiple HDFS input paths.
* Allow the user to specify the number of reducers.
* Validate command-line arguments.
* Write the final word-frequency output to HDFS.

### Program

```text
MyWordCount.java
```

### Command-Line Arguments

The program accepts four positional arguments:

```text
1. Number of reducers
2. Case sensitivity: true | false
3. Comma-separated HDFS input paths
4. HDFS output path
```

### Example

```bash
hadoop jar MyWordCount.jar MyWordCount \
4 \
true \
/user/cloudera/input/big.txt,/user/cloudera/input/writprog.pro \
/user/cloudera/outputWordCount
```

Case-insensitive execution:

```bash
hadoop jar MyWordCount.jar MyWordCount \
2 \
false \
/user/cloudera/input/big.txt,/user/cloudera/input/writprog.pro \
/user/cloudera/outputWordCount
```

### View Output

```bash
hdfs dfs -ls /user/cloudera/outputWordCount
```

```bash
hdfs dfs -cat /user/cloudera/outputWordCount/part-*
```

### Remove Output Before Re-running

Hadoop MapReduce requires the output directory to not already exist.

```bash
hdfs dfs -rm -r /user/cloudera/outputWordCount
```

---

# 2. AnalyzeStock — Stock Price Analysis

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

### Program

```text
AnalyzeStock.java
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

Date arguments use:

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

### View Output

```bash
hdfs dfs -cat /user/cloudera/stockoutput/*
```

The output has the following format:

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

### Remove Output Before Re-running

```bash
hdfs dfs -rm -r /user/cloudera/stockoutput
```

---

# Part B — Amazon EMR (Extra Credit)

## AnalyzeStockAdvanced

This is the optional EMR/advanced MapReduce exercise.

### Objective

For every year in the stock-price dataset, find the stock ticker having the **largest yearly fluctuation**.

The fluctuation is defined as:

```text
Difference = High Price - Low Price
```

The program produces:

```text
Year    Ticker    Low    High    Difference
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

The paths can point to HDFS or Amazon S3.

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
...
```

The program uses MapReduce processing to determine the maximum yearly fluctuation.

---

# Dataset

The assignment uses historical stock-price data obtained from Quandl.

Two datasets were provided:

### Full Dataset

Approximately:

```text
Compressed: ~400 MB
Uncompressed: ~1.6 GB
```

### Sample Dataset

The smaller sample dataset can be used during development and testing.

Expected input format:

```text
ticker,date,open,high,low,close,volume,ex-dividend,split_ratio,adj_open,adj_high,adj_low,adj_close,adj_volume
```

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

This homework demonstrates the following Hadoop concepts:

* Mapper and Reducer implementation
* Key-value based MapReduce processing
* Multiple reducers
* HDFS input/output
* Command-line argument handling
* Passing configuration values to MapReduce tasks
* CSV parsing
* Date-range filtering
* Aggregation using `avg`, `min`, and `max`
* Intermediate key-value design
* Sorting and grouping by keys
* Multi-stage MapReduce processing
* Hadoop execution on Amazon EMR
* HDFS and S3 input/output

---

# Compilation

The Java programs can be compiled using the Hadoop libraries available in the Hadoop environment.

Example:

```bash
javac -classpath "$(hadoop classpath)" -d . MyWordCount.java
```

```bash
javac -classpath "$(hadoop classpath)" -d . AnalyzeStock.java
```

```bash
javac -classpath "$(hadoop classpath)" -d . AnalyzeStockAdvanced.java
```

Create JAR files:

```bash
jar cf MyWordCount.jar *.class
```

```bash
jar cf AnalyzeStock.jar *.class
```

```bash
jar cf AnalyzeStockAdvanced.jar *.class
```

The exact compilation command may vary depending on the Hadoop/Cloudera installation.

---

# Important Hadoop Notes

### Output Directory

Hadoop will fail if the specified output directory already exists.

Remove it before re-running:

```bash
hdfs dfs -rm -r <output-path>
```

### Viewing HDFS Files

List files:

```bash
hdfs dfs -ls <path>
```

Read output:

```bash
hdfs dfs -cat <path>/*
```

### Multiple Input Files

`MyWordCount` supports multiple HDFS input paths by providing them as a comma-separated argum
