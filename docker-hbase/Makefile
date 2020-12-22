DOCKER_NETWORK = hbase
ENV_FILE = hadoop.env
current_branch := 2.3.3
hadoop_branch := 2.10.1
build:
	docker build -t rookiefly/hbase-base:$(current_branch) ./base
	docker build -t rookiefly/hbase-master:$(current_branch) ./hmaster
	docker build -t rookiefly/hbase-regionserver:$(current_branch) ./hregionserver
	docker build -t rookiefly/hbase-standalone:$(current_branch) ./standalone

wordcount:
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(hadoop_branch) hdfs dfs -mkdir -p /input/
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(hadoop_branch) hdfs dfs -copyFromLocal -f /opt/hadoop-2.7.4/README.txt /input/
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} hadoop-wordcount
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(hadoop_branch) hdfs dfs -cat /output/*
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(hadoop_branch) hdfs dfs -rm -r /output
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(hadoop_branch) hdfs dfs -rm -r /input
