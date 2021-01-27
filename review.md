cmd 창에 ipconfig 입력하면 

127.0.0.1 은 이름이 부여되어있음. localhost라고 해도 됨.

http://127.0.0.1:8000/welcom/

http://localhost:8000/welcome/  : ---> firstapp.views.welcome을 수행

http://localhost:8000/secondapp : ---> views.exam1 수행

http://localhost:8000/secondapp/exam2/  : ---> views.exam2 수행



HttpRequest : HTTP 프로토콜 기반으로 요청이 왔을 때, 요청 관련 정보를 제공하는 객체

(요청처리)		뷰함수가 호출될 때, 아규먼트로 전달된다. (장고서버가 객체를 생성한다.)

HttpResponse : HTTP 프로토콜 기반으로 온 요청에 대한 응답시 사용하는 객체 응답 내용을 담게 

(응답처리)	  	된다. (HTML 태그 문자열, 템플릿을 사용한 랜더객체)



`{% csrf_token %}`

페이지 소스 보기를 하면

`<input type="hidden" name="csrfmiddlewaretoken" value="dxVjO3t3DufJxtbp1ZYDRMyTskcGuyUScLbbhJUCvQQPbWzOIMwNAP5Rxru6Hs8a">` 

이번 요청이 정당한 방법이다 알리는 용도로 추가 됨.



**<실습>**

http://localhost:8000/workapp/exercise1/

![image-20210127134336657](C:\Users\jinsujeong\AppData\Roaming\Typora\typora-user-images\image-20210127134336657.png)

![image-20210127153610445](C:\Users\jinsujeong\AppData\Roaming\Typora\typora-user-images\image-20210127153610445.png)

301 : URL 끝에 / (슬래시) 안붙이면 다시 요청시킴. (결과는 나오지만, 내부적으로 통신이 한번 더 일어나게됨.)

200 : 성공

URL 문자열 뒤에 / 꼭 붙여줄 것!

http://localhost:8000/secondapp/exam2_1/

---------------------------------------------------------------URL 문자열

​                                     ---------------------------------URI (쿼리 문자열 포함)

HttpRequest.GET # GET 파라미터를 담고 있는 딕셔너리 같은 객체

HttpRequest.POST # POST 파라미터를 담고 있는 딕셔너리 같은 객체

`msg = request.GET.get("info1", "없음") + "-" + request.GET.get("info2", "없음") + "-" + request.GET.get("info3", "없음")`





[Query 문자열]

HTTP client 가 HTTP Server 요청시 서버에서 요청하려는 대상의 URI가 전달되는데 

이때 함께 전달될 수 있는 문자열이다.

- name = value 형식으로 구성되어야 한다.

- 여러 개의 name = value 가 사용될 때는 &  기호로 구분되게 구성해야 한다.

- 영문과 숫자는 그대로 전달되지만, 한글과 특수문자들은 % 기호와 16진수 코드값으로 전달된다. (UTF-8)

- 공백문자는 + 기호 또는 %20C로 전달 된다.

- Query 문자열을 가지고 HTTP Server에게 정보를 요청할 때는

  두 가지 요청 방식 중 한 개를 선택할 수 있다. 

  **GET** : Query 문자열이 외부에 보여진다. 요청 URL 뒤에 ?기호와 함께 전달되기 때문이다. Query 문자열의 길이 제한 있음.

  **POST** : Query 문자열이 외부에 보여지지 않는다. Query 문자열의 길이에 제한이 없다.

![image-20210127160940181](C:\Users\jinsujeong\AppData\Roaming\Typora\typora-user-images\image-20210127160940181.png)

https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=ABCabc+123+%EA%B0%80%EB%82%98%EB%8B%A4

한글 3바이트 씀. %EA%B0%80 : 가  %EB%82%98 : 나  %EB%8B%A4 : 다

**중요**

**post** : 쿼리 문자열이 외부로 보여지면 안될 때 내부로만 전달되게끔함. (주소필드를 나타내지 않음.) 반드시 폼 태그 써야함. 쿼리를 가지고 요청함.

특히, 로그인 정보 등등

**get** : 보여줘도 상관 없을 때  주소 필드에 나타남. , a태그는 get 방식 , 쿼리 문자열 없이 요청







http://localhost:8000/secondapp/exam5/

get 방식

![image-20210127170649251](C:\Users\jinsujeong\AppData\Roaming\Typora\typora-user-images\image-20210127170649251.png)

http://localhost:8000/secondapp/exam5/?name=%EC%A7%84%EC%88%98%EC%A0%95&address=%EA%B2%BD%EA%B8%B0%EB%8F%84

![image-20210127170846923](C:\Users\jinsujeong\AppData\Roaming\Typora\typora-user-images\image-20210127170846923.png)

http://localhost:8000/secondapp/exam5/?name=%EC%A7%84%EC%88%98%EC%A0%95

![image-20210127170955857](C:\Users\jinsujeong\AppData\Roaming\Typora\typora-user-images\image-20210127170955857.png)