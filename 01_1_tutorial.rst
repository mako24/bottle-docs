==============
チュートリアル
==============
このチュートリアルではBottleフレームワークのコンセプトと機能について紹介しています。基本的なトピックから高度な内容までひと通り網羅しています。
最初から最後まで読んでもよいですし、リファレンスとして使っても結構です。
*APIリファレンス* に、より詳細が記述されていますが、説明はチュートリアルより少ないかもしれません。
一般的な問題に対する解決策は *レシピ集* か *よくある質問* のページで見つかるかもしれません。
もし何かしらの助けが必要な場合、 メーリングリスト_ に参加するか、 IRCチャネル_ を訪れてみてください。



インストール
==============
Bottleフレームワークは外部フレームワークとの依存関係は一切ありません。あなたの作成したプロジェクトのディレクトリへ bottle.py_ をダウンロードするだけでコーディングを始める事ができます。

.. code-block:: sh

   $ wget http://bottlepy.org/bottle.py


このコマンドで、全ての新しい機能が含まれた最新の開発バージョンを取得することができます。もし安定版を使用したい場合は、 PyPi_ に公開されているバージョンを使用してください。
**pip** コマンドか **easy_install** コマンド、その他のパッケージマネージャを使ってインストールする事ができます。

.. code-block:: sh

   $ sudo pip install bottle              # 推奨
   $ sudo easy_install bottle             # pipを使う代わり
   $ sudo apt-get install python-bottle   # Ubuntuなど、Debian系のディストリビューションの場合

いずれの方法でも、Bottleアプリケーションを実行するためには Python 2.5 以上（3.x系を含む）がインストールされている必要があります。
システムにパッケージを追加する権限がない場合、また追加したくない場合は、まず virtualenv_ で環境を用意してください。

.. code-block:: sh

   $ virtualenv develop              # 仮想環境を作成する
   $ source develop/bin/activate     # 環境の切り替えをする
   (develop)$ pip install -U bottle  # Bottleをインストールする

virtualenv_ がインストールされていない場合は．．．

.. code-block:: sh

   $ wget https://raw.github.com/pypa/virtualenv/master/virtualenv.py
   $ python virtualenv.py develop    # Create virtual environment
   $ source develop/bin/activate     # Change default python to virtual one
   (develop)$ pip install -U bottle  # Install bottle to virtual environment


クイックスタート
================
このチュートリアルではBottleがインストール済の状態か、プロジェクトディレクトリにコピーしてあることを前提で進めます。
まずは一番シンプルな "Hello World" から始めましょう。

.. code-block:: python

   from bottle import route, run
   
   @route('/hello')
   def hello():
       return "Hello World!"
   
   run(host='localhost', port=8080, debug=True)

これをスクリプトに記述して、実行してみてください。 http://localhost:8080/hello にブラウザでアクセスしてみて、"Hello World"が表示されれば正常に動いています。

**Route()** デコレータは、URLを関数などの処理にバインドします。この例では、/hello に対して hello() 関数が割り当てられています。
これはルーティングと呼ばれ、Bottleフレームワークで最も重要な概念です。複数のルートを任意に定義することが可能です。
ブラウザでURLをリクエストする度に、関連付けられた関数が実行され値がブラウザに返却されます。

サンプルコードの最終行に記述されている **run()** 関数を呼び出すことでビルトインのWebサーバが起動します。ビルトインWebサーバはlocalhostの8080ポートで実行されます。
停止するにはCtrl+Cを入力してください。
実行サーバは後で変更することができますが、今は開発サーバがあれば十分です。開発サーバはセットアップ不要で、ローカルでのテストと実行をする上で最も簡単な方法です。

*Debugモード* は初期の開発段階では非常に役に立ちますが、アプリケーションを公開する際にはモードをOFF（False）にすることを忘れないようにしてください。

非常に簡単なサンプルですが、Bottleを使ったアプリケーションの構築についてのコンセプトをよく示しているものです。


デフォルトアプリケーション
--------------------------
簡潔さのため、このチュートリアルのサンプルはルーティング定義にモジュールレベルの **route()** デコレータを使用しています。
この場合、ルーティング定義はグローバルなデフォルトアプリケーション（default application）に追加されます。
最初に **route()** が呼び出された際に、自動的に **Bottle** のインスタンスが生成されます。
いくつかのモジュールレベルデコレータもデフォルトアプリケーションに関係しています。
もしオブジェクト指向的な記述をしたいのであれば、次のように記述することも可能です。

.. code-block:: python

   from bottle import Bottle, run, template
   
   app = Bottle()
   
   @app.route('/hello')
   def hello():
       return "Hello World!"
   
   run(app, host='localhost', port=8080)

オブジェクト指向的な手法については、ここではこれ以上触れません。（Default Applicationの章に記載があります）
そういう方法も選べることを覚えておいてください。


リクエストルーティング
======================
前項では単一のルーティング定義を持った、簡単なサンプルを実装しました。ルーティング部分のサンプルを改めて説明します。

.. code-block:: python

   @route('/hello')
   def hello():
       return "Hello World!"

**route()** デコレータはURLを関数に関連付けています。またルートがデフォルトアプリケーションに追加されます。
ひとつしかルートがないのもつまらないので、次のように追加してみましょう。

.. code-block:: python

   @route('/')
   @route('/hello/<name>')
   def greet(name='Stranger'):
       return template('Hello {{name}}, how are you?', name=name)

この例は2つのことを示しています。
* 単一のコールバック関数にひとつ以上のルートを定義できる
* URLにはワイルドカードを指定することができ、キーワード引数を介してアクセスすることができる


