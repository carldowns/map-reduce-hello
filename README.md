Sample Hadoop MapReduce Program
-----------

### Installation

http://blog.cloudera.com/blog/2014/09/how-to-install-cdh-on-mac-osx-10-9-mavericks/


### HDFS Commands

        $ start-dfs.sh
        $ stop-dfs.sh
        
        FIX: Hit a snag with the htfs-core.xml file, had the ‘name’ and ‘data’ declarations pointing to the same directory.
        
        list files
        $ hadoop fs -ls R
        
        ~/work/tools/hadoop $ hadoop fs -ls -R
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-07 14:28 test
        -rw-r--r--   1 carl.downs supergroup       8469 2014-10-07 14:22 test/README.md
        -rw-r--r--   1 carl.downs supergroup          0 2014-10-07 14:27 test/touched.txt
        
        list files showing ownership
        
        $ hadoop fs -ls R /
        
        ~/work/tools/hadoop $ hadoop fs -ls -R /
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-07 14:21 /user
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-07 14:25 /user/carl.downs
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-07 14:28 /user/carl.downs/test
        -rw-r--r--   1 carl.downs supergroup       8469 2014-10-07 14:22 /user/carl.downs/test/README.md

### To Run A Sample MR Job

        http://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
        
        Create a MR java file per spec.  This is for a simple hello / WordCount.java program.
        
        Compile:
        
        $ hadoop com.sun.tools.javac.Main WordCount.java
        $ jar cf wc.jar WordCount*.class
        
        Then check the jar
        
        $ jar -tvf wc.jar
        
        Put your input file(s) up there
        
        $ hadoop fs -put /Users/carl.downs/work/tools/hadoop/wordfiles/en.fr.dictionary.txt /user/carl.downs/mr1
        
        Run (output dir should not exist):
        
        $ hadoop jar wc.jar WordCount /user/carl.downs/mr1 /user/carl.downs/mr2.out2
        $ hadoop jar mapreduce-1.0-SNAPSHOT.jar /user/carl.downs/mr6 /user/carl.downs/whatever
        
        Look at results
        
        $ hadoop fs -cat /user/carl.downs/mr2.out2/part-r-00000

### To Reboot your HDFS System
        Stop processes and check that they are down
        
        $ stop-mapred.sh
        $ stop-dfs.sh
        $ jps
        
        HDFS is pointing at these locations for name:

        ~/work/tools/hadoop $ grep -r --include \*.xml "carl.downs" *
        hadoop-2.0.0-cdh4.1.2/etc/hadoop/core-site.xml:    <value>/Users/carl.downs/work/tools/hadoop/tmp</value>
        hadoop-2.0.0-cdh4.1.2/etc/hadoop/hdfs-site.xml:    <value>/Users/carl.downs/work/tools/hadoop/dfs/name</value>
        hadoop-2.0.0-cdh4.1.2/etc/hadoop/hdfs-site.xml:    <value>/Users/carl.downs/work/tools/hadoop/dfs/data</value>
        hadoop-2.0.0-mr1-cdh4.1.2/conf/core-site.xml:    <value>/Users/carl.downs/work/tools/hadoop/tmp</value>
        hadoop-2.0.0-mr1-cdh4.1.2/conf/hdfs-site.xml:    <value>/Users/carl.downs/work/tools/hadoop/dfs/name</value>
        hadoop-2.0.0-mr1-cdh4.1.2/conf/hdfs-site.xml:    <value>/Users/carl.downs/work/tools/hadoop/dfs/data</value>
        
        Nuke these (dfs, tmp)
        
        $ rm -rf tmp, dfs
        
        Reformat them:
        
        $ hadoop namenode -format
        
        recreate these directories
        
        $ mkdir dfs
        $ mkdir tmp
        
        Reformat the file system:
        
        $ hadoop namenode -format
        
        Start DFS and check the dir.
        HDFS is mapped to tmp, there is nothing in it.
        
        $ start-dfs.sh
        $ hadoop fs -ls -R /

### Start map reduce, a user of the HDFS.  It should populate the DFS mount.

        $ start-mapred.sh
        $ hadoop fs -ls -R /
        
        ~/work/tools/hadoop $ hadoop fs -ls -R /
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-08 11:01 /Users
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-08 11:01 /Users/carl.downs
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-08 11:01 /Users/carl.downs/work
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-08 11:01 /Users/carl.downs/work/tools
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-08 11:01 /Users/carl.downs/work/tools/hadoop
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-08 11:01 /Users/carl.downs/work/tools/hadoop/tmp
        drwxr-xr-x   - carl.downs supergroup          0 2014-10-08 11:01 /Users/carl.downs/work/tools/hadoop/tmp/mapred
        drwx------   - carl.downs supergroup          0 2014-10-08 11:01 /Users/carl.downs/work/tools/hadoop/tmp/mapred/system
        -rw-------   1 carl.downs supergroup          4 2014-10-08 11:01 /Users/carl.downs/work/tools/hadoop/tmp/mapred/system/jobtracker.info



END
-----------


