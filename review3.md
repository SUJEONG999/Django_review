

http://localhost:8000/secondapp/exam14/

bootstrap templates

**프로젝트 템플릿 참고**

https://startbootstrap.com/themes?showPro=false&showAngular=false&showVue=false 

https://themewagon.com/theme_tag/free/

static 태그 이용

static 태그에 의해서 자동으로 확장이 된다. 나머지 주소 images/olaf1.png만 쓰면 됨.

반드시 `{% load static %}` 먼저 쓰고 써야함.

```html
<img class="card-img-top" src="{% static 'images/olaf1.png' %}" alt="Card image" style="width:100%">
```

혹은,

하드코딩

```html
<img class="card-img-top" src="/static/images/olaf1.png" alt="Card image" style="width:100%">
```

static 은 정적 자원을 요청해왔을때, 전달. css, javascript, image 등등

장고에서는 html은 템플릿이고, 동적자원으로 사용.



```python
path('exam14/<word>/<int:num1>/<num2>', views.exam14, name='exam14'),
path('exam15/', views.exam15, name='exam15'),
path('exam16/', views.exam16, name='unico'),
```

path에서 name 속성은 선택적으로 작성할 수 있으며, 어떤 name 속성을 주든 상관없고, unique하면 됨.



### [exam15 + exam16]

exam15.html

```html
{% extends 'base.html' %}

{% block content %}
  <div style="text-align:center">
  <h1 >이름이 'unico'인 다른 장고뷰를 요청해보자!! </h1>
  <hr>
  <form action="{% url 'unico' %}" method="GET">
    <input type="text" name="message">
    <input type="submit">
  </form>
  </div>
{% endblock %}
```

url에 뷰이름(name 속성의 값) unico로 정의된 거 실행 시켜줘.

```<form action="/secondapp/exam16/" method="GET">
<form action="/secondapp/exam16/" method="GET">
```

같은 의미임.



exam16.html

```html
{% extends 'base.html' %}

{% block content %}
  <div class="container p-3 my-3 bg-primary text-white">
  <h1>'unico'라는 이름으로 알려져 있는 exam16.html 입니다. 잘 부탁해용!!!</h1>
  <h2>제가 받은 데이터는 {{ message }} 입니다.</h2>
  {% for msg in msg_list%}
    <h2>{{ msg }}</h2>
  {% endfor %}
    </div>
{% endblock %}
```

`<div class="container p-3 my-3 bg-primary text-white">` 는 파란색 박스에 하얀글씨로 표현

views.py

```python
def exam15(request):
    return render(request, 'exam15.html')


def exam16(request):
    print(request.GET.get('message'))
    msg_list = ['안녕', '방가방가', '하이']
    message = request.GET.get('message')
    context = {
        'message': message,
        'msg_list': msg_list,
    }
    return render(request, 'exam16.html', context)
```



`print(request.GET.get('message'))` 파이썬 콘솔 창에 메세지 보여줌



![image-20210129095009680](/images/image-20210129095009680.png)



![image-20210129095035907](/images/image-20210129095035907.png)

### [exam17 + exam18]

views.py

```python
def exam17(request):
    return render(request, 'exam17.html')


def exam18(request):
    name = request.GET.get('name')
    numbers = range(1, 46) # 1부터 45까지
    pick = sorted(random.sample(numbers, 6))
    context = {
        'name': name,
        'pick': pick,
    }
    return render(request, 'exam18.html', context)
```

exam17.html

```html
{% extends 'basesimple.html' %}

{% block mycontent %}
  <h1>소스 코드를 확인해 주세요...</h1>
  <form action="{% url 'exam18' %}" method="GET">
    이름 <input type="text" name="name">
    <input type="submit">
  </form>
{% endblock %}
```

exam18.html

```html
{% extends 'basesimple.html' %}

{% block mycontent %}
  <h1>{{ name }}님이 뽑은 번호는 {{ pick }}입니다.</h1>
{% endblock %}
```

![image-20210129095145502](/images\image-20210129095145502.png)

![image-20210129094754554](/images\image-20210129094754554.png)



### [exam19]

```python
path('exam19/', views.exam19, name='exam19'),
```

exam19.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>지금은 언제일까?</h1>
<hr>
<ul>
    <li>{{ current_date }}</li>
    <li>{{ current_date|date }}</li>
    <li>{{ current_date|date:"Y년 m월 d일 H시 i분 s초" }}</li>
    <li>{{ current_date|time }}</li>
    <li>{{ current_date|time:" H시 i분 s초" }}</li>
</ul>
</body>
</html>
```



```python
def exam19(request) :
    dt = datetime.now()
    context = {'current_date' : dt}
    return render(request, 'exam19.html', context)
```

![image-20210129100235116](/images/image-20210129100235116.png)

urls.py

```python
path('product1/', views.product1, name='product1'),
path('basket1/', views.basket1, name='basket1'),
path('product2/', views.product2, name='product2'),
```



```python
def product1(request) :
    return render(request, "product1.html", {'datas': [ x for x in range(1,11)]})

