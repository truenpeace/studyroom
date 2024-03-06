# API (Application Programming Interface)
기계와 기계 소프트웨어와 소프트웨어 사이에서도 수많은 요청과 정보 교환이 이루어지고 있다.<br>
이들 사이에서 소통할 창구가 필요하다. 예를들어 기상정보가 관리되는 기상청 서버가 있다.<br>
날씨 정보를 제공하는 다양한 웹 사이트나 앱들이 이 기상청 서버로부터 실시간으로 날씨 정보를 요청해서 받는다.<br>
그렇다면 이 기상청 서버에게 정보를 요청하는 지정된 형식이 있어야 한다.

`"data:191031|place:seoul|which:temperature"`

여기세 날짜, 지역, 조회할 내용을 작성해서 요청하면 답이 올거라는 공개된 메뉴얼이 있으면<br>
누구든 이걸 참조해서 기상청 정보를 활용할 수 있다.

이렇게 소프트웨어가 다른 소프트웨어로 부터 지정된 형식으로 요청, 명령을 받을 수 있는 수단을 API라고 한다.<br><br><br>

# REST (REpresentational State Transfer)
웹에 존재하는 모든 자원에 고유한 URI를 부여하여 자원에 대한 주소를 지정하는 **방법론**, **또는 규칙**이다.<br><br><br>

# RESTful API
RESTful API는 REST 특징을 지키면서 API를 제공한다는 의미 이다.
“프론트엔드에서 백엔드 API를 호출할 URL을 어떻게 만들것인가?”에 대한 이야기이다.
API 설계 규칙 가운데 가장 널리 사용되고 있는 규칙이다.
즉, 데이터를 주고받는데 있어서 개발자들 사이에 널리 쓰이는 일종의 형식이다.
기술이나 제품이 아니라 형식이기 때문에 어떤 프로그램 언어를 쓰든 무슨 프레임워크를 쓰든 폼에 맞춰서 기능들을 만들면 된다.
가장 중요한 특징은 각 요청이 어떤 동작이나 정보를 위한 것인지를 요청의 모습 자체로 추론이 가능하다는 것이다.


> 학원의 반 리스트 요청<br>
> http://(사이트 도메인)/1
> 
> 반의 학생들 리스트 요청<br>
> http://(사이트 도메인)/hello
> 
> 학생의 정보 수정 요청<br>
> http://(사이트 도메인)/hyot-hong

