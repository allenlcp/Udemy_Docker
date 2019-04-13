FROM openjdk:8

RUN apt-get update && apt-get install -y git \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /opt/allenlcp/

RUN git clone https://github.com/allenlcp/docker_temp.git

WORKDIR /opt/allenlcp/docker_temp/section072_command_line/

CMD ["./mvnw","spring-boot:run"]

   