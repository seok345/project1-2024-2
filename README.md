# Project1-2024-2
Ai_Software_Project1_2024_2
2024-09-12
오늘은 날씨도 별로라 학교 재낄까 하다 이교수님 수업 들으러 왔는데 좋았다.


# openweathermap
지정된 장소의 현재 날씨를 표시<br>
[OpenWeatherMap실습해보기]("https://openweathermap.org/themes/openweathermap/assets/vendor/owm/img/widgets/")
```
$.ajax({
			type: "GET",
			url: 'https://api.openweathermap.org/data/2.5/weather?q=London&units=metric&appid=7d96bc5108f52b80e2d9075a369b9f35',
		}).done(function(response) {
            console.log(response)
            // alert(response.weather.main[0])

            let wdata = response
            let exdata = response.weather[0];re
        
            temp.innerText = wdata.main.temp + "°C";
            min.innerText = wdata.main.temp_min;
            max.innerText = wdata.main.temp_max;
            wind.innerText = wdata.wind.speed;
        
            weather.innerText = exdata.main + "," + exdata.description;
            icon.setAttribute('src', icon_url + exdata.icon + ".png");
		}).fail(function(error) {
			alert("!/js/user.js에서 에러발생: " + error.statusText);
		});

```
# OPEN AI
[OPEN AI 실습해보기](https://platform.openai.com/docs/overview)
```
$.ajax({
        type: "POST",
        url: "https://api.openai.com/v1/chat/completions",
        headers: {
            "Authorization": "Bearer " + OPENAPI_KEY
        },
        data: JSON.stringify(data),
        contentType: "application/json; charset=utf-8"
    }).done(function (response) {
        console.log(response)
        //alert(response.choices[0].message.content)
        txtOut.value = (response.choices[0].message.content)
    }).fail(function (error) {
        console.log(error)
        errormsg = error.status + " : " + error.responseJSON.error.code + " - " + error.responseJSON.error.message
        txtOut.value = errormsg
    }
```
---------
```
    )
$.ajax({
        type: "POST",
        url: "https://api.openai.com/v1/images/generations",
        headers: {
            "Authorization": "Bearer " + OPENAPI_KEY
        },
        data: JSON.stringify(data),
        contentType: "application/json; charset=utf-8"
    }).done(function (response) {
        console.log(response)
        //alert(response.choices[0].message.content)
        gimage.src = (response.data[0].url)
        gimage2.src = (response.data[1].url)

    }).fail(function (error) {
        console.log(error)
        errormsg = error.status + " : " + error.responseJSON.error.code + " - " + error.responseJSON.error.message
        txtOut.value = errormsg
    }
    )
```
# google cloud vision
[OpenWeatherMap실습해보기]("https://cloud.google.com/vision?hl=ko")
```
$.ajax({
        type: "POST",
        url: CV_URL,
        headers: {
            "Accept": "application/json",
            "Content-Type": "application/json"
        },
        data: JSON.stringify(data), 
        contentType: "application/json; charset=utf-8"
    }).done(function(response) {
        let resultText = ''; 

        if (response.responses[0].faceAnnotations) {
            const faceAnnotations = response.responses[0].faceAnnotations;
            
            faceAnnotations.forEach((face, index) => {
                const joyLikelihood = face.joyLikelihood;
                const sorrowLikelihood = face.sorrowLikelihood;
                const blurredLikelihood = face.blurredLikelihood;

                let emotionStatus = `사진 속 ${index + 1}번째 사람은 : `;

                if (blurredLikelihood === "VERY_LIKELY" || blurredLikelihood === "LIKELY") {
                    emotionStatus += "얼굴이 너무 흐릿합니다.";
                } else if (joyLikelihood === "VERY_LIKELY" || joyLikelihood === "LIKELY") {
                    emotionStatus += "웃는 표정을 짓고 있습니다.";
                } else if (sorrowLikelihood === "VERY_LIKELY" || sorrowLikelihood === "LIKELY") {
                    emotionStatus += "우는 표정을 짓고 있습니다.";
                } else {
                    emotionStatus += "무표정 입니다.";
                }

                
                resultText += emotionStatus + '\n';
            });
        } else {
            resultText = "얼굴이 감지되지 않았습니다.";
        }
        
        
        document.getElementById("resultArea").value = resultText;

    }).fail(function(error) {
        console.log("이미지를 분석할 수 없습니다.");
        document.getElementById("resultArea").value = "Error: 이미지를 분석할 수 없습니다.";
    });

```


개발순서
1. 소스수정
2. 소스저장
3. 스테이지
4. 커밋 앤 푸쉬
5. 커밋 메시지

2024-09-19 깃허브 연동 실습
로컬에서 편집함.

2024-10-19 
CapStoneProject 중간과제 제출
이 웹사이트의 기능 :
1. 이미지 업로드 기능 : 사용자는 웹페이지에서 파일 업로드 버튼을 클릭하여 이미지를 업로드할 수 있습니다.
이 이미지는 Base64 형식으로 변환되어 서버로 전송되기 전에 텍스트 형식으로 변환됩니다. 

2. Google Vision API가 이미지 분석 결과를 반환하면, 반환된 데이터에는 이미지 속 사람들의 얼굴 정보와 감정 상태(웃는 표정, 우는 표정, 흐릿한 얼굴 등)가 포함됩니다.
감지된 얼굴이 여러 명일 경우, 각각의 얼굴에 대해 "무표정", "웃는 표정", "우는 표정", "얼굴이 흐릿함" 등의 감정 상태가 표시됩니다.

3. css를 사용해 깔끔한 디자인을 뽑아내었습니다.

1. 텍스트 기반 시스템(우리가 Json형태를 사용함.)과 호환성이 좋은 Base64 형식을 사용.
2. response.responses[0].faceAnnotations 50번쨰 줄에 있는 이코드가 로그창의 faceAnnotations
정보를 가져오는 코드이다.
3. 감정 분석 처리:
얼굴 감지 결과에 포함된 joyLikelihood, sorrowLikelihood, blurredLikelihood 값을 확인하여 
웃는 표정(joyLikelihood), 우는 표정(sorrowLikelihood), 또는 얼굴이 가려져 있거나 흐릿한 경우(blurredLikelihood)를 분석합니다.
4. i문을 접목시켜서 blurredLikelihood부터 시작해 VERY_LIKELY OR LIKELY부분이 떠있다면 출력 아니면 다음 
else if문으로 가서 다른 감정들이 해당하는지 확인하게 코딩했습니다.
5. 그 후 마지막 결과를 textArea부분에 출력되게 했습니다.
+Css