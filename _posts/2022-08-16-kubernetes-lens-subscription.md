2023년 1월 2일 부터  Kubenetes 클러스터 모니터링을 관리 용으로 많이 사용하는 Lens (기존. opensource)가 유료화 된다고 한다.

2022년 8월 현재 기준 아직 여유는 있으나..  
kuberctl과 같은 cli를 이용해 접근/관리하던 클러스터를 IDE 형태로  
시각적으로 관리할 수 있어서 매우 유용하게 사용해 왔기에 관심있게 볼 수 밖에 없는 변경이다.



## Lens **subscription** 관련 공식 블로그 포스팅 요약

> 공식 블로그:   
> [Lens 6 Released, Vision for the Future, New Subscription Model and Features Available](https://medium.com/k8slens/lens-6-released-vision-for-the-future-new-subscription-model-and-features-available-628ff21fe14a)

- Lens Personal  
  개인 사용자, 교육 기관, 스타트업 사용자는 무료로 사용이 가능하다.  
  (스타트업 기준. 연 매출 1천 달러 미만)

- Lens Pro  
  그 외 기업 사용자는 유료 버전인 Lens Pro subscrition 을 구매해 사용해야 한다.  
  현 렌즈 데스크톱 사용자는 5년간 렌즈 프로 구독료 50%를 할인받을 수 있다.   
  (제한 기간 내에 신청해야 한다.)    

  - $19.90 per month  
  - $199 per year  
    ![subscribe](/assets/img/post/2022-08-16-kubernetes-lens-subscription/1.png)    
    ![discount](/assets/img/post/2022-08-16-kubernetes-lens-subscription/2.png)  
  
    [https://app.k8slens.dev/subscribe](https://app.k8slens.dev/subscribe)

- OpenLens (opensource)  
  다행스럽게도 opensource 버전은 유지한다고 한다. (MIT 라이선스)  
 
  

## 대안은?
- 터미널 기반의 관리 툴인 [K9s](https://k9scli.io/) 를 사용할 수도 있겠으나, 이미 Lens UI/UX에 길들여진 입장에서는 손이 잘 안 간다.
- OpenLens 를 사용하자!!    
  - Download Release Binary: [https://github.com/MuhammedKalkan/OpenLens/releases](https://github.com/MuhammedKalkan/OpenLens/releases)
  - Github: [https://github.com/lensapp/lens](https://github.com/lensapp/lens)
  - (etc) Lens 공식 홈페이지: [https://k8slens.dev](https://k8slens.dev)