이런식으로 짜도 이에 맞춰 요청을 보내는 앱을 만들면 서비스의 기능 자체에는 문제가 없을 것이다. 그러나 문제는 서비스는 개발자 혼자 만드는 것이 아니라는 것이다. 이 일을 인계받는 다른 개발자나 이 API를 사용해서 다른 제품을 만드는 개발자들은 굉장히 일하기 힘들어 질 것이다.
때문에 RESTful API를 사용한다. 이는 요청의 주소만으로도 대략 이게 뭘 하는 요청인지 파악이 가능하다.
위와 같은 데이터가 있다
![image](https://github.com/truenpeace/studyroom/assets/107344551/b134b22f-8d03-4b82-b84a-361aed605294)
![image](https://github.com/truenpeace/studyroom/assets/107344551/657396fd-4d76-443a-bb16-eac6b76640ac)
![image](https://github.com/truenpeace/studyroom/assets/107344551/9113edab-ae78-4f0e-8da5-cbae5c0b418f)
![image](https://github.com/truenpeace/studyroom/assets/107344551/c3071acd-6fa6-43e1-9197-e86d0a02add2)

한페이지에 10명씩 받아오는 거라면 아래와 같이 표현할 수 있다.

![image](https://github.com/truenpeace/studyroom/assets/107344551/d091fc81-acf0-4e1f-a0cc-0e8fa0fab82b)

자원을 구조와 함께 나타내는 이런 형태의 구분자를 URI라고 한다.

이런 조회작업 뿐 아니라 정보는 넣거나 수정하거나 삭제하는 작업도 필요하다.
이를 통틀어서 CRUD라고 부른다.
> CREATE : 생성
> READ : 조회
> UPDATE : 수정
> DELETE : 삭제

서버에 REST API로 요청을 보낼때는 HTTP란 규약에 따라 신호를 전달한다.
> **H**yper**T**ext **T**ransfer **P**rotocol

REST API의 HTTP의 메소드는 5가지가 있다.
* GET : 데이터를 조회할 때 사용
* POST : 데이터를 추가할 때 사용
* DELETE : 데이터를 지울 때 사용
* PUT : 데이터를 변경할 때 사용 ⇒ 통채로 변경
* PATCH : 데이터를 변경할 때 사용 ⇒ 일부만 변경

이들을 목적에 따라 구분해서 사용해야 한다.

GET , DELETE보다 POST, PUT, PATCH가 정보는 더 많이 비교적 안전하게 감춰서 실어보낼 수 있다.
> GET , DELETE < POST, PUT, PATCH

<br><br><br>

# REST 규칙에 어긋나는 url
* url은 page 기준이 아닌 resource 기준으로 작성<br>
`[GET] http://127.0.0.1:8000/product/main_page_product`

* 메인 페이지에 표출될 정보가 무엇인지 판별하여 url을 정한다.
* 한번에 여러 종류의 정보를 표출한다면, 프론트엔드 개발자와 협의하여 REST에 맞춰 두가지 이상의 엔드포인트를 동시에 호출
* 우리 웹 서비스 메인페이지에 여름특가 + 사용자의 내 상세 정보<br>
`[GET] http://127.0.0.1:8000/store/find_store`

* 동사(find)를 사용하지 않는다.<br>
`[POST] [http://127.0.0.1:8000/product/add_first_item_information]`

* 자원을 추가(add)할 때는 ~/post 만으로 충분하다.<br>
`[GET] [http://127.0.0.1:8000/store](http://127.0.0.1:8000/store/search_store)?name='강남'`

* 검색 기능은 자원의 정보를 호출하는 기능이므로 [GET] method를 사용한다.
* 검색 키워드는 body를 통해 전달하지 않고, query string을 활용한다.<br><br><br>

# Query Parameters (GET parameters)
웹 페이지의 url 주소를 보면 종종 `?` 가 포함되어 있는 것을 봤을 것이다.<br>
`?` 뒤에는 `key=value` 형식의 문자열이 따라온다. 이를 **Query parameter**라고 부른다.
주로 아래와 같은 경우에 활용한다.
* filtering : 데이터를 조건으로 거르기
* sorting : 특정 방식으로 정렬
* searching : 검색

`/users?id=123`

ID가 123인 사용자를 가져온다.<br><br><br>

# Path Parameters
해당 리소스에 더 자세한 정보를 얻기 위해 접근할 때 사용한다.

`/users/123`

ID가 123인 사용자를 가져온다.<br><br><br>

# Path Variable와 Query Parameter는 언제 사용할까?
리소스를 식별하려면 **Path Variable**를 사용해야 한다. 그러나 항목을 정렬하거나 필터링하려면 **Query Parameter**를 사용해야 한다.
예를 들어 다음과 같이 정의할 수 있다.
> /users 👉🏻 사용자 목록 가져오기<br>
> /users?occupation=programer 👉🏻 프로그래머 목록 가져오기<br>
> /users/123 👉🏻 ID가 123인 사용자 가져오기

기본 CRUD 기능을 구현하기 위해 다른 URL 및 기타 **Query Parameter**를 정의할 필요없다. 수행하려는 작업에 따라 HTTP 메소드를 변경한다.
> /users [GET] 👉🏻 사용자 목록 가져오기<br>
> /users [POST] 👉🏻 새 사용자 생성<br>
> /users/123 [PUT] 👉🏻 사용자 업데이트<br>
> /users/123 [DELETE] 👉🏻 사용자 제거

---

> 출처: [https://velog.io/@remon/API-RESTful-API](https://velog.io/@remon/API-RESTful-API)
