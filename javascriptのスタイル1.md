マウスが要素上に存在している時に色が変化するようにする

インラインスタイルとsetAttributeメソッドを理解する

インラインスタイルとは
HTML要素の開始タグの中に直接CSSソースコードを記述するプロパティの指定方法のこと
インラインスタイルの追加と削除はそれぞれsetAttributeとremoveAttributeというメソッドを使用する

setAttributeメソッド
指定した要素上に新しい属性を追加、または既存の属性の値を変更する
使用例
要素.setAttribute(name, value)
nameは属性の名前を文字列で指定する
valueは属性に設定したい値を指定する

見本
HTML
<div id="test">テスト</div>

setAttributeメソッドの使用方法
const sample = document.getElementById("test")

sample.setAttribute("style", "color: red;")
// <div id="test" style="color: red;">テスト</div> が取得できる


removeAttributeメソッド
指定した要素から特定の属性を削除する
使用例
要素.removeAttribute(name)
nameは属性の名前を文字列で指定する

見本
HTML
<div class="contents" id="apple">りんご</div>

removeAttributeメソッドの使用方法
const apple = document.getElementById("apple")

apple.removeAttribute("class")
// <div id="apple">りんご</div> が取得できる

これら2つを使用してマウスカーソルが指定の要素上に存在している時に色が変色するように行う
以前記述した続きに記載する

.js

window.addEventListener('load', function(){

  const pullDownButton = document.getElementById("lists")

  pullDownButton.addEventListener('mouseover', function(){
    pullDownButton.setAttribute("style", "background-color:#FFBEDA;")  ←ここが追加記載の分
  })

  pullDownButton.addEventListener('mouseout', function(){
    console.log("mouseout OK")
  })

  pullDownButton.addEventListener('click', function() {
    console.log("click OK")
  })
})

これを記載することによってmouseoverしたタイミングでイベント発火が起こる
今回の場合は色が変化する

またイベント発火前と発火後で表示されるHTMLにも変化が出る
発火前
html
<li class="background-red" id="lists">リスト
  
発火後
html
<li class="background-red" id="lists" style="background-color:#FFBEDA;">リスト

ただ現状のままではマウスを乗せるの変化するが、離した後も変化した状態が続くため
外した際には色が戻るように設定を加える必要がある
そのため今回触るのはmouseoutの段落にあたる
.js
  
pullDownButton.addEventListener('mouseout', function(){
    pullDownButton.removeAttribute("style")                 ←先ほどと同様に追加記載
  })

この文を記載することによってマウスカーソルを要素から外すと色が戻るように実装ができます

プルダウンメニューを表示してみる
先に表示するプルダウンメニューの要素はHTMLファイルにて作成する
しかし単にHTMLファイルにプルダウンメニューの記述をするだけでは初めから画面に表示された状態のままになってしまう
なので、cssのdisplay:none;を用いて非表示の状態にする
  
html
  <ul class="show-lists">
      <li>リスト1</li>
      <li>リスト2</li>
      <li>リスト3</li>
      <li>リスト4</li>
    </ul>

このままだと画面上に表示されたままになってしまうため先ほど記載した
display:none;を指定し、非表示の状態にする

.cssにて
.hidden{
  display: none;
}
  
次は先ほど非表示の状態にしたプルダウンメニューをクリックすることで切り替えるようにする

イベント発火させる要素にidを指定する
.html
<ul class="show-lists hidden" id="pull-down">  

idを指定した要素を取得するため.jsの記述を行う  
.js  
  
const pullDownButton = document.getElementById("lists")
const pullDownParents = document.getElementById("pull-down")　　　　　　　　　　←先ほどのファイルに追加記載

これでHTMlの情報を取得することができたので実際に表示を行う
要素をクリックすると表示ができるようにしたい
かつdisplay:block;を指定し表示する必要があるため
.js
pullDownButton.addEventListener('click', function() {
    pullDownParents.setAttribute("style", "display:block;")     ←先ほどのファイルに追加記載
  })
  
記載する際にconsole.log("click OK")は消しておく

.html
<ul class="show-lists hidden">    ←hiddenを追加記載する
      <li>リスト1</li>
      <li>リスト2</li>
      <li>リスト3</li>
      <li>リスト4</li>
    </ul>
  
これで先ほど記載したプルダウンメニューは非表示となる
