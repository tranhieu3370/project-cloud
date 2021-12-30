# project-cloud
I/install hadoop 
 1. Installing Java
 - sudo apt-get install default-jdk
 2. Adding a dedicated Hadoop user
 - sudo addgroup hadoop
 3. Tạo hadoop group và hduser user
 - sudo adduser hduser sudo
 4. Installing SSH
 - sudo apt-get install ssh
 5. Verify installation
 - which ssh
 - which sshd
 6.
 - su hduser
 - /home/bhaskar$ ssh-keygen -t rsa -P ""
 7. cấp quyền cho ssh key
 - chmod 0600 ~/.ssh/authorized_keys
 8. check ssh
 - /home/bhaskar$ ssh localhost
 9. move the Hadoop installation to the /usr/local/hadoop directory
 - /home/bhaskar$ sudo mkdir -p /usr/local/hadoop
 10. Install Hadoop
 - /home/bhaskar$ cd /home/bhaskar/Downloads/hadoop-2.6.5/
 - move the Hadoop installation to the /usr/local/hadoop directory
   hduser@D:/home/bhaskar/home/bhaskar/Downloads/hadoop-2.6.5$ sudo mv * /usr/local/hadoop/
   (or)
   If you directly downloaded from mirros, follow this
   hduser@D:/home/bhaskar$
   wget http://mirrors.sonic.net/apache/hadoop/common/hadoop-2.6.5/hadoop-2.6.5.tar.gz
   (check pro per mirror)
   hduser@D:/home/bhaskar$ tar xvzf hadoop-2.6.5.tar.gz
   Move to the folder, where your hadoop download is available and execute the following
   sudo mv * /usr/local/hadoop/
 11. set read/wri- te permission
 - /home/bhaskar$ sudo chown -R hduser:hadoop /usr/local/hadoop
 12. cấu hình file .bashrc
 -  /home/bhaskar$ vim ~/.bashrc
 - Add the following @ end
    #HADOOP VARIABLES START
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    export HADOOP_INSTALL=/usr/local/hadoop
    export PATH=$PATH:$HADOOP_INSTALL/bin
    export PATH=$PATH:$HADOOP_INSTALL/sbin
    export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
    export HADOOP_COMMON_HOME=$HADOOP_INSTALL
    export HADOOP_HDFS_HOME=$HADOOP_INSTALL
    export YARN_HOME=$HADOOP_INSTALL
    export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native
    export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib"
    export HADOOP_HOME_WARN_SUPPRESS=1
    export HADOOP_ROOT_LOGGER="WARN,DRFA"
    #HADOOP VARIABLES END
 13. chạy lại file ./bashrc
 - /home/bhaskar$ source ~/.bashrc
 14. sửa file hadoop-env.sh
 - Add the following
   export JAVA_HOME==/usr/lib/jvm/java-8-openjdk-amd64
 15. tạo file 
 - /home/bhaskar$ sudo mkdir -p /app/hadoop/tmp
 - /home/bhaskar$sudo chown hduser:hadoop /app/hadoop/tmp
 16. sửa file core-site.xml
 - /home/bhaskar$ vi /usr/local/hadoop/etc/hadoop/core-site.xml
     Open the file and enter the following in between the <configuration></configuration> tag:
    <property>
     <name>hadoop.tmp.dir</name>
     <value>/app/hadoop/tmp</value>
     <description>A base for other temporary directories.</description>
    A.Baskar Page 5
    </property>
    <property>
     <name>fs.default.name</name>
     <value>hdfs://localhost:54310</value>
     <description>The name of the default file system. A URI whose
     scheme and authority determine the FileSystem implementation. The
     uri's scheme determines the config property (fs.SCHEME.impl) naming
     the FileSystem implementation class. The uri's authority is used to
     determine the  the host, port, etc. for a filesystem.</description>
    </property>
  17. 
     /home/bhaskar$ cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template
    /usr/local/hadoop/etc/hadoop/mapred-site.xml
  18. sửa file mapred-site.xml
  - /home/bhaskar$ cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template
    Open the file and enter the following in between the <configuration></configuration> tag:
    Add the following
    <property>
     <name>mapred.job.tracker</name>
     <value>localhost:54311</value>
     <description>The host and port that the MapReduce job tracker runs
     at. If "local", then jobs are run in-process as a single map
     and reduce task.
     </description>
    </property>
  19.
    /home/bhaskar$ sudo mkdir -p /usr/local/hadoop_store/hdfs/namenode
    /home/bhaskar$ sudo mkdir -p /usr/local/hadoop_store/hdfs/datanode
    /home/bhaskar$ sudo chown -R hduser:hadoop /usr/local/hadoop_store
  20.sửa file hdfs
    /home/bhaskar$ vim /usr/local/hadoop/etc/hadoop/hdfs-site.xml
    Open the file and enter the following in between the <configuration></configuration> tag:
    And add the following
    A.Baskar Page 6
    <property>
     <name>dfs.replication</name>
     <value>1</value>
     <description>Default block replication.
     The actual number of replications can be specified when the file is created.
     The default is used if replication is not specified in create time.
     </description>
    </property>
    <property>
     <name>dfs.namenode.name.dir</name>
     <value>file:/usr/local/hadoop_store/hdfs/namenode</value>
    </property>
    <property>
     <name>dfs.datanode.data.dir</name>
     <value>file:/usr/local/hadoop_store/hdfs/datanode</value>
    </property>
  21. 
      - /home/bhaskar$ hadoop namenode –format
  22. chạy hadoop
      - /home/bhaskar$ start-all.sh
II/ install hive
    Steps for hive installation
    •	Download and Unzip Hive
    •	Edit .bashrc file
    •	Edit hive-config.sh file
    •	Create Hive directories in HDFS
    •	Initiate Derby database
    •	Configure hive-site.xml file

    download and unzip Hive
    =============================
    wget https://downloads.apache.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz
    tar xzf apache-hive-3.1.2-bin.tar.gz

    Edit .bashrc file
    ========================
    sudo nano .bashrc
    export HIVE_HOME= /home/hdoop/apache-hive-3.1.2-bin

    export PATH=$PATH:$HIVE_HOME/bin
    source ~/.bashrc
    Edit hive-config.sh file
    ====================================
    sudo nano $HIVE_HOME/bin/hive-config.sh
    export HADOOP_HOME=/home/hdoop/hadoop-3.2.1
    Create Hive directories in HDFS
    ===================================
    hdfs dfs -mkdir /tmp
    hdfs dfs -chmod g+w /tmp
    hdfs dfs -mkdir -p /user/hive/warehouse
    hdfs dfs -chmod g+w /user/hive/warehouse

    Fixing guava problem – Additional step
    =================

    rm $HIVE_HOME/lib/guava-19.0.jar
    cp $HADOOP_HOME/share/hadoop/hdfs/lib/guava-27.0-jre.jar $HIVE_HOME/lib/

    Initialize Derby and hive
    ============================
    schematool -initSchema -dbType derby


    hive

    optional Step – Edit hive-site.xml
    ===========
    cd $HIVE_HOME/conf
    cp hive-default.xml.template hive-site.xml
    sudo nano hive-site.xml – change metastore location to above created hdfs path(/user/hive/warehouse)

 
 
 
