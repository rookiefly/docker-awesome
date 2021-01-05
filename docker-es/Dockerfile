FROM docker.elastic.co/elasticsearch/elasticsearch:7.10.1

ENV VERSION=7.10.1
RUN sh -c '/bin/echo -e "y" | /usr/share/elasticsearch/bin/elasticsearch-plugin install https://hub.fastgit.org/medcl/elasticsearch-analysis-ik/releases/download/v${VERSION}/elasticsearch-analysis-ik-$VERSION.zip'