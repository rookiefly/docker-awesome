DOCKER_NETWORK = dockerhadoop_default
ENV_FILE = hadoop.env
current_branch = 2.10.1
build:
	docker build -t rookiefly/hadoop-base:$(current_branch) ./base
	docker build -t rookiefly/hadoop-namenode:$(current_branch) ./namenode
	docker build -t rookiefly/hadoop-datanode:$(current_branch) ./datanode
	docker build -t rookiefly/hadoop-resourcemanager:$(current_branch) ./resourcemanager
	docker build -t rookiefly/hadoop-nodemanager:$(current_branch) ./nodemanager
	docker build -t rookiefly/hadoop-historyserver:$(current_branch) ./historyserver
	docker build -t rookiefly/hadoop-submit:$(current_branch) ./submit

wordcount:
	docker build -t hadoop-wordcount ./submit
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(current_branch) hdfs dfs -mkdir -p /input/
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(current_branch) hdfs dfs -copyFromLocal /opt/hadoop-2.7.4/README.txt /input/
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} hadoop-wordcount
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(current_branch) hdfs dfs -cat /output/*
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(current_branch) hdfs dfs -rm -r /output
	docker run --network ${DOCKER_NETWORK} --env-file ${ENV_FILE} rookiefly/hadoop-base:$(current_branch) hdfs dfs -rm -r /input
