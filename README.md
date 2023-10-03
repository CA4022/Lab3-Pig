# Lab3-Pig

This lab covers the basics of PIG with  examples.

## Download and Install PIG
The official apache manual is availalbe [here](http://pig.apache.org/docs/r0.17.0/start.html). Key steps are summarised below.

* Get and install pig from one of the [mirror sites](http://www.apache.org/dyn/closer.cgi/pig/), copying it into the same directory where you have your hadoop root

   - `$ wget https://ftp.heanet.ie/mirrors/www.apache.org/dist/pig/pig-0.17.0/pig-0.17.0.tar.gz `
   - `$ tar -xvzf pig-0.17.0.tar.gz`
   - `$ cp -avi pig-0.17.0.tar.gz $HADOOP_HOME/../pig-0.17.0`
   
* Note you will need Java >6 and Hadoop 2.x or greater  
* Make sure your $HADOOP_HOME environment variable is correctly set
* Set $PIG_HOME and add pig to PATH
   
   - `$ export PIG_HOME=<your pig home>`
   - `$ export PATH=$PATH:$PIG_HOME/bin`

* Test your installation: `$ pig -help`
* Run the grunt shell
  - in local mode: `$ pig -x local`
  - in mapreduce mode (default): `$ pig -x mapreduce`
* Exit the grunt shell: `$grunt> quit`

## PIG Examples
1. Let's try Wordcount (locally first)

```
lines = LOAD 'local_file.txt' AS (line:chararray);
words = FOREACH lines GENERATE FLATTEN(TOKENIZE(line)) as word;
grouped = GROUP words BY word;
wordcount = FOREACH grouped GENERATE group, COUNT(words);
DUMP wordcount;
```
* NOTE: if you run pig on mapreduce, you need to make sure the input file is on HDFS, using 'hdfs://localhost:8020/user/<username>/input/input_file.txt'

2. A Toy Example:
      - Create two CSV file as follows:
            
            A.txt containing two lines: 0,1,2 and 1,3,4
            
            B.txt containing two lines: 0,5,2 and 1,7,8
   
      - Run the PIG Latin commands below, one by one from shell, and observe what is contained in d, e, f and g after each dump (note the content of g and reflect upon the way pig handles schema on the fly
```
a = LOAD ‘A.txt’ using PigStorage(‘,’); 
b = LOAD ‘B.txt’ using PigStorage(‘,’); 
c = UNION a,b;
SPLIT c INTO d IF $0==0, e IF $0==1; 
dump d;
dump e;
f = FILTER c BY $1>3;
dump f;
A = LOAD ‘A.txt’ using PigStorage(‘,’) as (a1:int, a2:int, a3:int); 
B = LOAD ‘B.txt’ using PigStorage(‘,’) as (b1:int, b2:int, b3:int); 
C = UNION A,B;
G = FOREACH C GENERATE a2, a2*a3;
dump G;
```

3. More examples:
   - [PigLatin basics](http://pig.apache.org/docs/r0.17.0/basic.html#load) 
   - [git script examples](https://gist.github.com/brikis98/1332818)

