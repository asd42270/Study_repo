#### filter와 interceptor의 차이

##### filter란
![image](https://github.com/user-attachments/assets/4b87af39-b963-486c-b34a-12ab4ce88744)
- 이렇게 Spring Context 앞단에 위치하여 Dispatcher Servlet에 요청이 전달되기 전/후에 URL 패턴에 맞는 모든 요청에 대한
**부가 작업**을 처리할 수 있는 기능을 제공한다.
- 요청과 응답 객체에 대한 조작이 가능하다.
- Spring Context 외부에 즉, Web Context에 위치하여 관리된다!
- 그런데 Filter도 스프링 빈으로 등록이 가능하다.
- Filter는 전역적인 요청/응답 수정에 용이하다.

#### interceptor란
![image](https://github.com/user-attachments/assets/2481a525-f0e5-49a3-a2a7-4e62f366a2ad)
- Spring이 제공하는 기술이다.
- Dispatcher Servlet이 url에 맞는 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공한다.
- 요청이나 응답 객체를 조작할 수 없다.
- Spring Context 내부에서 동작한다.
- 핸들러 전후의 전처리/후처리에 적합하다.


#### 주목하자!
- Filter는 다음 필터로 넘겨줄 때 요청 객체와 응답 객체를 넘겨준다. 그 말은? 요청 객체와 응답 객체에 대한 직접적인 조작이 가능하다라는 것이다.
- Interceptor는 그냥 여러 인터셉터를 실행시킨다.


#### 필터와 인터셉터의 용도
##### 필터
- 공통된 보안 및 인증/인가 작업
- 모든 요청에 대한 로깅
- 이미지/데이터 압축 or 인코딩
##### 인터셉터
- 세부적인 보안 및 인증/인가 작업
- API 호출에 대한 로깅
- Controller로 넘겨주는 정보의 가공
