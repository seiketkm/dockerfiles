FROM openjdk:11-jdk
ARG VERSION=stanford-corenlp-full-2018-10-05
EXPOSE 9000
RUN wget http://nlp.stanford.edu/software/$VERSION.zip &&\
    unzip $VERSION.zip &&\
    rm $VERSION.zip
WORKDIR $VERSION
RUN export CLASSPATH="`find . -name '*.jar'`"
ENTRYPOINT ["java", "-mx4g", "-cp", "'*'", "edu.stanford.nlp.pipeline.StanfordCoreNLPServer"]
ENTRYPOINT java -cp "*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer
