---
layout: post

title: 2022 삼성 SDS 동계 대학생 알고리즘 특강 후기 (+ Pro test)

subtitle: 22.1.3 ~ 22.1.14

author: hanabzu

categories: 후기

tags: SDS Algorithm 코딩테스트 후기

---

<img src="https://user-images.githubusercontent.com/76643387/151699877-70cdf283-6e49-4fbf-9fde-fa39c596cac5.png" title="" alt="image" data-align="center">

### 1. 소개

4학년을 앞둔 겨울 방학 삼성 SDS 동계 알고리즘 특강에 입과하였다.   
2주간 진행되며 C++/Java로 나누어 뽑는데 테스트에 통과하고 나면 분반을 바꿀 수도 있는 듯 하다.  
(같이 입과한 친구가 2차에서 1차로 바꿨음.)   
지원은 간단하게 구글설문으로 진행되지만 여러 증명서를 요구하니 미리 준비해야 한다.

### 2. 입과 테스트

풀이 시간 제한 없는 알고리즘 5문제를 일주일동안 풀어서 제출하면 된다.   
각 문제당 두시간 정도 풀이하는 것이 권장된다고 적혀있어서 별로 어렵지 않겠거니하고 이틀남은 주말에 몰아서 풀었다가 힘들어 죽는 줄... (2번에서 상당히 골치 아팠다...)   
그래도 5문제 중 4문제를 풀었고 400/500점으로 최종 제출하였다. 5번은 손도 못댔음.   
백준 기준으로 5번 문제 외에는 **실버 상위 ~ 골드 중하위** 정도의 문제들이었던 것 같다. 
같이 지원해서 붙은 친구들을 보니 합격선이 높진 않았어서 적당히 부담없이 봐도 될 것 같다.  
아무래도 온라인으로 진행해서 인원수가 널널한 편이기도 한 듯.  
대략 1.5문제 이상 정도만 풀어도 붙는다고 보면 된다.   

<img title="" src="https://user-images.githubusercontent.com/76643387/151700777-66f3dd65-db86-4ff8-a82b-8b0687bbadd9.png" alt="image" width="593" data-align="center">

- 입과 테스트에 통과 후 받은 교재. 받자마자 조금 훑어봤는데 내용이 매우 맘에 듬.

### 3. 특강 진행

원래는 선릉역에 있는 멀티 캠퍼스에서 진행하는 것으로 알고 있는데 코로나여서 줌과 비스무리한 knox meeting 환경에서 오전 9시부터 오후 6시까지 수업을 들었다.   
수업 수료 조건은 출석률 80% 이상과 과정 종료 후 있을 SDS pro 검정에 반드시 참여하는 것이다.  
pro 시험 합격은 못하더라도 수료할 수 있다.   

실시간으로 진행되는 수업이다 보니 꽤 집중력이 필요했고 역시 나는 수업에 완전히 몰입하진 못했다... ㅠㅠ   
첫번째 주차는 **기본 알고리즘과 시간복잡도, 자료 구조부터 정수론 조합론**까지, 두번째 주차는 **그래프 이론과 다이나믹 프로그래밍**에 대해서 집중적으로 공부할 수 있었고 백준 그룹을 통해 매일 약 10문제 정도씩 풀어볼 만한 문제가 소개되어 있어서 연습해 볼 수 있었다.   
물론 하루에 다 풀 분량은 절대 아니고 난이도 또한 대부분 골드~플레 정도의 어려운 문제들로 이루어져 있다. 

