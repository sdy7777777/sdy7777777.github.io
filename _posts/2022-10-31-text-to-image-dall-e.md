---
layout: single
title: "[인공지능] DALL-E-2. 글자로 쓰면 그림을 그려 준다고? (Text to Image)"
#subtitle: ""
date: 2022-10-31 17:30:00 +0900
last_modified_at: 2022-10-31 17:30:00 +0900 # sitemap.xml 에서 사용 됨. 

categories: 인공지능
tags:
  - DALL-E-2, "Deep learning", 인공지능
#  - test
---

인공 지능이 성큼 우리 속으로 다가오고 있는 것 같다.  
기술적인 접근 보다도 재미 위주로 접근해 보기 좋은 서비스를 알게 되어 글로 남긴다.

바로. DALL-E-2!!!  

---

## DALL-E ??
- DALL-E는 Open AI에서 공개한 AI 모델로  
  자연어처리와 컴퓨터 비전을 결합하여 텍스트에서 이미지를 생성할 수 있다.
- 초현실주의 미술가 살바도르 달리(Salvador Dalí) + 픽사 애니메이션의 로봇 캐릭터 WALL-E의 이름을 합쳐서 지은 것 이라고 한다.
  ![SalvadorDali-walle](/assets/img/post/2022-10-31-text-to-image-dall-e/SalvadorDali-walle.png)  
- 주어진 텍스트로 이미지를 만들수 있다니 정말 재미있지 않나??
- [나무 위키: DALL-E](https://namu.wiki/w/DALL%C2%B7E)

```bash
살바도르 달리(Salvador Dalí)가 초현실주의 화가라서일까??  
DALL-E가 그려주는 그림들도 어쩐지 초현실주의 작품 같은 느낌을 주는 결과물이 많은 편이다.
```


## 어디서 체험 해 볼 수 있을까?

> [https://openai.com/dall-e-2/](https://openai.com/dall-e-2/)

Open AI의 DALL-E 페이지에서 체험 가능하다.  
(구글 계정으로 로그인 하여 사용)

하지만 무료는 아니다!!!
- 최초 가입시 무료로 주는 50 크레딧을 이용해 사용해 볼 수 있다.
- 크레딧은 추가 구매가 가능한데 $15 USD 에 115 크레딧을 주는 것 같다.
  (구매 해 보지는 않았다.)    
  ![credit1](/assets/img/post/2022-10-31-text-to-image-dall-e/dall-e-2-credit2.png)    


## 체험 해 보자.

> 입력어: 3D render of a DevOps engineer character who likes to eat milk and sweet potatoes.  
  

결과>  
![output](/assets/img/post/2022-10-31-text-to-image-dall-e/dall-e-2-output.png)  

아래 두 이미지는 맘에(?) 든다 ㅋㅎㅎㅎㅎㅎ  
<img src="/assets/img/post/2022-10-31-text-to-image-dall-e/dall-e-2-output1.png" width="50%" height="50%"/>  
<img src="/assets/img/post/2022-10-31-text-to-image-dall-e/dall-e-2-output1.png" width="50%" height="50%"/>  
<!--
![output1](/assets/img/post/2022-10-31-text-to-image-dall-e/dall-e-2-output1.png){: width="50%" height="50%"}  
![output2](/assets/img/post/2022-10-31-text-to-image-dall-e/dall-e-2-output2.png){: width="50%" height="50%"}   
-->

## 막간. 프롬프트 엔지니어링(Prompt Engineering)

우리가 AI 스피커로 이야기 하다보면 혹은 구글 등의 번역 서비스 사용시   
특정 패턴으로 이야기 할 경우 더 잘 이해하고, 더 잘 번역해 주는 경험들을 많이 한다.  

인공지능이 이해하기 쉽도록 입력값을 주는 분야를 **프롬프트 엔지니어링(Prompt Engineering)** 이라고 한다.

DALL-E 와 같은 최근에 거대한 데이터셋을 이용한 인공지능들에서(GPT-3를 활용한 OpenAI 서비스 등)  
인공지능에게 자연어로 명령어하고 결과값을 받는 형식이 많이 도입되고 있다.  
이에 따라 
- 인공지능이 자연어 명령의 의도를 더 잘 이해하고
- 우리가 원하는 결과를 더 잘 도출 할 수 있는도록 하기 위한  
**프롬프트 엔지니어링(Prompt Engineering)** 영역에 대한 관심도 높아지고 있다고..


