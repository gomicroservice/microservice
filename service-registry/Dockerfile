FROM openjdk:8-alpine
ADD target/service-registry-0.0.1-SNAPSHOT.jar app.jar
RUN sh -c 'touch /app.jar'
ENV JAVA_OPTS=""
# http://www.thezonemanager.com/2015/07/whats-so-special-about-devurandom.html
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -Dspring.profiles.active=docker -Dlogging.path=/log/ -jar /app.jar" ]
EXPOSE 8761
VOLUME ["/log"]
