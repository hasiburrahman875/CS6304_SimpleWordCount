# SimpleWordCount

### General Information:

* Operating System:         Mac -> Windows App, Windows -> Default Remote Desktop, Ubuntu -> Remmina
* Machine:                  cs6304-<mst_username>-01.class.mst.edu
* User:                     <mst_username>
* Default Password:         <mst_password>

### Importing Project:
* Open “eclipse”, right-click on “Package Explorer” window, click import.
* Select “Git”-> “Projects from Git” and click “next”.
* Select “clone url” and click “next”.
* Paste “https://github.com/md-hasibur-rahman/CS6304_SimpleWordCount” in the “url” textbox, and click “next”. 
* Choose “Import existing project” and click “finish”.

### Referencing libraries:
* Right click on project and select “build path”-> “configure build path” ->”libraries”->”add external jars”.
* Go to "home" -> "git" -> "SimpleWordCount" -> "lib" and select all jars and click ok.

### Input file:
* Open folder "file", you will see the input file named "WordCount.txt"

### If it shows "Compiling for Java version '1.7' is no longer supported. Minimal supported version is '1.8'" 

it means Eclipse is compiling with Java 7 instead of Java 8 or newer.

Follow these steps to fix it:

---

## 1. Verify Installed JDKs
1. Go to **Window → Preferences**.
2. Navigate to: **Java → Installed JREs**.
3. Ensure `jdk1.8.x` or `openjdk-1.8.x` is listed.
   - If not, click **Add… → Standard VM** and browse to your Java 8 installation:
     ```
     /usr/lib/jvm/java-8-openjdk-amd64
     ```
   - Select it and set it as the **default JRE**.

---

## 2. Configure Execution Environment
1. In Preferences, go to **Java → Installed JREs → Execution Environments**.
2. Select **JavaSE-1.8**.
3. Under **Compatible JREs**, tick your Java 8 JDK.
4. Apply and Close.

---

## 3. Update Project JRE
1. Right-click your project → **Properties**.
2. Go to **Java Build Path → Libraries**.
3. If it shows **JRE System Library [JavaSE-1.7]**:
   - Remove it.
   - Click **Add Library → JRE System Library**.
   - Choose **Execution Environment: JavaSE-1.8** (or Workspace default JRE if set to 1.8).
4. Apply and Close.

---

## 4. Set Compiler Compliance Level
1. Right-click your project → **Properties**.
2. Go to **Java Compiler**.
3. Check **Enable project specific settings**.
4. Set **Compiler compliance level = 1.8**.
5. Apply and Close.

---

## 5. Clean & Rebuild
1. From the top menu: **Project → Clean…**.
2. Select your project and rebuild.

---

## ✅ Done
Your project should now compile with **Java 8** instead of Java 7.

### Output jar:
* Right click on project and select "Export".
* Choose type "Java" -> "Jar file" and click "next".
* Select the export destination as "home" -> "git" -> "SimpleWordCount" -> "jar" and click "Finish".

### Hadoop Commands:
```
hadoop fs -mkdir InputFolder                                      //to create a new input folder
hadoop fs -copyFromLocal <input file> InputFolder                  //to copy a file from local directory to hadoop environment
hadoop fs -ls InputFolder                                          //to see the files inside "InputFolder"
hadoop jar <jar file name> <class name> InputFolder OutputFolder   //running mapreduce operation
hadoop fs -ls OutputFolder                                        //to see the files inside "OutputFolder"
hadoop fs -cat OutputFolder/part-r-00000                          //to see the content inside "OutputFolder/part-r-00000" file
hadoop fs -rm -r OutputFolder                                     //to remove "OutputFolder" directory and all its files
```

- remove OutputFolder before generating the next results.
- remove/clean InputFolder if you want to use a different file as input.


### Common Errors:
Error 1: mkdir: Call From cs6304-mrpk9-02/127.0.1.1 to localhost:9000 failed on connection exception: java.net.ConnectException: Connection refused  
Explanation and Fix: In general this error comes if you are running hadoop first time on your VM after a reset. The below commands will fix it.
```
stop-all.sh
hadoop namenode -format
start-all.sh
```
You can use the below command to check if namenode, datanode and nodemanager are running.
```
jps

```

Error 2: mkdir: `hdfs://localhost:9000/user/<username>': No such file or directory  
Explanation and Fix: The error comes when there is no directory /user and /user/<username> in hdfs and you are trying to create a folder using "hadoop fs -mkdir InputFolder ".   
Below command will create the directory structure if required and solves the problem.
```
hdfs dfs -mkdir -p InputFolder
```

Warning 1: WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable  
Fix: You can just ignore this warning.


