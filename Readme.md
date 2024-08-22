# About the repo
`I = "Original author"`
I started this repo because I wanted to learn PySpark.
However, I also didn't want to use Jupyter notebook as it
is typically the case in the examples I came across. 

Therefore, I started with setting up a spark cluster 
using docker. 




# Running the code (Spark standalone cluster)
You can run the spark standalone cluster by running:
```shell
make run
```
or with 3 workers using:
```shell
make run-scaled
```

---

1. Get into the master to make changes to the configuration files
```shell
docker exec -it da-spark-master /bin/bash
```
2. `cd /opt/spark/conf`
3. mv log4j2.properties.template log4j.properties
4. `vi log4j.properties`
5. Update the log from `info` to `Error`


# Set everything to be logged to the console
`rootLogger.level = error`
`rootLogger.appenderRef.stdout.ref = console`

To save the changes you made in vi, ensure you are in command mode by pressing Esc.
Type :w and press Enter. This command writes (saves) the file.
If you want to save and exit vi in one step, you can use :wq and press Enter.

---



You can submit Python jobs with the command:
```shell
make submit app=dir/relative/to/spark_apps/dir
```
e.g. 
```shell
make submit app=data_analysis_book/chapter03/word_non_null.py
```

There are a number of commands to build the standalone cluster,
you should check the Makefile to see them all. But the
simplest one is:
```shell
make build
```

## Web UIs
The master node can be accessed on:
`localhost:9090`. 
The spark history server is accessible through:
`localhost:18080`.


# Running the code (Spark on Hadoop Yarn cluster)
Before running, check the virtual disk size that Docker
assigns to the container. In my case, I needed to assign
some 70 GB.
You can run Spark on the Hadoop Yarn cluster by running:
```shell
make run-yarn
```
or with 3 data nodes:
```shell
make run-yarn-scaled
```
You can submit an example job to test the setup:
```shell
make submit-yarn-test
```
which will submit the `pi.py` example in cluster mode.

You can also submit a custom job:
```shell
make submit-yarn-cluster app=data_analysis_book/chapter03/word_non_null.py
```

There are a number of commands to build the cluster,
you should check the Makefile to see them all. But the
simplest one is:
```shell
make build-yarn
```

## Web UIs
You can access different web UIs. The one I found the most 
useful is the NameNode UI:
```shell
http://localhost:9870
```

Other UIs:
- ResourceManger - `localhost:8088`
- Spark history server - `localhost:18080`

# About the branch expose-docker-hostnames-to-host
The branch expose-docker-hostnames-to-host contains the 
shell scripts, templates, and Makefile modifications 
required to expose worker node web interfaces. To run 
the cluster you need to do the following. 
1. run
```shell
make run-ag n=3
```
which will generate a docker compose file with 3 worker
nodes and the appropriate yarn and hdfs site files
in the yarn-generated folder.
2. register the docker hostnames with /etc/hosts
```shell
sudo make dns-modify o=true
```
which will create a backup folder with your original
hosts file.

Once you are done and terminate the cluster, restore 
your original hosts file with:
```shell
sudo make dns-restore
```



For more information read the story that original author has published on Medium:
[here](https://medium.com/@MarinAgli1/using-hostnames-to-access-hadoop-resources-running-on-docker-5860cd7aeec1).
[Standalone Spark Cluster](https://medium.com/@MarinAgli1/setting-up-a-spark-standalone-cluster-on-docker-in-layman-terms-8cbdc9fdd14b)


