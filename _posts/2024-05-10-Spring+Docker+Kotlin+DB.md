---
title: "Spring(MySQL) + Docker + Kotlin"
date: 2024-04-25 02:23:00 +0900
categories: [Sort]
tags: [CodingTest, Algorithm, Sort]
---
# 1. 프로젝트를 위한 실습
 프로젝트에서는 Server가 HardWare로부터 raw-data를 수신 받아 이상치를 검증하고 DB에 저장하는 시나리오 1과 Server가 DB로부터 정상적인 데이터를 가져와 Front에 Broadcast하는 시나리오 2로 간단하게 이루어져있다. 중간에 Admin은 Server에 송신하는 HardWare의 상태를 점검할 수 있도록 만들 것이다. 왜냐면 만약에 고객이 컴플레인이 들어오면 해당 장치에 무슨 이상이 있는지 단계별로 알아야하기 때문이다.

# 3.1. Host(Server) <- Docker(Client)
아직 하드웨어가 도착을 안해서 Docker에서 C 프로그램으로 Server에 Json 데이터를 보내는 Client 프로그램을 작성해서 하드웨어에서 서버로 보내는 신호를 대체 하였습니다.

<center>
<img src="https://github.com/cisco/openh264/assets/56510688/b626c744-3cd1-432e-8f53-9175ba789572" width="720" height=""/>
<p><b>[그림1]. 현재 실습 아키텍쳐</b></p>
</center>

# 2. Client 소스 코드

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <curl/curl.h>
#include <sys/wait.h>
#include <time.h> // time 라이브러리 추가

// JSON 데이터를 서버로 보내는 함수
void sendSensorData(const char *json_data) {
    CURL *curl;
    CURLcode res;

    // libcurl 초기화
    curl = curl_easy_init();
    if (curl) {
        // 요청을 보낼 서버 URL
        curl_easy_setopt(curl, CURLOPT_URL, "http://host.docker.internal:8080/sensorData");

        // POST 메서드 사용
        curl_easy_setopt(curl, CURLOPT_POST, 1L);

        // 전송할 데이터 설정
        curl_easy_setopt(curl, CURLOPT_POSTFIELDS, json_data);

        // Content-Type 설정 (JSON 형식)
        struct curl_slist *headers = NULL;
        headers = curl_slist_append(headers, "Content-Type: application/json");
        curl_easy_setopt(curl, CURLOPT_HTTPHEADER, headers);

        // 요청 보내기
        res = curl_easy_perform(curl);

        // 에러 처리
        if (res != CURLE_OK)
            fprintf(stderr, "curl_easy_perform() failed: %s\n", curl_easy_strerror(res));

        // libcurl 세션 종료
        curl_easy_cleanup(curl);

        // 헤더 리스트 정리
        curl_slist_free_all(headers);
    }
}

