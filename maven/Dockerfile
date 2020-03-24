# Use Debian stable as the base image
FROM debian:stable

# Meta info
LABEL maintainer "julius.astrab@live.com"

# Workdir
WORKDIR /opt/

# Download OpenJDK
ADD https://download.java.net/openjdk/jdk11/ri/openjdk-11+28_linux-x64_bin.tar.gz  .

# Download Maven
ADD http://us.mirrors.quenda.co/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz .

# Add startup script.
ADD https://raw.githubusercontent.com/juliusastrab/docker_files/master/maven/entrypoint.sh .

# Prepare the container for ElasticSearch.
RUN mkdir -p /opt/java; mkdir -p /opt/maven; \
    tar -xf openjdk-11+28_linux-x64_bin.tar.gz -C /opt/java --strip-components=1; \
    tar -xf apache-maven-3.6.3-bin.tar.gz -C /opt/maven --strip-components=1; \
    useradd -u 1001 --shell /bin/bash maven; \
    chown -R 1001:1001 /opt/*; \
    chmod +x /opt/entrypoint.sh;

# Setup persistent storage
VOLUME ["/usr/src/mymaven", "/root/.m2", "/usr/src/mymaven/target"]

# Change user to elastic user.
USER maven

# Setups env
ENV JAVA_HOME "/opt/java/"
ENV MAVEN_HOME "/opt/maven/"
ENV PATH "${PATH}:${JAVA_HOME}/bin:${MAVEN_HOME}/bin"
#ENV MAVEN_PATH_CONF "/config"

# Starts ElasticSearch
CMD /opt/entrypoint.sh