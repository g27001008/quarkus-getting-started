FROM eclipse-temurin:11-jdk

# Download, extract and install the AWS CLI installer
RUN cd /opt \ 
 && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
 && unzip awscliv2.zip \
 && sudo ./aws/install

RUN aws --version

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
