<p align="center"><img width='300px' src="https://user-images.githubusercontent.com/24623403/195018411-a7a93794-d079-4311-977e-f61a5970a415.png"></p>
<p align='center'>
  <a href="https://github.com/Endless-Creation-32nd/javascript-deep-dive/issues"><img src='https://img.shields.io/github/issues/Endless-Creation-32nd/javascript-deep-dive'></a>
  <a href="https://github.com/Endless-Creation-32nd/javascript-deep-dive/pulls"><img src='https://img.shields.io/github/issues-pr/Endless-Creation-32nd/javascript-deep-dive'></a>
  <a href="https://github.com/Endless-Creation-32nd/javascript-deep-dive/graphs/contributors"><img src='https://img.shields.io/github/contributors/Endless-Creation-32nd/javascript-deep-dive'></a>
</p>

# javascript-deep-dive

도서&lt;모던 자바스크립트 Deep Dive> 스터디 저장소 입니다.

<br>

## 스터디 방식

- 브랜치는 자신의 이름을 딴 브랜치로 만들고, 주마다 자신이 맡은 장 이름으로 PR을 미리 연다
- 브랜치명(자기 이름)
  - inseo
  - jeongjin
  - sangjun
  - hyorin
- PR에서 **주마다 궁금했던 점, 더 알고싶었던 점을 코멘트로 작성**하고 스터디를 하며 내용을 최종 검토한 뒤 main에 merge한다.
- **위 과정을 반복한다.**

<br>

## 인덱스

<table>
  <thead>
    <tr>
      <th>주차</th>
      <th>내용</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="4">1주차 변수</td>
      <td><a href="/04장-변수/README.md">04장: 변수</a></td>
    </tr>
    <tr><td><a href="/13장-스코프/README.md">13장: 스코프</a></td></tr>
    <tr><td><a href="/14장-전역_변수의_문제점/README.md">14장: 전역 변수의 문제점</a></td></tr>
    <tr><td><a href="/15장-let,const키워드와_블록_레벨_스코프/README.md">15장: let, const 키워드와 블록 레벨 스코프</a></td></tr>
    <tr>
      <td rowspan="4">2주차 타입과 실행 컨텍스크</td>
      <td><a href="/06장-데이터타입/README.md">6장: 데이터 타입</a></td>
    </tr>
    <tr><td><a href="/09장-타입 변환과 단축평가/README.md">09장: 타입 변환과 단축 평가</a></td></tr>
    <tr><td><a href="/11장-원시 값과 객체의 비교/README.md">11장: 원시 값과 객체의 비교</a></td></tr>
    <tr><td><a href="/23장-실행_컨택스트/README.md">23장: 실행 컨텍스트</a></td></tr>
    <tr>
      <td rowspan="7">3주차 JavaScript 기초 함수</td>
      <td><a href="/05장-표현식과_문/README.md">5장: 표현식과 문</a></td>
    </tr>
    <tr><td><a href="/08장-제어문/README.md">08장: 제어문</a></td></tr>
    <tr><td><a href="/10장-객체 리터럴/README.md">10장: 객체 리터럴</a></td></tr>
    <tr><td><a href="/12장-함수/README.md">12장: 함수</a></td></tr>
    <tr><td><a href="/16장-프로퍼티_어트리뷰트/README.md">16장: 프로퍼티 어트리뷰트</a></td></tr>
    <tr><td><a href="/17장-생성자 함수에 의한 객체 생성/README.md">17장: 생성자 함수에 의한 객체 생성</a></td></tr>
    <tr><td><a href="/18장-함수와 일급 객체/README.md">18장: 함수와 일급 객체</a></td></tr>
    <tr>
      <td rowspan="4">4주차 빌트인 객체, strict mode, 프로토타입, this</td>
      <td><a href="/19장-프로토타입/README.md">19장: 프로토타입</a></td>
    </tr>
    <tr><td><a href="/20장-strict mode/README.md">20장: strict mode</a></td></tr>
    <tr><td><a href="/21장-빌트인객체/README.md">21장: 빌트인 객체</a></td></tr>
    <tr><td><a href="/22장-this/README.md">22장: this</a></td></tr>
    <tr>
      <td rowspan="3">5주차 클로저, 클래스, 함수</td>
      <td><a href="">24장: 클로저</a></td>
    </tr>
    <tr><td><a href="/25장-클래스/README2.md">25장: 클래스</a></td></tr>
    <tr><td><a href="">26장: ES6 함수의 추가 기능</a></td></tr>
  </tbody>
</table>

## 폴더 구조 및 컨벤션

- 폴더 구조
  ```
  📂 root
  - 📂 04장-변수 // n장-장이름
  	- 📃 README.md // README.md 파일에 정리합니다.
  - 📂 13장-스코프
  	- 📃 README.md
  - 📂 14장-전역_변수의_문제점 // n장-장이름(띄어쓰기는 _로 표시)
  	- 📃 README.md
  ...
  ```
- 커밋 컨벤션
  - 4장 : 변수 정리
  - {n}장 : {장\_이름} 정리
- PR 컨벤션
  - ex) [1주차: 변수] 14. 전역 변수의 문제점
  - ex) [1주차: 변수] 4. 변수
  - ex) [1주차: 변수] 13.스코프
  - ex) [8주차: 비동기 프로그래밍] 41. 타이머

<br>

## 👨‍👩‍👧‍👦 팀원

<table>
  <tr align="center">
    <td>오정진</td>
    <td>이상준</td>
    <td>이효린</td>
    <td>황인서</td>
  </tr>
  <tr>
    <td align="center">
        <a href="https://github.com/ojj1123"><img src="https://avatars.githubusercontent.com/u/33178048?v=4" width="150px" alt="오정진"/><br /></a>
     </td>
    <td align="center">
      <a href="https://github.com/Sangjun-man"><img src="https://avatars.githubusercontent.com/u/66112027?v=4" width="150px;" alt="이상준"/><br /></a><br />
      </td>
     <td align="center">
        <a href="https://github.com/hyorish03"><img src="https://avatars.githubusercontent.com/u/108210492?v=4" width="150px" alt="이효린"/><br /></a>
     </td>
     <td align="center">
        <a href="https://github.com/sjsjsj1246"><img src="https://avatars.githubusercontent.com/u/24623403?v=4" width="150px" alt="황인서"/><br /></a>
     </td>
  <tr>
</table>
