FROM nginx
WORKDIR /home/
COPY ./part_4/nginx.conf /etc/nginx/nginx.conf
COPY ./part_3/miniserver.c .
COPY ./part_4/run.sh .
RUN apt-get update
RUN apt-get install -y gcc spawn-fcgi libfcgi-dev
ENTRYPOINT ["sh", "./run.sh"]