def basket1(request) :
    pid = request.GET.get("pid")
    context = { 'pid' : pid, 'count' : 1 }
    return render(request, "basket1.html", context)

def product2(request) :
    return render(request, "product2.html", None)
```



사용자의 편의성은 product2가 더 좋음

```html
{% extends 'basesimple.html' %}

{% block mycss %} <!-- 스타일 태그에 들어갈 css내용만 작성 -->
img {
   width: 150px;
   height: 150px;
   border: 1px solid black;
   padding: 10px;
   box-shadow: 10px 10px 5px gray;
   margin: 10px;
}
section {
   text-align: center;
}
{% endblock %}

{% block mycontent %}
<h2 style="text-align:center">원하는 상품을 클릭해 주세요!!!</h2>
<hr>
{% load static %}
<section>
   {% for data in datas %}
      {% if data != 10 %}
         <a href="/secondapp/basket1?pid=p00{{data}}"><img src='/static/images/{{data}}.jpg'></a>
      {% else %}
         <a href="{% url 'basket1' %}?pid=p0{{data}}"><img src='/static/images/{{data}}.jpg'></a>
      {% endif %}
       {% if data == 5 %}
         <br>
      {% endif %}
   {% endfor %}
</section>
{% endblock %}
```



```html
{% extends 'basesimple.html' %}

{% block mycontent %}
<h3>{{ pid }} 상품을 {{ count }}개 선택하셨군요...</h3>
<a href="{% url 'product1' %}">상품 선택 화면으로</a>
{% endblock %}
```

url태그 사용해서 정의해놓은 이름으로 요청해도 되고,  

`<a href="/secondapp/product1/">상품 선택 화면으로</a>` 으로 해도 됨.



```html
{% extends 'basesimple.html' %}

{% block mycss %}
img {
   width: 150px;
   height: 150px;
   border: 1px solid black;
   padding: 10px;
   box-shadow: 10px 10px 5px gray;
   margin: 10px;
}
section {
   text-align: center;
}
{% endblock %}

{% block mycontent %}
<h2 style="text-align:center">원하는 상품을 클릭해 주세요!!</h2>
<hr>
{% load static %}
<section>
   <img src="{% static 'images/1.jpg' %}">
   <img src="{% static 'images/2.jpg' %}">
   <img src="{% static 'images/3.jpg' %}">
   <img src="{% static 'images/4.jpg' %}">
   <img src="{% static 'images/5.jpg' %}"><br>
   <img src="{% static 'images/6.jpg' %}">
   <img src="{% static 'images/7.jpg' %}">
   <img src="{% static 'images/8.jpg' %}">
   <img src="{% static 'images/9.jpg' %}">
   <img src="{% static 'images/10.jpg' %}">
</section>
<p style="text-align:center"><output></output></p>  <!--비어있는 output생성(서버로부터 가져온 내용(aJAX결과)을 현재 페이지에 보여줌)-->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
    $('img').each(function(index, dom) {
      $(dom).click(function() {
         if (index < 10)
            pid = 'p00'+(index+1);
         else
            pid = 'p0'+(index+1);
         $.getJSON("{% url 'basket2' %}?pid="+pid, function(jsonObj) {
            $("output").html("<h3>"+jsonObj.pid+"상품을 "+ jsonObj.count+"개 선택하셨네요<h3>");
         });
      });
    });

</script>
{% endblock %}
```



$('CSS선택자').each(함수) : css선택자를 찾아서 각각 함수를 한번씩 호출하시오.

$.each(자바스크립트객체, 함수)



```python
def product2(request) :
    return render(request, "product2.html", None)

def basket2(request) :
    pid = request.GET.get("pid")
    jsonContent = {"pid": pid, "count": 1}
    return JsonResponse(jsonContent)
```



# [AJAX 프로그래밍 핵심 내용]

1.  JSON(XML) 형식으로 응답하는 뷰를 준비해야한다. 

   (템플릿을 거치지 않고 뷰가 직접 응답)

2. JavaScript만 사용해서 **AJAX 요청**을 구현하는 방법

   ​	var **xhr** = new XMLHttpRequest()

   ​	xhr.onload = 함수

   ​	xhr.open("GET", 대상URL, true)

   ​	xhr.send()

   jQuery API를 사용하여 AJAX 요청을 구현하는 방법

   ​	$.getJSON("대상URL", 함수)

   ​	$.ajax({.......})



```python
path('json1/', views.json1, name='json1'),
path('json2/', views.json2, name='json2'),
path('json3/', views.json3, name='json3'),
```

템플릿이 따로 없음.



```python
def json1(request):
    return JsonResponse({
        'message' : '안녕 파이썬 장고',
        'items' : ['가나다', '파이썬', '장고', '자바스크립트', 'CSS3'], # 배열(리스트)
        'num' : 100
    }, json_dumps_params={'ensure_ascii':False})
