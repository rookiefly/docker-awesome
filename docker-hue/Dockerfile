FROM gethue/hue:latest

COPY hue.ini /usr/share/hue/desktop/conf/hue.ini

COPY ./initdb.sh .

RUN chmod +x ./initdb.sh

CMD ["./initdb.sh"]