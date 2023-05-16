画面表示1でforEach関数でイベント発火した要素に対して処理ができていることが確認できた

このままクリックした要素の文字列を取得できるように実装する
要素の文字列を取得する際にはinnerHtMLプロパティを使用する

innerHTMLプロパティとは
HTML要素の取得や書き換えを行うことができる
画面表示1までだとクリックすると対応した各要素が出力されていた
例）
リスト１をクリック
li class=pull-down-list リスト１　/li
が出力されていたが、innerHTMLを使用し
リスト1のみを出力できるようにする

.jsにて
pullDownChild.forEach(function(list) {
    list.addEventListener('click', function() {
      const value = list.innerHTML
      console.log(value)
    })
    
innerHTMLを用いることでクリックした要素の文字列を変数valueに代入している
そのvalueの値をconsole.logを記述することで所得した値の確認をしている
すると出力されるのはリスト1のみで他のli class等は出力されない
この結果からクリックしたリストの文字列が取得できていることが確認できる

あとはクリックしたものを実際に画面に表示したいのでまたhtmlを触る
.html
 <span id="current-list">選択されていません</span>
 
 jsに反映させる
 .js
 const currentList = document.getElementById("current-list")
 
  pullDownChild.forEach(function(list) {
    list.addEventListener('click', function() {
      const value = list.innerHTML
      currentList.innerHTML = value
    })
 
 
 currentListにconst valueが入力される
 
 ここまでで実装は完了しているのですが、一行目にwindow.addEventListenerを設置し
 ページが読み込まれた後に実行するようにしているがこの記述方法はあまり用いられないため記述方法を変更する
 
 これまで行ってきた処理名をつける
 今回はpullDownという関数名を設ける
 
 
 .js 
function pullDown() {　　　　　　　　　　←追加記載

  const pullDownButton = document.getElementById("lists")
  const pullDownParents = document.getElementById("pull-down")
  const pullDownChild = document.querySelectorAll(".pull-down-list")
  const currentList = document.getElementById("current-list")

  pullDownButton.addEventListener('mouseover', function(){
    this.setAttribute("style", "background-color:#FFBEDA;")
  })

  pullDownButton.addEventListener('mouseout', function(){
    this.removeAttribute("style")
  })

  pullDownButton.addEventListener('click', function() {
    if (pullDownParents.getAttribute("style") == "display:block;") {
      pullDownParents.removeAttribute("style")
    } else {
      pullDownParents.setAttribute("style", "display:block;")
    }
  })

  pullDownChild.forEach(function(list) {
    list.addEventListener('click', function() {
      const value = list.innerHTML
      currentList.innerHTML = value
    })
  })
}

window.addEventListener('load', pullDown) 　　　　←この部分を最後に持ってくる
 
ここまでで一連の処理は完了になる
最後の変更点については処理自体に変更がないのでブラウザ上での挙動に変更はない

