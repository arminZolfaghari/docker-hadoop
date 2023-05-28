# Hadoop on Docker

## Requirement
Docker Desktop is required, and to check that it is correctly installed, you can run the following command:
```
docker run hello-world
```
## Start Hadoop Cluster
Go to the terminal and clone the git repository to your computer:
```
git clone https://github.com/arminZolfaghari/docker-hadoop.git
```
And get to the docker-hadoop directory and run the following command to create containers:
```
docker-compose up -d
```
It takes a few minutes to start the whole Hadoop cluster completely.

Use `docker ps` to verify the containers are up.

## Running Python MapReduce function
We will use the word count example for the MapReduce function. You can see the MapReduce codes and input text file [here](https://github.com/arminZolfaghari/docker-hadoop/tree/master/mapreduce%20codes).

### How to put files or directory in containers from the host computer
You can copy the local directory to the container with following command:
```
docker cp LOCAL_PATH CONTAINER:PATH
```
for example, we put `mapreduce codes` directory in namenode container:
``` 
docker cp ./mapreduce\ codes namenode:codes
```

### How to put files in HDFS from the container
First of all, you must go to the namenode container:
```
docker exec -it namenode bash
```
If you run `ls` command, you should find `codes` directory in the namenode container.
The MapReduce program access files from the Hadoop Distributed File System (HDFS). Run the following to transfer the input file text to HDFS:
```
hadoop fs -mkdir -p input
hdfs dfs -put ./codes/input.txt input
```
To check you can see https://localhost:9870 and select `Utilities`, then select `Browse the file system`

### Run Python MapReduce in namenode
** Note: Because Hadoop runs on Apache server which is built in Java, the program takes a Java JAR file as an input. To execute Python in Hadoop, we will need to use the Hadoop Streaming library to pipe the Python executable into the Java framework **

Use `find / -name 'hadoop-streaming*.jar` to locate the hadoop string library JAR file. The path should look something like `PATH/hadoop-straming-3.2.1.jar`
Finally, we can execute the MapReduce program with command:
```
hadoop jar /opt/hadoop-3.2.1/share/hadoop/tools/lib/hadoop-streaming-3.2.1.jar -file ./codes/mapper.py \
-mapper "python3 mapper.py" -file ./codes/reducer.py -reducer "python3 reducer.py" \
-input input/input.txt -output output/PATH
```
For see the result:
```
hdfs dfs -cat output/PATH/part-00000
```
Also, you can see the result at https://localhost:9870. Select `Utilities`, then select `Browse the file system`.

### Shut down Hadoop Cluster
To safely shut down the cluster and remove containers, run:
```
docker-compose down
```

## Credits
This repository is inspired by [@big-data-europe](https://github.com/big-data-europe/docker-hadoop).

## Contact
If you have any questions, feel free to ask me: </br>
:envelope_with_arrow:	arminzolfagharid@gmail.com


