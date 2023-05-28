続きをおこなっていく
javascriptで実装する準備を行う

memo.jsを作成
javascriptのところに自身で作成を行う
ただそのままだと読み込みができていない状態なので読み込むための設定を行う
application.jsのファイルを開く


app/javascript/packs/application.js


require("@rails/ujs").start()

require("turbolinks").start()

require("@rails/activestorage").start()

require("channels")

require("../memo")  


上記4つはすでに記載があるので、先程追加作成したmemo.jsを読み込むために5段落目を追加記載
..については以前も説明したが、一つ上の階層を意味するものでapplication.jsから見てどこに位置しているのかを確認して記載する


これで読み取りは完了したためjavascriptからサーバーサイドへリクエストを送信する処理を設定する

先程作成したmemo.jsを使用する
以前に記載したaddEventListenerメソッドを使用する
第一引数にはloadイベントを指定し、第二引数の中には実行したい処理を記載


memo.js


function post (){
 //リクエストを送信する処理
};

window.addEventListener('load', post);


記載後正常にイベント発火しているか確認したい場合は
console.log("イベント発火")を記載する
記載する箇所はリクエストを送信する処理のところ


AjaxAppでは投稿ボタンをクリックすることでメモを非同期投稿できるようにするため、投稿ボタンの要素を取得する。
getElementByIdメソッドを用いてクリックの対象となる投稿ボタンの要素を取得する


app/views/posts/index.html.erbにて投稿ボタンにsubmitというidが付与されているか確認。

<h1>AjaxApp</h1>

<%= form_with url:  "/posts", method: :post,id: "form" do |form| %>

  <%= form.text_field :content , id: "content"%>
  
  <%= form.submit '投稿する' , id: "submit" %>
  
<% end %>


確認後4行目の値をjavascritで取得できるようにする


memo.jsに追加記載


const submit = document.getElementById("submit");

これで取得した投稿ボタンの要素を変数submitへ格納できた


次に投稿ボタンをクリックした時にイベント発火させる
投稿ボタンがクリックされたことを認識するためにsubmit.addEventListenerと記述する
これは以前も行ったことがあるため復習。
先ほどはloadを使用し読み込んだらというイベントを使用したが今回はクリックされたをイベントとして認識したいため、addEventListenerの
第一引数にはclickイベントを使用する第二引数に実行したい処理を記述することは変わりません。


memo.js


submit.addEventListener("click", () => {

    console.log("イベント発火");
    

を追加記載
クリックした際にイベント発火が起こるかこれで確認できる


フォームに入力した値を取得する

投稿したいメモの内容をサーバーサイドへ送信するためにフォームに入力した値を取得する
先ほどと同様にgetElenentByIdメソッドを使用してフォームの要素を取得する


memo.js

function post (){

  const submit = document.getElementById("submit");
  
  submit.addEventListener("click", () => {
  
    const form = document.getElementById("form");
    
  });
  
};

window.addEventListener('load', post);

これで先ほどと同様にフォームの要素を変数formに格納できた
FormDataオブジェクトを使用する
