# References:
#   https://github.com/UIUC-CS498-Cloud/MP4_HadoopMapReduce_Docker/blob/master/Dockerfile
#   http://www.science.smith.edu/dftwiki/index.php/Tutorial:_Docker_Anaconda_Python_--_4

FROM ubuntu:20.04

# https://www.cyberciti.biz/faq/explain-debian_frontend-apt-get-variable-for-ubuntu-debian/
# zero interaction while installing or upgrading the system via apt. It accepts the default answer for all questions.
ENV DEBIAN_FRONTEND=noninteractive

# install curl + wget on your ubuntu image.
# https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
# WARNING: Always combine RUN apt-get update with apt-get install in the same RUN statement.
RUN apt-get update && \
    apt-get install -y curl vim wget expect git zip unzip

## Java installing
RUN mkdir -p /aicsvc/apps
RUN wget https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz
RUN tar xvfz openjdk-11.0.2_linux-x64_bin.tar.gz -C /aicsvc/apps
RUN ln -s /aicsvc/apps/jdk-11.0.2 /aicsvc/apps/java
ENV JAVA_HOME=/aicsvc/apps/java
ENV PATH="$JAVA_HOME/bin:${PATH}"

## Anaconda installing
RUN wget https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh
RUN bash Anaconda3-2020.11-Linux-x86_64.sh -b
RUN rm Anaconda3-2020.11-Linux-x86_64.sh

# Set path to conda
ENV PATH /root/anaconda3/bin:$PATH

# Updating Anaconda packages
#RUN conda update conda
#RUN conda update anaconda
#RUN conda update --all
RUN pip install pyspark==3.1.1 torch==1.8.1

## install haddop
RUN curl -s "https://archive.apache.org/dist/hadoop/core/hadoop-2.9.2/hadoop-2.9.2.tar.gz" | tar -xz -C /usr/local/
RUN cd /usr/local && ln -s ./hadoop-2.9.2 hadoop
ENV HADOOP_PREFIX /usr/local/hadoop
ENV HADOOP_COMMON_HOME /usr/local/hadoop
ENV HADOOP_HDFS_HOME /usr/local/hadoop
ENV HADOOP_MAPRED_HOME /usr/local/hadoop
ENV HADOOP_YARN_HOME /usr/local/hadoop
ENV HADOOP_CONF_DIR /usr/local/hadoop/etc/hadoop
ENV YARN_CONF_DIR $HADOOP_PREFIX/etc/hadoop
ENV HADOOP_CLASSPATH $JAVA_HOME/lib/tools.jar
ENV PATH="/usr/local/hadoop/bin:${PATH}"

RUN sed -i "/^export JAVA_HOME/ s:.*:export HADOOP_PREFIX=/usr/local/hadoop HADOOP_HOME=/usr/local/hadoop\n:" $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
RUN sed -i '/^export HADOOP_CONF_DIR/ s:.*:export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop/:' $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
RUN chmod a+rwx -R /usr/local/hadoop/

## Download and setup Apache Spark
RUN curl -s "https://archive.apache.org/dist/spark/spark-3.1.1/spark-3.1.1-bin-hadoop2.7.tgz" | tar -xz -C /usr/local/
RUN ln -s /usr/local/spark-3.1.1-bin-hadoop2.7 /usr/local/spark

ENV SPARK_HOME /usr/local/spark
ENV PATH="$SPARK_HOME/bin:${PATH}"
RUN chmod a+rwx -R $SPARK_HOME

# Make vim nice
RUN echo "set background=dark" >> ~/.vimrc
RUN echo "colorscheme torte" >> ~/.vimrc

ENV PYSPARK_PYTHON=python3

# download nltk stopwords
RUN pip3 install nltk
RUN [ "python", "-c", "import nltk; nltk.download('stopwords')" ]

# download source
RUN mkdir -p /workspace