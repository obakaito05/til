イベント発火元となった要素を取得する

thisメソッドを使用しイベントが発火した要素を取得することができる
thisメソッドとは
関数の中でthisを使用するとイベント発火元となった要素を取得することができる
thisは使用する場面によって取得できるものが異なる
先ほど使用したファイルを使用する

.js
window.addEventListener('load', function(){

  const pullDownButton = document.getElementById("lists")
  const pullDownParents = document.getElementById("pull-down")

  pullDownButton.addEventListener('mouseover', function(){
    this.setAttribute("style", "background-color:#FFBEDA;")  　　　←ここをthisに変更する
  })

  pullDownButton.addEventListener('mouseout', function(){
    this.removeAttribute("style")　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←同じものを使用しているためthisへ変更する
  })

  pullDownButton.addEventListener('click', function() {
    pullDownParents.setAttribute("style", "display:block;")
  })
})

このthisはpullDownButtonが二度使用されている為省略するために使用
先ほどの処理自体に変更はないが、見やすいコードへの変更のためにthisを使用することが必要になる

しかし今のままではクリックしてプルダウンメニューを表示することができるが、直すことができない
そのためクリックしてプルダウンメニューの表示を非表示へ変更する
if分による条件分岐を用いて実装を行う

指定の要素をクリックした時にプルダウンメニューのstyle属性にdisplay:block;が指定されているかどうかで分岐する
指定されていなければ先ほど行ったsetAttribute display:block;を指定する
指定されている場合はremoveAttributeを用いてstyle属性を取り除く

プルダウンメニューの状態を確認するために付与されているインラインスタイルを取得する必要がある
属性の取得はgetAttributeメソッドを使用する

getAttributeメソッド
要素.getAttribute('属性名')


.jsにて
pullDownButton.addEventListener('click', function() {
    // プルダウンメニューの表示と非表示の設定
    if (pullDownParents.getAttribute("style") == "display:block;") {
      // pullDownParentsのstyle属性にdisplay:block;が指定されている場合（つまり表示されている時）実行される
      pullDownParents.removeAttribute("style")
    } else {
      // pullDownParentsのstyle属性にdisplay:block;が指定されていない場合（つまり非表示の時）実行される
      pullDownParents.setAttribute("style", "display:block;")
    }
})

二行目の記述で表示されているときはクリックを押すと消すことができる状態になる
elseでそれ以外の時になるのでその下の4行目の分で非表示のときを表すことができる
ここまでで条件分岐がうまくいっていればクリックすればプルダウンメニューの表示・非表示が行えるようになる


デバッグをして条件分岐の処理を確認する
debuggerメソッドを使用してデバッグを行う

debuggerとは
使用することでソースコードに処理を一旦停止させる場所を指定できる
指定したブレークポイントまで処理が進むことで、処理を一時的に停止することができる
処理が停止している状態で、特定の変数の値を確認したり一行だけ処理を進めたりなど幅広いデバッグが可能となる

実際に先ほどの位置にdebuggerを使用してみる

.js
pullDownButton.addEventListener('click', function() {
    debugger
    // プルダウンメニューの表示と非表示の設定
    if (pullDownParents.getAttribute("style") == "display:block;") {
      pullDownParents.removeAttribute("style")
    } else {
      pullDownParents.setAttribute("style", "display:block;")
    }
  })

入力後コンソールを開いてプルダウンメニューを表示するために要素をクリックする
するとdebuggerで処理が停止していることが確認できる
ブラウザ上で停止している状態で、上部にある再生ボタンのような形をした青色のボタンをクリックすることで
debuggerによる停止状態を解除することができる
確認後は削除するのを忘れないこと


次は選択したリストを表示する
リストの表示、非表示を切り替えることができたため出てきたリストの文字列を取得し画面上に表示する
イベント発火させるHTML要素を取得する

.html
<ul class="show-lists hidden" id="pull-down">
      <li class="pull-down-list">リスト1</li>
      <li class="pull-down-list">リスト2</li>
      <li class="pull-down-list">リスト3</li>
      <li class="pull-down-list">リスト4</li>
    </ul>
    
.js
classを指定した要素を取得する
HTMLにてli class=pull-down-listを作成したためその文をjsにも反映させる
const pullDownChild = document.querySelectorAll(".pull-down-list")の文を追加記載

pullDownChildは配列オブジェクトとなっている
配列の中に取得した4つのHTML要素が格納されている

document.querySelectorAll(".pull-down-list")
//=> NodeList(4) li.pull-down-list, li.pull-down-list, li.pull-down-list, li.pull-down-list

この中に格納されている要素1つひとつに対して処理が実行できるように行う
配列に対して繰り返し処理を行うforEachを使用

.js
// コースの値を取得し表示する
  pullDownChild.forEach(function(list) {
  })

この記述を行うことでpullDownChildに格納されているHTML要素のの全てに処理を行うことが可能となる
ここにイベントを追加する

.js
pullDownChild.forEach(function(list) {
    list.addEventListener('click', function() {
      console.log(list)
    })


コンソール上で確認し、クリックするとそれに対応して反応があるか確認する
出力できていればイベント発火した要素に対して処理ができているということがわかる
