FROM bde2020/hadoop-resourcemanager:2.0.0-hadoop3.2.1-java8

MAINTAINER Armin Zolfaghari <arminzolfagharid@gmail.com>

COPY sources.list /etc/apt/sources.list
RUN apt-get update &&  DEBIAN_FRONTEND=noninteractive apt-get install -y python3

EXPOSE 8088

CMD ["/run.sh"]
