FROM ubuntu:22.04
LABEL dev="Ishika"

# Install required packages
RUN apt-get update && \
    apt-get install -y default-jdk unzip curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Define environment variables
ENV TOMCAT_VERSION=9.0.113
ENV CATALINA_HOME=/opt/tomcat
ENV PATH=$CATALINA_HOME/bin:$PATH

# Download and extract Tomcat
RUN curl -fSL https://dlcdn.apache.org/tomcat/tomcat-9/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.zip -o /tmp/tomcat.zip && \
    unzip /tmp/tomcat.zip -d /opt && \
    mv /opt/apache-tomcat-${TOMCAT_VERSION} $CATALINA_HOME && \
    rm /tmp/tomcat.zip

# Set working directory
WORKDIR $CATALINA_HOME

# Download application and MySQL connector
RUN curl -fSL https://s3-us-west-2.amazonaws.com/studentapi-cit/student.war \
        -o webapps/student.war && \
    curl -fSL https://s3-us-west-2.amazonaws.com/studentapi-cit/mysql-connector.jar \
        -o lib/mysql-connector.jar

# Copy context.xml
COPY context.xml conf/context.xml

# Set execute permission for Tomcat scripts
RUN chmod +x bin/*.sh

# Expose Tomcat port
EXPOSE 8080

# Start Tomcat (shell form allows env variable expansion)
CMD ["sh", "-c", "catalina.sh run"]
