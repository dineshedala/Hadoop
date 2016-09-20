There are 3 modes in installation in Hadoop.
  1. Local (Standalone) Mode
  2. Pseudo-Distributed Mode
  3. Fully-Distributed Mode

Basic requirements for installing hadoop 1.x is:
  1. Nix Operating system i.e linux or unix operating systems
  2. Java 1.7 or later
  3. SSH (Secure Shell)

## Local Mode ##
* In this mode it will act as a simple Java Program.
* To run Hadoop in Local Mode, set the JAVA_HOME environment variable in hadoop/conf/hadoop-env.sh
* None of the Hadoop deamons will be running in this mode.
* Do the following changes in ~/.bashrc file.
```
  export JAVA_HOME=/usr/local/jdk1.7.0_71
  export PATH=$PATH:$JAVA_HOME/bin 
```
and now apply changes into the current system
```
  $source ~/.bashrc
```
* Do the following change in hadoop-env.sh

  `export JAVA_HOME=/path/to/java_home`

## Pseudo-Distributed Mode ##
* In this mode all the hadoop deamons will be running in a single machine.
* To install Hadoop in Pseudo-Distributed Mode, we need to change the following files:
```
  1. hadoop-env.sh
  
# Set Hadoop-specific environment variables here.

# The only required environment variable is JAVA_HOME.  All others are
# optional.  When running a distributed configuration it is best to
# set JAVA_HOME in this file, so that it is correctly defined on
# remote nodes.

# The java implementation to use.  Required.
# export JAVA_HOME=/usr/lib/j2sdk1.6-sun

# Extra Java CLASSPATH elements.  Optional.
# export HADOOP_CLASSPATH="<extra_entries>:$HADOOP_CLASSPATH"

# The maximum amount of heap to use, in MB. Default is 1000.
# export HADOOP_HEAPSIZE=2000

# Extra Java runtime options.  Empty by default.
# if [ "$HADOOP_OPTS" == "" ]; then export HADOOP_OPTS=-server; else HADOOP_OPTS+=" -server"; fi

# Command specific options appended to HADOOP_OPTS when specified
export HADOOP_NAMENODE_OPTS="-Dcom.sun.management.jmxremote $HADOOP_NAMENODE_OPTS"
export HADOOP_SECONDARYNAMENODE_OPTS="-Dcom.sun.management.jmxremote $HADOOP_SECONDARYNAMENODE_OPTS"
export HADOOP_DATANODE_OPTS="-Dcom.sun.management.jmxremote $HADOOP_DATANODE_OPTS"
export HADOOP_BALANCER_OPTS="-Dcom.sun.management.jmxremote $HADOOP_BALANCER_OPTS"
export HADOOP_JOBTRACKER_OPTS="-Dcom.sun.management.jmxremote $HADOOP_JOBTRACKER_OPTS"
# export HADOOP_TASKTRACKER_OPTS=
# The following applies to multiple commands (fs, dfs, fsck, distcp etc)
# export HADOOP_CLIENT_OPTS

# Extra ssh options.  Empty by default.
# export HADOOP_SSH_OPTS="-o ConnectTimeout=1 -o SendEnv=HADOOP_CONF_DIR"

# Where log files are stored.  $HADOOP_HOME/logs by default.
# export HADOOP_LOG_DIR=${HADOOP_HOME}/logs

# File naming remote slave hosts.  $HADOOP_HOME/conf/slaves by default.
# export HADOOP_SLAVES=${HADOOP_HOME}/conf/slaves

# host:path where hadoop code should be rsync'd from.  Unset by default.
# export HADOOP_MASTER=master:/home/$USER/src/hadoop

# Seconds to sleep between slave commands.  Unset by default.  This
# can be useful in large clusters, where, e.g., slave rsyncs can
# otherwise arrive faster than the master can service them.
# export HADOOP_SLAVE_SLEEP=0.1

# The directory where pid files are stored. /tmp by default.
# export HADOOP_PID_DIR=/var/hadoop/pids

# A string representing this instance of hadoop. $USER by default.
# export HADOOP_IDENT_STRING=$USER

# The scheduling priority for daemon processes.  See 'man nice'.
# export HADOOP_NICENESS=10
  
  2. core-site.xml
  
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
 
<!-- Put site-specific property overrides in this file. -->
 
<configuration>
 
</configuration>  
  
  3. hdfs-site.xml
  
<property> 
     <name>dfs.permissions</name> 
     <value>true</value> 
     <description> If "true", enable permission checking in
     HDFS. If "false", permission checking is turned
     off, but all other behavior is
     unchanged. Switching from one parameter value to the other does
     not change the mode, owner or group of files or
     directories. </description> 
</property> 
 
<property> 
     <name>dfs.permissions.supergroup</name> 
     <value>hdfs</value> 
     <description>The name of the group of
     super-users.</description> 
</property> 
 
<property> 
     <name>dfs.namenode.handler.count</name> 
     <value>100</value> 
     <description>Added to grow Queue size so that more
     client connections are allowed</description> 
</property> 
 
<property> 
     <name>ipc.server.max.response.size</name> 
     <value>5242880</value> 
</property> 
 
<property> 
     <name>dfs.block.access.token.enable</name> 
     <value>true</value> 
     <description> If "true", access tokens are used as capabilities
     for accessing datanodes. If "false", no access tokens are checked on
     accessing datanodes. </description> 
</property> 
 
<property> 
     <name>dfs.namenode.kerberos.principal</name> 
     <value>nn/_HOST@EXAMPLE.COM</value> 
     <description> Kerberos principal name for the
     NameNode </description> 
</property> 
 
<property> 
     <name>dfs.secondary.namenode.kerberos.principal</name> 
     <value>nn/_HOST@EXAMPLE.COM</value> 
     <description>Kerberos principal name for the secondary NameNode. 
     </description> 
</property> 
 
<property> 
     <!--cluster variant --> 
     <name>dfs.secondary.http.address</name> 
     <value>ip-10-72-235-178.ec2.internal:50090</value> 
     <description>Address of secondary namenode web server</description> 
</property> 
 
<property> 
     <name>dfs.secondary.https.port</name> 
     <value>50490</value> 
     <description>The https port where secondary-namenode
     binds</description> 
</property> 
 
<property> 
     <name>dfs.web.authentication.kerberos.principal</name> 
     <value>HTTP/_HOST@EXAMPLE.COM</value> 
     <description> The HTTP Kerberos principal used by Hadoop-Auth in the HTTP endpoint.
     The HTTP Kerberos principal MUST start with 'HTTP/' per Kerberos HTTP
     SPNEGO specification. 
     </description> 
</property> 
 
<property> 
     <name>dfs.web.authentication.kerberos.keytab</name> 
     <value>/etc/security/keytabs/spnego.service.keytab</value> 
     <description>The Kerberos keytab file with the credentials for the HTTP
     Kerberos principal used by Hadoop-Auth in the HTTP endpoint. 
     </description> 
</property> 
 
<property> 
     <name>dfs.datanode.kerberos.principal</name> 
     <value>dn/_HOST@EXAMPLE.COM</value> 
     <description> 
     The Kerberos principal that the DataNode runs as. "_HOST" is replaced by the real
     host name. 
     </description> 
</property> 
 
<property> 
     <name>dfs.namenode.keytab.file</name> 
     <value>/etc/security/keytabs/nn.service.keytab</value> 
     <description> 
     Combined keytab file containing the namenode service and host
     principals. 
     </description> 
</property> 
 
<property> 
     <name>dfs.secondary.namenode.keytab.file</name> 
     <value>/etc/security/keytabs/nn.service.keytab</value> 
     <description> 
     Combined keytab file containing the namenode service and host
     principals. 
     </description> 
</property> 
 
<property> 
     <name>dfs.datanode.keytab.file</name> 
     <value>/etc/security/keytabs/dn.service.keytab</value> 
     <description> 
     The filename of the keytab file for the DataNode. 
     </description> 
</property> 
 
<property> 
     <name>dfs.https.port</name> 
     <value>50470</value> 
     <description>The https port where namenode
     binds</description> 
</property> 
 
<property> 
     <name>dfs.https.address</name> 
     <value>ip-10-111-59-170.ec2.internal:50470</value> 
     <description>The https address where namenode binds</description> 
</property> 
 
<property> 
     <name>dfs.datanode.data.dir.perm</name> 
     <value>750</value> 
     <description>The permissions that should be there on
     dfs.data.dir directories. The datanode will not come up if the
     permissions are different on existing dfs.data.dir directories. If
     the directories don't exist, they will be created with this
     permission.</description> 
</property> 
 
<property> 
     <name>dfs.access.time.precision</name> 
     <value>0</value> 
     <description>The access time for HDFS file is precise upto this
     value.The default value is 1 hour. Setting a value of 0
     disables access times for HDFS. 
     </description> 
</property> 
 
<property> 
     <name>dfs.cluster.administrators</name> 
     <value> hdfs</value> 
     <description>ACL for who all can view the default
     servlets in the HDFS</description> 
</property> 
 
<property> 
     <name>ipc.server.read.threadpool.size</name> 
     <value>5</value> 
     <description></description> 
</property> 
 
<property> 
     <name>dfs.namenode.kerberos.internal.spnego.principal</name> 
     <value>${dfs.web.authentication.kerberos.principal}</value> 
</property> 
 
<property> 
     <name>dfs.secondary.namenode.kerberos.internal.spnego.principal</name> 
     <value>${dfs.web.authentication.kerberos.principal}</value> 
</property>   
  
  4. mapred-site.xml
  
<property>
 <name> </name>
 <value> </value>
 <description> </description>
</property>  
  
  5. masters
  6. slaves
```

