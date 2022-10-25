FROM eclipse-temurin:11-jdk

# JMeter Version
ARG JMETER_VERSION="5.5"

# Download and extract the JMeter tar file
RUN cd /opt \
 && wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz \
 && tar xzf apache-jmeter-${JMETER_VERSION}.tgz \
 && rm apache-jmeter-${JMETER_VERSION}.tgz
 
# Create a symlink to the jmeter process in a normal bin directory
RUN ln -s /opt/apache-jmeter-${JMETER_VERSION}/bin/jmeter /usr/local/bin

# Indicate the default command to run
CMD jmeter
