FROM maven AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests=true

FROM openjdk:17-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8180
CMD ["java","-jar","app.jar"]