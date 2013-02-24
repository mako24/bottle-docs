.. Bottle documentation master file, created by
   sphinx-quickstart on Mon Jan 28 13:31:03 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Bottle: Python Web Framework
==================================

Bottle はPythonで実装されたシンプルで軽量なWebアプリケーションフレームワークです。
単一のファイルとして配布され、Python 標準ライブラリ以外に依存関係がない作りになっています。

Bottle には大きく次のような特徴があります。


* ルーティング
* テンプレート
* ユーティリティ
* ビルトインサーバ


簡単なサンプル
---------------

.. code-block:: python

   from bottle import route, run, template
   
   @route('/hello/:name')
   def index(name='World'):
       return template('<b>Hello {{name}}</b>!', name=name)
   
   run(host='localhost', port=8080)


上記のコードをスクリプトに保存して実行するか、Pythonのコンソールに貼り付けて実行するだけ。

結果はブラウザで http://localhost:8080/hello/world にアクセスして確認できます。


ダウンロードとインストール
--------------------------
Python Package Index ( PyPi_ ) からインストール（easy_install -U bottle）するか、 bottle.py_ をダウンロードして作成したプロジェクトのディレクトリへ配置します。
Bottle を利用するには、バージョンが2.5以上、または3.0以上のPythonがインストールされている必要があります。



ユーザガイド
------------
Bottleフレームワークを使ったWebアプリケーションの開発方法を学びたい方はここから読んでみてください。
ここで何かわからないことがあれば メーリングリスト_ へ質問してみてください。


.. toctree::
   :maxdepth: 2

   01_1_tutorial
   01_2_routing
   01_3_stpl
   01_4_api


ナレッジベース
----------------------------

.. toctree::
   :maxdepth: 2

   02_knowledge_base


Development and Contribution
----------------------------


.. toctree::
   :maxdepth: 2

   03_development_and_contribution



ライセンス
----------
ソースコードおよびドキュメントはMITライセンスに準じて利用可能です。


.. code-block:: none

   Copyright (c) 2012, Marcel Hellkamp.
   
   Permission is hereby granted, free of charge, to any person obtaining a copy
   of this software and associated documentation files (the "Software"), to deal
   in the Software without restriction, including without limitation the rights
   to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
   copies of the Software, and to permit persons to whom the Software is
   furnished to do so, subject to the following conditions:
   
   The above copyright notice and this permission notice shall be included in
   all copies or substantial portions of the Software.
   
   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
   AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
   OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
   THE SOFTWARE.



References:

* このドキュメントは Bottle公式サイト_ を翻訳したものです。

.. _Bottle公式サイト: http://bottlepy.org/docs/dev/
.. _PyPi: http://pypi.python.org/pypi/bottle
.. _bottle.py: https://github.com/defnull/bottle/raw/master/bottle.py
.. _メーリングリスト: mailto:bottlepy%40googlegroups.com

..  Indices and tables
..  ==================
..  
..  * :ref:`genindex`
..  * :ref:`modindex`
..  * :ref:`search`

