# ÉTAPE 1: Construction (utilise une image avec Gradle et le JDK pour compiler)
FROM gradle:8.5.0-jdk21 AS builder
WORKDIR /app
# Copie les fichiers de build pour télécharger les dépendances
COPY build.gradle.kts settings.gradle.kts gradlew /app/
COPY gradle /app/gradle
# Télécharge les dépendances
RUN ./gradlew dependencies
# Copie le reste du code source
COPY src /app/src
# Construit l'exécutable (bootJar crée un JAR auto-exécutable)
RUN ./gradlew bootJar

# ÉTAPE 2: Production (utilise une image très légère avec seulement le JRE pour l'exécution)
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Copie le JAR construit de l'étape "builder"
COPY --from=builder /app/build/libs/*.jar app.jar

# Rend l'application compatible avec les variables d'environnement du Cloud (comme le port de Render)
ENV SERVER_PORT=8080

# Exposer le port de l'application
EXPOSE 8080

# Commande de démarrage
ENTRYPOINT ["java", "-jar", "app.jar"]