# Dockerfile "multistage build" en 2 étapes :
# 1. la compilation du projet dans un conteneur "maven"
# 2. le déploiement du projet dans un conteneur "eclipse temurin"

# Etape 1 : compilation du projet
FROM maven:3.9.3-eclipse-temurin-17-alpine as builder
# paramétrage du dossier de travail
WORKDIR /opt/epnak-api
# copie uniquement du pom.xml dans le conteneur
COPY pom.xml .
# copie du code source
COPY ./src ./src
# compilation du projet (sans les tests) et création d'un .jar
RUN mvn clean install -DskipTests

# Etape 2 : conteneur en production
# utilisation d'une image recommandée pour sa légèreté
FROM eclipse-temurin:17-jre-alpine
EXPOSE 8000/tcp
WORKDIR /opt/<nom-projet>
COPY --from=builder /opt/<nom-projet>/target/<nom-projet>-0.0.1-SNAPSHOT.jar <nom-projet>-0.0.1-SNAPSHOT.jar
ENTRYPOINT ["java","-jar","/opt/<nom-projet>/<nom-projet>-0.0.1-SNAPSHOT.jar"]
