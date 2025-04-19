
# WordCount-Using-MapReduce-Hadoop

This repository is designed to test MapReduce jobs using a simple word count dataset. In this project we provide a input file and then we create a maaper and reducer logic to count the occurence orf each word in the given input. There are sample input and Expected output for the sample input.

## Approach and implementation
1. Mapper Logic: We use StringTokenizer to create tokens from the input file and loop it using while loop to map all the words in the input file with key value pairs.

2. Reducer Logic: Using the output of Mapper logic we increase a variable sum value as we encounter same words and retun them. this way we will get a list of words and the number of times it occured in the input file as output.


## Setup and Execution

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn clean package
```

### 4. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/code/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Move Dataset to Docker Container**

Copy the dataset to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Connect to Docker Container**

Access the Hadoop ResourceManager container:

```bash
docker exec -it resourcemanager /bin/bash
```

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/dataset
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/dataset
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command: Here I got an error saying output already exists so I changed it to output1 instead as destination folder

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/dataset/input.txt /output1
```

### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output1/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output1/ shared-folder/output/
    ```
3. Commit and push to your repo so that we can able to see your output

## Challenges faced: 
 1. faced difficulty in coming up with the correct syntax for the controller, mapper and reducer classes even when had a clear understaing of what should be done in each classes 
 Solution: Found the syntaxes in websites like Geek for Geeks
 2. understanding the exception message and solving them.
 Solution: Discussed with friends and reached to conclusion as to what part was throwing the exceptions and fixed it.

## Sample Input: 
 ```bash
   Hello world
   Hello Hadoop
   Hadoop is powerful
   Hadoop is used for big data
   ```

## Expected output: 
 ```bash
Hadoop 3
Hello 2
is 2
used 1
for 1
big 1
data 1
powerful 1
world 1
   ```