# Dockerfile "multistage build" en 2 étapes :
# 1. la compilation du projet dans un conteneur "maven"
# 2. le déploiement du projet dans un conteneur "eclipse temurin"

# Etape 1 : compilation du projet
FROM maven:eclipse-temurin as builder
# paramétrage du dossier de travail
WORKDIR /opt/tripwizu-api
# copie uniquement du pom.xml dans le conteneur
COPY pom.xml .
# copie du code source
COPY ./src ./src
# compilation du projet (sans les tests) et création d'un .jar
RUN mvn clean install -DskipTests

# Etape 2 : conteneur en production
# utilisation d'une image recommandée pour sa légèreté
FROM eclipse-temurin:21-jre-alpine
EXPOSE 8000/tcp
WORKDIR /opt/tripwizu-api
COPY --from=builder /opt/tripwizu-api/target/tripwizu-0.0.1-SNAPSHOT.jar tripwizu-0.0.1-SNAPSHOT.jar
ENTRYPOINT ["java","-jar","/opt/tripwizu-api/tripwizu-0.0.1-SNAPSHOT.jar"]
