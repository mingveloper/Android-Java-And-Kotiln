
<h1>OkHttp3와 Retrofit2</h1>
<br/>  


1.Okttp3 
* Http통신을 간편하게하고 데이터 등을 교환하는 방법이며 구성에 도움을 주는 오픈 소스 라이브러리.
* execute()(동기 방식), enqueue()(비동기 방식) 방식을 각각 제공하여 개발자가 선택해서 개발 가능.
* 몇 줄의 코드만으로 REST API,HTTP의 요청, 응답을 처리 할수 있다.

<br/>  

2.Retrofit2

* Square사에서 만든 http 통신을 간편하게 해주는 라이브러리. 
* 기본적으로 OkHttp를 네트워크 계층으로 활용하여 구축된다.
* 구글 샘플 예제같은 경우를 보면 Retrofit2로 네트워크 통신을 보여주는 경우가 많음. 

* [AsyncTask나 Volley에 비해 응답속도가 빠르다.](http://instructure.github.io/blog/2013/12/09/volley-vs-retrofit/)

  
||One Discussion|Dashboard(7 requests)|25 Discussions|
|---|---|---|---|
|AsyncTask|941 ms|4,539 ms|13,957 ms|
|Volley|560 ms|2,202 ms|4,275 ms|
|Retrofit|312 ms|889 ms|1,059 ms|


<br/>  
<br/>  

<h2>OkHttp3 사용해보기</h2>

----



<br/>  

1. AndroidManifest.xml에 Internet 권한 추가하기.
```xml

<!-- 인터넷 권한 설정   -->
<uses-permission android:name="android.permission.INTERNET" />

```
2. gradle 앱수준에 라이브러리 추가하기.

```gradle

dependencies{
    implementation("com.squareup.okhttp3:okhttp:4.9.0")
    implementation("com.squareup.okhttp3:logging-interceptor:4.9.0")
}

```

3. 네트워크 통신은 Thread안에서 실행.

```kotlin
// OkHttpClient 객체를 생성
var client = OkHttpClient()

// client가 요청할 request를 생성
val request: Request = Request.Builder().url(통신할 Url).build()

// 요청을 실행한다.
// 동기방식처리 를 하고자 한다면 execute, 비동기 처리를 원한다면 enqueue를 사용한다.
client.newCall(request).enqueue()

// 요청 결과에 따른 작업을 위해 Callback을 달아준다.
client.newCall(request).enqueue(object : Callback {
            override fun onFailure(request: Request?, e: IOException?) {
                //실패한 경우
            }

            override fun onResponse(response: Response?) {
                //성공한 경우
                println(response?.body()?.string())
            }
        })
```

```kotlin
  // 요청할 작업을 body에 넣고 get/post 작업을 추가할 수 있다.
// ex ) "userId" 항목에 데이터 userId 을 넣어라!
val body = FormBody.Builder().add("userId",userId).build() as RequestBody

// request에 body를 post 해준다. (get도 가능)
val request : Request = Request.Builder().url(통신할Url).post(body).build()
```

<br/>  
  

<h2>Retrofit2 사용해보기</h2>

----

1. AndroidManifest.xml에 Internet 권한 추가하기.
```xml

<!-- 인터넷 권한 설정   -->
<uses-permission android:name="android.permission.INTERNET" />

```
<br/>  
2. gradle 앱수준에 라이브러리 추가하기.<br/>  
Retrofit은 자동적으로 JSON 응답을 사전에 정의된 POJO를 통해 직렬화 할 수 있다.<br/>   JSON을 직렬화 하기 위해서는 먼저 Gson converter가 필요하다.<br/>  

```gradle

dependencies{
    implementation 'com.squareup.retrofit2:retrofit:'Version''
    implementation 'com.google.code.gson:gson:'Version''
    implementation 'com.squareup.retrofit2:converter-gson:'Version''
}

```
<br/>  

**OkHttp는 이미 Retrofit2 모듈의 종속성에 포함되어 있어,<br/>   별도의 OkHttp 설정이 필요하다면 다음과 같이 Retrofit2에서 OkHttp 종속성을 제외해야 한다.**
<br/>  

```gradle

dependencies{
    implementation('com.squareup.retrofit2:retrofit:Version') {
        exclude module: 'okhttp'
}
    implementation 'com.google.code.gson:gson:'Version''
    implementation 'com.squareup.retrofit2:converter-gson:'Version''
    implementation 'com.squareup.okhttp3:okhttp:'Version''
    implementation 'com.squareup.okhttp3:logging-interceptor:'Version''
}

```


<br/>  
<br/>  
<h2>Reference<h2>

----

* [OkHttp](https://square.github.io/okhttp/)

* [INSTRUCTURE TECH BLOG](http://instructure.github.io/blog/2013/12/09/volley-vs-retrofit/)

* https://jade314.tistory.com/entry/Android-Library-OKHttp-http