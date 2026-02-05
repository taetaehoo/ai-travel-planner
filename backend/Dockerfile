# 1단계: 빌드 스테이지
FROM gradle:8.10-jdk17 AS build
WORKDIR /app

# [중요] gradlew를 포함한 모든 프로젝트 파일을 복사
COPY . .

# [해결] 실행 권한 부여 및 경로 명시
RUN chmod +x ./gradlew
RUN ./gradlew clean build --no-daemon -x test

# 2단계: 실행 스테이지
FROM eclipse-temurin:17-jre
WORKDIR /app

# 빌드 결과물인 jar 파일 복사
# build/libs 폴더에 보통 2개의 jar가 생기므로, plain이 아닌 것을 가져오도록 필터링
COPY --from=build /app/build/libs/*[!-plain].jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]