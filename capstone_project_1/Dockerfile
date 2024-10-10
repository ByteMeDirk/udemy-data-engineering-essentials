FROM ubuntu:20.04

# Set environment variables
ENV SPARK_VERSION=3.5.3
ENV HADOOP_VERSION=3
ENV JAVA_VERSION=11
ENV PYTHON_VERSION=3.9

# Install dependencies
RUN apt-get update && apt-get install -y \
    openjdk-${JAVA_VERSION}-jdk \
    python${PYTHON_VERSION} \
    python3-pip \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Download and install Spark
RUN wget https://downloads.apache.org/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
    && tar -xzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
    && mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} /opt/spark \
    && rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz

# Set Spark environment variables
ENV SPARK_HOME=/opt/spark
ENV PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin

# Install PySpark
RUN pip3 install pyspark==${SPARK_VERSION}

# Copy configuration files
COPY spark-defaults.conf $SPARK_HOME/conf/
COPY spark-env.sh $SPARK_HOME/conf/

# Set working directory
WORKDIR $SPARK_HOME

# Copy entrypoint script
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]