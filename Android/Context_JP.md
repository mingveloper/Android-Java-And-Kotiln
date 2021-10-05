<h1>Contextとは?</h1>

<br/>


* アプリケーションの現在の状態を示す。
* アクティビティやアプリケーションの情報を得るために使用できる。
* リソース、データベース、shared preferenceなどにアクセスするために使用できる。
* アクティビティとアプリケーションクラスは、Contextクラスを拡張したクラスである。


<br/>
<br/>  

<h2>Application Context</h2>

----

<br/>

Application Contextはシングルトンインスタンスであり、ActivityからgetApplicationContext()を通じてアクセスできる。

このContextはアプリケーションライフサイクルに縛られており、現在のContextが終了した後もContextが必要な作業や、アクティビティスコープから外れたContextが必要な作業に適しています。

たとえば、アプリケーションにシングルトンオブジェクトを生成し、当該オブジェクトにContextが必要であれば、常にApplication Contextを伝達しなければならない。

もし、Activity Contextを伝達すると、当該オブジェクトはActivityを常に参照してしまうので、アクティビティが画面に表示されない瞬間にもガベージ·コレクションが進まず、メモリの漏れが発生することになる。
アプリケーション全体で使用するライブラリを特定のアクティビティで初期化するなら、当然Application Contextを使用しなければならない。 getApplicationContext()は、ちょうど上のような場合にのみ使用すること。

<br/>  
<br/>

<h2>Activity Context</h2>

----

<br/>



Activity Context はactivity 内で有効なContextである。

このContextは、アクティビティライフサイクルと 繋がっていて、 Activity Contextはアクティビティと共に消滅しなければならない場合に使用しなければならない。 例えば、アクティビティとライフサイクルが同じオブジェクトを生成しなければならないときにActivity Contextが使用できる。


アプリの階層構造

![application_layer_screenshot](./../Images/Activity_Context.jpeg)


* Application Context はMyApplication, MainActivity1, MainActivity2 すべてで使用できる。
* MainActivity1のContextはMainActivity1でのみ使用できる。
* MainActivity2のContextはMainActivity2でのみ使用できる。


<br/>
<br/>

<h2>ContentProvider의 getContext()</h2>

----

抽象クラスContent Providerを相続したクラスでget Context()メソッドを通じて読み込むことができるContextはApplication Context。

<br/>
<br/>

<h2>いつ、どんなContextを使えばいいかな？</h2>

----

状況を想定して、いつ、どのようなContextを使用するか?。 

class My Applicationとclass MyDBシングルトンがあると仮定する。 <br/> 
そしてMyDBがcontextが必要な状況なら、どんなcontextが必要でしょうか？
正解はApplication Contextです。 <br/>もしActivity の Context を伝達したとすれば、アクティビティが使用されない場合でも MyDB が不要にアクティビティを参照し、メモリ漏れが発生することになる。 <br/>したがって、シングルトンの場合には、常にApplication Contextを伝達するのが正しい。<br/>
それでは、いつActivity Contextを使用するのか？ <br/>いつでもアクティビティを使用するとき、Toast, Dialog などの UIオペレーションでContextが必要なら、この時はActivity Contextを使用しなければならない。
<br/>常にできるだけ近いContextを使うようにしよう。 <br/>Activityにいる場合はActivity Contextを、アプリケーションにいる場合は、Application Contextを使用。 <br/>シングルトンの場合はApplication Contextを使用。


<br/>
<br/>

<h2>getApplicationContext()を使ってはいけない場合<h2>

----

<br/>

<H4>

* Application ContextはActivity Contextが提供する機能の全体は提供しない。 特にGUIと関連したコンタクト操作は失敗する確率が高い。
  
* Application Contextがユーザ呼び出しによって生成されたcleanupされていないオブジェクトを持っている場合、メモリ漏れが発生することがある。<br/> Activity オブジェクトはガベージコレクションが可能であるが、Application オブジェクトはプロセスが生きている間に残っている。

<br/>
<br/>  

<h2>Reference<h2>

----

* https://developer.android.com/reference/android/content/Context

* https://roomedia.tistory.com/entry/Android-Context%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C