```

생성하고자하는 딕셔너리 객체, json_dumps_params={'ensure_ascii':False}) :한글이 들어있어서 설정해주어야함.(한글자라도 한글들어가면.. 영어로만 되어있으면 안해주어도 됨.)

http://localhost:8000/secondapp/json1/

![image-20210129111457131](/images/image-20210129111457131.png)

딕셔너리로 만들어줌.

```python
def json2(request):
    data = [{'name': 'Peter', 'email': 'peter@example.org'},
            {'name': 'Julia', 'email': 'julia@example.org'}]
    return JsonResponse(data, safe=False)
```

여러덩어리의 json 객체이면 **safe=False** 설정해주어야함.

http://localhost:8000/secondapp/json2/

![image-20210129111726314](/images/image-20210129111726314.png)



```python
def json3(request):
    return JsonResponse({ "name" : "자바스크립트", "age" : 26, "kind" : "웹앱개발 전용 OOP 언어"},
                        json_dumps_params={'ensure_ascii':False})
```

http://localhost:8000/secondapp/json3/

![image-20210129112050592](/images/image-20210129112050592.png)



### [json3+exam20.html]

http://localhost:8000/secondapp/exam20/

![image-20210129112317970](/images/image-20210129112317970.png)

```python
def exam20(request):
    return render(request, 'exam20.html')
```



```html
{% extends 'base.html' %}

{% block content %}
  <div class="jumbotron text-center">
    <h1>Django+AJAX</h1>
  <h4>AJAX  기술을 이용해서 Django 서버 프로그램을 요청합니다.</h4>
  </div>
<div class="card-deck">
  <div class="card bg-primary">
    <div class="card-body text-center">
      <p class="card-text">클릭해 보셔용</p>
    </div>
  </div>
  <div class="card bg-warning">
    <div class="card-body text-center">
      <p class="card-text">클릭해 보셔용</p>
    </div>
  </div>
  <div class="card bg-success">
    <div class="card-body text-center">
      <p class="card-text">클릭해 보셔용</p>
    </div>
  </div>
  <div class="card bg-danger">
    <div class="card-body text-center">
      <p class="card-text">클릭해 보셔용</p>
    </div>
  </div>
</div>
<script>
$(document).ready(function () {
  $('p').click(function(e) {
   	$.getJSON('/secondapp/json3/',  function(data) {
   	      content = "";
   	      console.log(data);
		  $.each(data, function(key, value) {
		        console.log("key : "+key + " value : " +value);
		        content += value + "<br>";
		  });
		  $(e.target).html(content);
	});
  });
});
</script>
{% endblock %}
```



http://localhost:8000/secondapp/exam21/

```html
<script src="http://code.jquery.com/jquery-3.5.1.min.js"></script>
<script>
$(document).ready(function() {
   $.ajax({
        type : 'get',
        url : 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=75474bdfc6c0a4eb738939dd66c101b5&targetDt=20210127',
        dataType : 'json',
        error: function(xhr, status, error){
            alert(error);
        },
        success : function(json){
            console.log(json)
            $('div').html(JSON.stringify(json))
            console.log(json.boxOfficeResult.dailyBoxOfficeList[0].movieNm)
        }
    });
});
</script>
```

아래 방법 결과도 같음

```html
<script>
$(document).ready(function() {
   $.getJSON('http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=75474bdfc6c0a4eb738939dd66c101b5&targetDt=20210127',
        function(json){
            console.log(json)
            $('div').html(JSON.stringify(json))
            console.log(json.boxOfficeResult.dailyBoxOfficeList[0].movieNm)
        }
    );
});
</script>
```

http://jsonviewer.stack.hu/



http://localhost:8000/secondapp/exam22/

![image-20210129131641703](/images/image-20210129131641703.png)

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
 <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.0/js/bootstrap.min.js"></script>
<script>
$(document).ready(function() {
   $.ajax({
        type : 'get',
        url : 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=75474bdfc6c0a4eb738939dd66c101b5&targetDt=20210128',
        dataType : 'json',
        error: function(xhr, status, error){
            alert(error);
        },
        success : function(json){
            console.log(json)
            $('div:last').html(JSON.stringify(json))
            console.log(json.boxOfficeResult.dailyBoxOfficeList[0].movieNm)
        }
    });
});
</script>
</head>
<body>
<div class="jumbotron text-center">
   <h1>일별 박스 오피스</h1>
  <p>영화관 입장권 통합 전산망 오픈  API 를 AJAX 기술로 활용합니다.</p>
</div>
    <hr>
    <div></div>
</body>
</html>
```

값을 고정시켜서 작성 : **하드 코딩** .. 딱 그날짜의 정보를 계속 가져옴

http://localhost:8000/secondapp/exam23/

![image-20210129131845073](/images/image-20210129131845073.png)