강사님께서는 보통 오전 중엔 기본적인 개념에 대한 설명, 점심 이후에는 예제 문제들을 통해 직접 코드로 구현한 것들을 소개해주셨다.  
평소에 내가 짜던 코드보다 훨씬 깔끔한 구조와 표현들을 빠르게 익힐 수 있었고 또 혼자 공부하기 상당히 곤란했던 알고리즘에 대해 더 다가갈 수 있도록 여러 레퍼런스들을 소개해주셔서 큰 도움이 되었다. ([GeeksForGeeks](https://www.geeksforgeeks.org/) 라든지...)  

특히 첫주와 두번째 주에서 배우는 내용이 서로 다르고 강사님도 다른 분이셨음에도 불구하고 **Index tree**를 강조하셨는데 Segment tree를 사용하는 문제에 다 적용할 수 있고 개인적으로 세그트리보다 훨씬 이해하기도 쉽고 유용하다고 느껴졌다. 사실 세그트리 쓸 줄 모르지만... 

아무튼 수업 들을 때는 문제는 사실 거의 못 풀었고 대신 내가 지금까지 공부해왔던 알고리즘 내용에서 나사 빠져있는 이런저런 부분들을 메꾸는 기분으로 공부했다.   
수업 마지막 날에는 삼성 SDS pro 시험 팁? 에 대해서 소개를 해주셨고 인덱스 트리와 다익스트라를 한번 더 강조하셨다.

### 4. SDS Pro. 검정

수료 조건인 SDS pro 시험은 원래는 SDS 현직자들만 보는 시험으로 특강 입과자들에 한해서 **\*특별히\***  보게 해준다...   
그래도 어려운 시험이다보니 합격하면 파격적이게도 삼성 SDS 입사 지원 시 모든 전형을 건너뛰고 **임원 면접**만 보면 입사 가능! (현직자들도 한번에 붙는 경우는 많지 않다고는 한다.)   

시험은 선릉 멀티 캠퍼스에 직접 가서 보고, 아침 9시부터 4시간동안 한 문제를 풀면 된다.  
중요한 건 VS, 이클립스와 같은 IDE는 사용할 수 있는데 레퍼런스 열람이 안되므로 사용할 STL 문법 등은 모두 외워야 한다. 문제에 나와있는 테스트 케이스를 모두 맞춰도 실제 채점 시에는 히든 테케가 있고, 코드 리뷰까지 거쳐서 최종 결과를 낸다고 한다.   

사실 교육에서 배운 모든 내용을 소화하진 못했고 집이 아닌 환경에서 코딩 테스트를 보는 건 처음이라 상당히 떨리는 마음으로 봤는데 다행히 아는 알고리즘이 나와서 다 풀고나니 두시간 정도가 지나 바로 퇴실했다. 아무래도 한 문제다보니 운이 꽤 중요하다고 생각된다. 문제 난이도는 플레5 정도? 였던 것 같다.   

그러고는 시간이 남아 혼자 선릉과 정릉을 돌았다... 

<img title="" src="https://user-images.githubusercontent.com/76643387/151701706-e3d6544a-c20f-4683-8893-b0f034e752bc.png" alt="image" width="492" data-align="center">

* 무지 추웠다... 그래도 도심에 이런 공간이 있다니 신기해.

### 5. 결과

![image](https://user-images.githubusercontent.com/76643387/151701907-0a10eddc-9bcb-4a42-9743-12f6b3665626.png)

다른 리뷰들을 보니 당일 오후에 문자가 오는 경우가 많던데 나는 안 와서 매우 노심초사했으나 3일 후 합격 문자가 왔다!!   
이후 담당자 분이 전화로 전형에 대해서 소개해주셨다. 나는 아직 졸업이 1년이나 남아서 올해 하반기에 지원하면 된다고 한다... 또한 공인 영어 성적이 필요하단다.   
빨리 합격증도 실물로 받고 싶지만 파업 덕분에 택배가 오질 않고 있다... 언젠간 오겠지.   

---

- 03/07

드디어 왔다...! 😍

<img src="https://user-images.githubusercontent.com/76643387/156988270-01bbca55-76b2-47d3-99cc-6a86ead422a3.jpg" title="" alt="KakaoTalk_20220307_163800972" width="524">

### 6. 느낀 점

기간이 너무 길지도 않아 부담스럽지 않고 Pro test라는 좋은 기회까지 얹어주니 알고리즘 실력을 더 키우고 싶다면 매우 도움이 된다고 생각된다.   
나도 평소에 꾸준히 문제를 풀어왔었지만 이번 2주간의 교육으로 훨씬 벌크업된 기분이다.

그래도 만약 아예 코테에 대해 잘 모르거나 반대로 평소에 많이 공부를 해왔다면 굳이 들을 필요는 없겠다는 생각도 든다.
대략적으로 골드~플레 난이도를 다룬다. 첫날에 N-queen을 품.   
4학년부터 들을 수 있다고 되어있는데 취준으로 바쁜 4학년이 듣기에는 조금 부담스러울 수도 있어서 3학년 겨울방학에 듣길 잘한 것 같다.  

교육 과정은 모두 끝났지만 앞으로 수업에서 소개됐던 문제들을 **\*모두\*** 풀어버릴 생각이다.  
또한 인상적인 문제들은 블로그에 정리해서 남기려고 한다.   
(인상적이라기보다도 나를 심히 괴롭혔던 문제들...)
