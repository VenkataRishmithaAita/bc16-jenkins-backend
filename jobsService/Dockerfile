FROM  adoptopenjdk/openjdk11
ARG JAR_FILE=/build/libs/jobsService-0.0.1-SNAPSHOT.jar
COPY ${JAR_FILE} app.jar
EXPOSE 8081
ENTRYPOINT ["java","-jar","app.jar"] 