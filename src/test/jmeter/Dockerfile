# build on the top of Java image
FROM eclipse-temurin:11.0.16.1_1-jdk
# JMeter Version
ARG JMETER_VERSION=”5.3"
# Download and unpack the JMeter tar file
RUN cd /opt \
 && wget https://mirrors.estointernet.in/apache//jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz \
 && tar xzf apache-jmeter-${JMETER_VERSION}.tgz \
 && rm apache-jmeter-${JMETER_VERSION}.tgz
# Create a symlink to the jmeter process in a normal bin directory
RUN ln -s /opt/apache-jmeter-${JMETER_VERSION}/bin/jmeter /usr/local/bin
# Copying custom property file
COPY user.properties /opt/apache-jmeter-${JMETER_VERSION}/bin/user.properties
# Indicate the default command to run
CMD jmeter