int main(void)
{
    int pipefd[2];
    pid_t pid;

    // 파이프 생성
    if (pipe(pipefd) == -1) {
        perror("pipe");
        exit(EXIT_FAILURE);
    }

    // 자식 프로세스 생성
    pid = fork();

    if (pid == -1) {
        perror("fork");
        exit(EXIT_FAILURE);
    } else if (pid == 0) { // 자식 프로세스
        close(pipefd[1]); // 자식 프로세스는 쓰기 파이프를 닫음

        // 파이프로부터 데이터 읽기 및 HTTP 요청 보내기
        char buffer[256];
        ssize_t bytes_read;
        while ((bytes_read = read(pipefd[0], buffer, sizeof(buffer))) > 0) {
            // JSON 형식으로 변환
            char json_data[512]; // 더 큰 크기로 수정
            // 현재 시간 구하기
            time_t rawtime;
            struct tm *timeinfo;
            time(&rawtime);
            timeinfo = localtime(&rawtime);
            // 날짜와 시간을 형식에 맞게 포맷팅
            char datetime[20];
            strftime(datetime, sizeof(datetime), "%Y-%m-%d %H:%M:%S", timeinfo);
            sprintf(json_data, "{\n\"SensorData\": \"%s\",\n\"date\": \"%s\",\n\"Type\": \"sensor_type\",\n\"ID\": \"sensor_id\",\n\"PORT\": 8080\n}", buffer, datetime);

            // libcurl을 사용하여 HTTP POST 요청 보내는 함수 호출
            sendSensorData(json_data);

            printf("Sending data: %s\n", json_data);
            sleep(1); // 1초 대기
        }

        close(pipefd[0]); // 파이프 닫기
        _exit(EXIT_SUCCESS);
    } else { // 부모 프로세스
        close(pipefd[0]); // 부모 프로세스는 읽기 파이프를 닫음

        // 부모 프로세스는 사용자가 Ctrl+C를 누를 때까지 계속해서 데이터를 전송
        while (1) {
            // 센서 데이터 생성
            char sensor_data[256];
            sprintf(sensor_data, "%ld", random() % 1000);

            // 파이프로 데이터 전송
            write(pipefd[1], sensor_data, sizeof(sensor_data));

            // 0.5초 대기
            usleep(500000); // 0.5초 대기
        }

        close(pipefd[1]); // 파이프 닫기
        wait(NULL); // 자식 프로세스가 종료되길 기다림
    }

    return 0;
}
```

### Client가 보내는 Json 형식

```json
{
    "SensorData": 1~1000 Random Value,
    "date" : YYYY%MM%DD-HH:MM:SS, 
    "sensor_type" : TYPE, 
    "sensor_id" : ID,
    "PORT": 8080
}
```


# 3. Server 프로그램

### Controller(receiveFromArudino)

```java
package com.WatchDogs.controller;

import com.WatchDogs.members.SensorData;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class receiveFromArudino {
    @PostMapping("/sensorData")
    public String receiveSensorData(@RequestBody SensorData sensorData) {
        // 클라이언트로부터 받은 센서 데이터 처리
        System.out.println("Received sensor data: " + sensorData);

        // 여기서 데이터 처리 로직을 수행하고 필요에 따라 응답을 반환할 수 있습니다.
        return "Received sensor data: " + sensorData.getSensorData() +
                ", date: " + sensorData.getDate() +
                ", sensor type: " + sensorData.getType() +
                ", PORT: " + sensorData.getPort();
    }
}
```

### Model(SensorData)

```java
package com.WatchDogs.members;

import com.fasterxml.jackson.annotation.JsonProperty;

public class SensorData {
    @JsonProperty("SensorData")
    private String sensorData;

    @JsonProperty("date")
    private String date;

    @JsonProperty("Type")
    private String type;

    @JsonProperty("ID")
    private String id;

    @JsonProperty("PORT")
    private int port;

    // Getters and Setters
    public String getSensorData() {
        return sensorData;
    }

    public void setSensorData(String sensorData) {
        this.sensorData = sensorData;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }

    public String getType() {
        return type;
    }

    public void setType(String type) {
        this.type = type;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public int getPort() {
        return port;
    }

    public void setPort(int port) {
        this.port = port;
    }

    // toString method override
    @Override
    public String toString() {
        return "SensorDataDTO{" +
                "sensorData='" + sensorData + '\'' +
                ", date='" + date + '\'' +
                ", type='" + type + '\'' +
                ", id='" + id + '\'' +
                ", port=" + port +
                '}';
    }
}
```

### index.html

```hmtl
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Received Data</title>
</head>
<body>
<h1>Received Data</h1>
<div id="receivedData"></div>

<script>
    // 서버에 데이터 요청
    fetch('/sensorData')
        .then(response => response.text())
        .then(data => {
            // 받은 데이터를 출력
            document.getElementById('receivedData').innerText = data;
        })
        .catch(error => console.error('Error:', error));
</script>
</body>
</html>
```

<center>
<img src="https://github.com/cisco/openh264/assets/56510688/542743eb-45a7-42d0-8176-9e23cb7be8b7" width="480" height=""/>
<p><b>[그림2]. Server 패키지 구조</b></p>
</center>





