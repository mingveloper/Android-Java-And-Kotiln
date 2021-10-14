<h1>OkHttp3と Retrofit2</h1>
<br/>


1.Okttp3
* Http通信を簡単にデータなどを交換する方法であり、構成に役立つオープンソースライブラリ。
* execute()(同期 方式)、enqueue()(非同期 方式)方式をそれぞれ提供し、開発者が選択して開発可能。
* 数行のコードだけでREST API、HTTP の要請、応答を処理できる。

<br/>

2.Retrofit2

* Square社で作ったhttp通信を簡単にしてくれるライブラリ。
* 基本的にOKHttpをネットワーク階層として活用して構築される。
* Googleサンプル例題のような場合を見ると、Retrofit2でネットワーク通信を表示する場合が多い。

* [AsyncTaskやVolleyに比べて応答速度が速い。](http:/instructure.github.io/blog/2013/12/09/volley-vs-retrofit/)


||One Discussion|Dashboard(7 requests)|25 Discussions|
|---|---|---|---|
|AsyncTask|941 ms|4,539 ms|13,957 ms|
|Volley|560 ms|2,202 ms|4,275 ms|
|Retrofit|312 ms|889 ms|1,059 ms|


<br/>
<br/>

<h2>OKHttp3を使ってみる<h2>

----



<br/>

1.AndroidManifest.xmlにInternet権限を追加する。
```xml

<!--インターネット権限設定-->
<uses-permission android:name="android.permission.INTERNET" />

```
2.gradleアプリレベルにライブラリを追加する。

```gradle

dependencies{
implementation("com.squareup.okhttp3:okhttp:4.9.0")
implementation("com.squareup.okhttp3:logging-interceptor:4.9.0")
}

```

3.ネットワーク通信はThreadの中で実行。

```kotlin
// OKHttpClientオブジェクトを作成
var client = OkHttpClient()

// clientが要請するリクエストを作成
val request: Request = Request.Builder().url(통신할 Url).build()

// リクエストを実行する。
// 同期方式の処理をしたい場合はexecute、非同期の処理をしたい場合はenqueueを使う。
client.newCall(request).enqueue()

// 要請結果による作業のためにCallbackを付ける。
client.newCall(request).enqueue(object : Callback {
override fun onFailure(request: Request?, e: IOException?) {
//失敗した場合
}

override fun onResponse(response: Response?) {
//成功した場合
println(response?.body()?.string())
}
})
```

```kotlin
// 要請する作業をbodyに入れてgetpost作業を追加できる。
//ex) "userId"項目にデータuserIdを入れろ！
val body = FormBody.Builder().add("userId",userId).build() as RequestBody

//requestにbodyをpostしてくれる。 (getも可能)
val request : Request = Request.Builder().url(통신할Url).post(body).build()
```

<br/>


<h2>Retrofit2 お試し<h2>

----

1.AndroidManifest.xmlにInternet権限を追加する。
```xml

<!--インターネット権限設定-->
<uses-permission android:name="android.permission.INTERNET" />

```
<br/>
2.gradleアプリレベルにライブラリを追加する。<br/>
Retrofit は、自動的にJSON 応答を事前に定義された POJO を介して直列化することができる。<br> JSON を直列化するためには、まずGson converter が必要である。<br/>

```gradle

dependencies{
implementation 'com.squareup.retrofit2:retrofit:'Version''
implementation 'com.google.code.gson:gson:'Version''
implementation 'com.squareup.retrofit2:converter-gson:'Version''
}

```
<br/>

**OKHtp は既にRetrofit2 モジュールの従属性に含まれており、<br> 別途の OKHtp 設定が必要なら、次のように Retrofit2 から OKHtp 従属性を除外しなければならない。**
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