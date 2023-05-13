HTMLの要素をjavascript上で取得し操作するためにDOMという仕組みを利用する

DOMとは
ドキュメントオブジェクトモデルといい、HTMLを解析しデータを作成する仕組みのこと

簡単に要約すると
HTMLは単なる文字情報になので、そのまま表示すると見にくい状態にあたる
HTMLをDOMへ変更し、入力していたCSSやjavascriptによる装飾を行なってユーザーがページを閲覧することができる

HTMLは階層構造になっているためDOMによって解析されたHTMLは階層構造のあるデータとなる
これをDOMツリーやドキュメントツリーと呼ぶ
javascriptのメソッドを使用するとDOMツリーを操作できる

javascriptの強みでもあるHTMLの要素名や「id,class」といった属性の情報をもとに
DOMツリーの一部を取得したりcssの変更、要素の追加、削除を行うことができる


DOMツリーからHTMLを取得する方法

３つ方法
①id名から取得する方法
②class名から取得する方法
③セレクタ名から取得する方法

説明
①document.getElementById("id名")を使用すること

マッチするidを持つHTMLの要素を取得することができる
DOMツリーから特定のHTMLの要素を取得するためのメソッドの1つ
引数に渡したidを持つ要素を取得する
id名はHTML上に必ず一つしか存在しないので複数形は使用しない

②document.getElementsByClassName("class名")を使用すること

classを指定して取得する際には上記を使用する
クラス名は複数存在することが考えられるためElementsと複数形となるため注意が必要

③document.querySelectorAll("セレクタ名")

セレクタ名とはcssでスタイルを適用するために指定している要素のこと
セレクタ名を指定して取得する場合はquarySelectorAllメソッドを使用する
HTML上から引数で指定したセレクタに合致するものを全て取得する
セレクタ名として頻繁に使用するものはclass名,id名, HTMLタグを覚えておく
ちなみにdocument.querySelector("セレクタ名")を指定した場合は
HTML上から引数で指定したセレクタに合致しる要素のうち一番最初に見つかった要素1つを取得することができる


HTMLファイルにjavascriptを使用する際はscript要素を追加する必要がある

script要素とは

実行できるコードを埋め込んだり参照したりするために使用されるHTMLのこと
追加したいHTMLへ記載する
 <script src="作成したjsファイル名"></script>

機能の実装

javascriptは何かが起きたらコードを実行するという概念に基づいて設計が行われている
何かが起きたら＝「イベント」と称する
イベントはHTMLに対して行われた処理の要求のこと
例）ユーザーがブラウザ上のボタンをクリックした

またイベントを認識してjavascriptのコードが動き出すことを「イベント発火」という
イベント発火の代表例）
・loadイベント　　　　　　　　　　　　　→ ページ全体が全て読み込まれた後にイベント発火する
・clickイベント　　　　　　　　　　　→　　指定された要素がクリックされた時にイベント発火する
・mouseoverイベント　　　→ マウスカーソルが指定された要素上に乗った時にイベント発火する
・mouseoutイベント　　　　　→ マウスカーソルが指定された要素上から外れた時にイベント発火する

イベントを発火させる際に使用するメソッドがaddEventListener()メソッド
addEventListener()メソッドの使用方法

要素.addEventListener('イベント名', 関数)

一例としてHTMLとjsのファイルを作成し確認
.htmlファイルにて
<li class="background-red" id="lists">リスト</li>

.jsにて
const pullDownButton = document.getElementById("lists")
console.log(pullDownButton)

にて反映しているか確認するもnullと中身が何もない状態になっているため
コードを読み込まれるようにする

.js
window.addEventListener('load', function(){

  const pullDownButton = document.getElementById("lists")
  console.log(pullDownButton)
})

先ほど勉強したaddEventListenerを使用する
イベントにはloadを使用しページ全体が全て読み込まれるようにする
すると先ほどnullだった情報が、
//<li class="background-red" id="lists">リスト</li>
と入っている状態になる

イベント発火をしているか確認する場合はconsole.log（）で確認する

次にmouseoverイベントを用いてマウスカーソルが指定の要素上に乗った時に、
イベント発火ができているかを確認する

.js
window.addEventListener('load', function(){

  const pullDownButton = document.getElementById("lists")

  pullDownButton.addEventListener('mouseover', function(){
    console.log("mouseover OK")
  })
})

//mouseoverした場合に、mouseover OKと出力される

同じようにマウスカーソルが要素から外れた時とクリックした時にイベント発火させる

.js

window.addEventListener('load', function(){

  const pullDownButton = document.getElementById("lists")

  pullDownButton.addEventListener('mouseover', function(){
    console.log("mouseover OK")
  })

  pullDownButton.addEventListener('mouseout', function(){
    console.log("mouseout OK")
  })

  pullDownButton.addEventListener('click', function() {
    console.log("click OK")
  })
})

次はjavascriptにてスタイルを操作する
