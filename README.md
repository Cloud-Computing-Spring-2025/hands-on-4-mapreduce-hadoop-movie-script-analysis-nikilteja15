# Movie Script Analysis Using Hadoop MapReduce

## **Project Overview**
This project analyzes a dataset of movie script dialogues using **Hadoop MapReduce**. The analysis extracts insights such as:
1. **Most Frequently Spoken Words** by characters.
2. **Total Dialogue Length per Character**.
3. **Unique Words Used by Each Character**.

The project is implemented using **Java and Hadoop MapReduce**, deployed on a **Hadoop cluster using Docker**, and managed via **GitHub**.

---

## **Setup and Execution**

### **1. Fork and Open the Repository**
1. Accept the **GitHub Classroom invitation** and fork the repository.
2. Open the repository in **GitHub Codespaces**.

### **2. Start the Hadoop Cluster**
Run the following command in the terminal to start the Hadoop cluster using Docker:
```bash
docker-compose up -d
```

### **3. Build the Java Code with Maven**
```bash
mvn clean install
```
This will generate a JAR file in the `target/` directory.

### **4. Prepare Input Data Files**
Ensure that `movie_dialogues.txt` is present in the `input/` directory.

### **5. Move JAR and Input Files to Hadoop Container**
#### **5.1 Move JAR File**
```bash
docker cp target/movie-script-analysis.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```
#### **5.2 Move Input File**
```bash
docker cp input/movie_dialogues.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### **6. Connect to the Hadoop Container**
```bash
docker exec -it resourcemanager /bin/bash
```
Navigate to the Hadoop directory:
```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### **7. Setup HDFS for Input Data**
#### **7.1 Create HDFS Directory**
```bash
hadoop fs -mkdir -p /input/movie_scripts
```
#### **7.2 Upload Input File to HDFS**
```bash
hadoop fs -put movie_dialogues.txt /input/movie_scripts/
```

### **8. Run the MapReduce Job**
```bash
hadoop jar movie-script-analysis.jar com.movie.script.analysis.MovieScriptAnalysis /input/movie_scripts/movie_dialogues.txt /output
```

### **9. View Output Results**
#### **9.1 List Output Directories**
```bash
hadoop fs -ls /output
```
#### **9.2 View Output Files**
##### **Task 1: Most Frequent Words by Character**
```bash
hadoop fs -cat /output/task1/part-r-00000
```
##### **Task 2: Dialogue Length Analysis**
```bash
hadoop fs -cat /output/task2/part-r-00000
```
##### **Task 3: Unique Words by Character**
```bash
hadoop fs -cat /output/task3/part-r-00000
```

### **10. Copy Output from HDFS to Local System**
```bash
hadoop fs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```
Exit the container:
```bash
exit
```
Copy files from container to local machine:
```bash
docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ ./output/
```

### **11. Submit the Assignment**
#### **11.1 Push Code and Output to GitHub**
```bash
git add .
git commit -m "Completed Movie Script Analysis Assignment"
git push origin main
```
#### **11.2 Submit on GitHub Classroom**
Ensure that the following files are included:
- **JAR file** (`movie-script-analysis.jar`)
- **Input file** (`movie_dialogues.txt`)
- **Output files** (`/output/` directory)
- **This README file**

---

## **Sample Input and Expected Output**
### **Sample Input (movie_dialogues.txt)**
```
JACK: The ship is sinking! We have to go now.
ROSE: I won’t leave without you.
JACK: We don’t have time, Rose!
```

### **Expected Output**
#### **1. Most Frequently Spoken Words**
```
the 3
we 3
have 2
to 2
now 1
without 1
```
#### **2. Total Dialogue Length per Character**
```
JACK 54
ROSE 25
```
#### **3. Unique Words Used by Each Character**
```
JACK [the, ship, is, sinking, we, have, to, go, now, don’t, time, rose]
ROSE [i, won’t, leave, without, you]
```

---

## **Challenges Faced & Solutions**
| **Challenge** | **Solution** |
|--------------|-------------|
| Handling punctuation in words | Used regex to clean tokens |
| Tracking unique words efficiently | Used HashSet per character |
| Managing Hadoop counters | Implemented custom counters in Mapper |

---

## **Conclusion**
This project successfully implements **Hadoop MapReduce** to analyze movie script dialogues. By following the above steps, the program extracts insights such as frequently spoken words, dialogue lengths, and unique words per character. The entire pipeline is built, executed, and deployed using **Docker-based Hadoop clusters**, demonstrating skills in **Big Data processing and MapReduce programming**.

