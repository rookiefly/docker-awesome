FROM rookiefly/hadoop-base:2.10.1
MAINTAINER rookiefly <rookiefly@163.com>

HEALTHCHECK CMD curl -f http://localhost:8088/ || exit 1

ADD run.sh /run.sh
RUN chmod a+x /run.sh

EXPOSE 8088

CMD ["/run.sh"]
