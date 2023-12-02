# Spark Installation on windows

Spark version - 3.3.3

- Install java jdk of compatible version as spark runs on jvm. 

- Set `JAVA_HOME` environment variable if not defined already.
- Set `PATH = %PATH%;%JAVA_HOME%`

- Download spark hadoop combine package from apache. Extract it in folder `D:\spark-setup\`
```bash
wget https://dlcdn.apache.org/spark/spark-3.3.3/spark-3.3.3-bin-hadoop3.tgz
```

This will install hadoop of version 3.3.2

- Download `winutils.exe` of compatible hadoop version which is Windows binaries for Hadoop.

```bash
wget https://github.com/kontext-tech/winutils/blob/master/hadoop-3.3.1/bin/winutils.exe 
```

- Set below environment variables
```bash
SET SPARK_HOME=D:\spark-setup\spark-3.3.3-bin-hadoop3
SET HADOOP_HOME=D:\spark-setup\spark-3.3.3-bin-hadoop3
SET PATH=%PATH%;%SPARK_HOME%;%SPARK_HOME%\bin
```

spark-shell use below keys to exit
```
clt+D
```

