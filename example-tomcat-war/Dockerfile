# Этап 1: Сборка проекта внутри контейнера
FROM ubuntu:18.04 AS builder

# Устанавливаем только необходимые для сборки Java и Maven
RUN apt update 
RUN apt install default-jdk -y
RUN apt install maven -y
RUN rm -rf /var/lib/apt/lists/*

# Создаем рабочую директорию внутри контейнера
WORKDIR /app

# Копируем ВСЕ файлы вашего проекта с компьютера в контейнер
# (Файлы pom.xml, папка src и т.д. должны лежать в той же папке, что и этот Dockerfile)
COPY . .

# Запускаем сборку приложения с помощью Maven
RUN mvn clean package -DskipTests


# Этап 2: Финальный контейнер с Tomcat 9
FROM tomcat:9.0-jdk11

# Очищаем стандартную папку приложений Tomcat
RUN rm -rf /usr/local/tomcat/webapps/*

# Копируем собранный war-файл из этапа builder
COPY --from=builder /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

# Открываем порт 8080
EXPOSE 8080

# Запускаем Tomcat
CMD ["catalina.sh", "run"]