動的ルーティング
----------------
ワイルドカードを含むルーティングは *動的ルーティング* （静的なルーティングの対として）と呼ばれ、同時に複数のURLに一致します。
シンプルなワイルドカードはルーティング定義の中でスラッシュで区切られた **<name>** の形式をとります。
例えば、ルートが「/hello/<name>」と定義されている場合、「/hello/alice」、「/hello/bbb」は有効なURLとして受け付けられます。
しかし、「/hello」「/hello/」「/hello/mr/smith」は無効です。

URLの中に設定されたワイルドカードは、キーワード引数としてコールバック関数に渡されます。
この機能を正しく使用すれば、見栄えよく意味のあるURLを持ったRESTfulな実装が可能です。

他のURLサンプルを下記に示します。

.. code-block:: python

   @route('/wiki/<pagename>')            # matches /wiki/Learning_Python
   def show_wiki_page(pagename):
       ...
   
   @route('/<action>/<user>')            # matches /follow/defnull
   def user_api(action, user):
       ...

*バージョン0.10からの新機能です*

フィルタは、URL中のワイルドカードがコールバック関数に渡される前に、データ型を明確に定義するために使用されます。フィルタされた
ワイルドカードは **<name:filter>** または **<name:filter:config>** の形式で定義されます。
configはオプションとして使用され、その構文は使用するフィルタによって異なります。

デフォルトでは下記のフィルタが定義されています。（今後追加されるかもしれません）

* **:int**
  符号付きの整数にマッチします。integerに変換されます。
* **:float**
  intに似ていますが、浮動小数点数にマッチします。
* **:path**
  スラッシュを含むすべての文字列に最短マッチさせます。パスセグメントをマッチングする際に使用します。
* **:re**
  configに正規表現を指定してマッチングに使用することができます。マッチした文字列は変更できません。

いくつか例を見てみましょう。

.. code-block:: python

   @route('/object/<id:int>')
   def callback(id):
       assert isinstance(id, int)
   
   @route('/show/<name:re:[a-z]+>')
   def callback(name):
       assert name.isalpha()
   
   @route('/static/<path:path>')
   def callback(path):
       return static_file(path, ...)

カスタムしたフィルタを作ることもできます。詳細は **Routing** のセクションを参照してください。

*バージョン0.10からの変更点があります*

新しい構文がバージョン0.10から導入されましたが、古い構文もまだ動作します。いくつかのサンプルで古い構文を見ることができるかもしれません。

============== ================
古い構文       新しい構文
============== ================
:name          <name>
:name#regexp#  <name:re:regexp>
:#regexp#      <:re:regexp>
:##            <:re>
============== ================

現時点では非推奨ではありませんが、最終的にはそうなりますので、今後新しくプロジェクトを作成する場合は、古い構文は使用しないようにしてください。


HTTPリクエストメソッド
----------------------
HTTPプロトコルにはいくつかの メソッド_ が定義されています。Bottleでは、メソッドがパラメータに明示されていなければデフォルトでGETになり、ルートのマッチングもGETリクエストのみ適用されます。
POST/PUT/DELETEなどの他のメソッドをハンドリングする場合、**route()** デコレータのキーワード引数(method)を指定するか、各リクエストに対応したデコレータを使用する必要があります。
**get()**、**post()**、**put()**、**delete()** です。

HTMLフォームからの送信には一般的にPOSTメソッドが使用されます。次のサンプルではどのようにログインページのハンドリングを行うかの例を示しています。

.. code-block:: python

   from bottle import get, post, request
   
   @get('/login') # or @route('/login')
   def login_form():
       return '''<form method="POST" action="/login">
                   <input name="name"     type="text" />
                   <input name="password" type="password" />
                   <input type="submit" />
                 </form>'''
   
   @post('/login') # or @route('/login', method='POST')
   def login_submit():
       name     = request.forms.get('name')
       password = request.forms.get('password')
       if check_login(name, password):
           return "<p>Your login was correct</p>"
       else:
           return "<p>Login failed</p>"

この例では /login のURLに対して異なる2つの関数がGETリクエスト、POSTリクエストに対応しています。
最初の関数はGETリクエストの処理でHTMLをユーザに表示し、2つ目の関数はサブミットされたフォームを処理するために起動され、ログイン認証処理を実行しています。
**Request.forms** の詳しい使い方は *リクエストデータ* で説明します。


**HEADとANYメソッド**

HEADメソッドは、ほぼGETと同じものですが、レスポンスボディを返さないという違いがあります。これはドキュメント全体をダウンロードせず、メタ情報のみ取得するのに役立ちます。BottleがHEADリクエストを受け付けた時、GETメソッドの処理をしてからリクエストボディを取り除くので、HEADルートを定義する必要はありません。

Additionally, the non-standard ANY method works as a low priority fallback: Routes that listen to ANY will match requests regardless of their HTTP method but only if no other more specific route is defined. This is helpful for proxy-routes that redirect requests to more specific sub-applications.
加えて、

To sum it up: HEAD requests fall back to GET routes and all requests fall back to ANY routes, but only if there is no matching route for the original request method. It’s as simple as that.


静的なファイルのルーティング
----------------------------


エラーページ
------------


コンテンツの生成
================


リクエストデータ
================


テンプレート
============


プラグイン
==========


開発方法
========


アプリケーションの配置
======================


用語集
======




.. _PyPi: http://pypi.python.org/pypi/bottle
.. _メーリングリスト: mailto:bottlepy%40googlegroups.com
.. _IRCチャネル: http://webchat.freenode.net/?channels=bottlepy
.. _bottle.py: http://bottlepy.org/bottle.py
.. _virtualenv: http://pypi.python.org/pypi/virtualenv
.. _メソッド: http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html
