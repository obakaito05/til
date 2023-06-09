オブジェクトの作成
そもそもオブジェクトとは
データや処理といった情報を一つにまとめた集合体のこと
見本

データ
名前：伊藤さん
性別：男性
年齢：30歳

処理
・歩く
・食べる

これらの情報をhumanとして一つにまとめるとオブジェクトの完成
データとはキーとバリューのセットになったもの
プロパティとはキーとバリューのセットになったもの
データ＝プロパティ
オブジェクトの要素を決める役割をもつ
オブジェクトはプロパティ（データ）の集合体から成り立っているもの

プロパティの書き方
変数名　= {キー：バリュー}
         プロパティ
         
先ほどのhumanを例として考える

let human = {              ←ここから
  name: "Ito",
  gender: "man",
  age: 30,
 }　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここまでをオブジェクトとゆう
 
もしオブジェクトに追加したい場合は

1.オブジェクト、プロパティに値を代入する
2.オブジェクト['プロパティ']に値を代入する

見本
コンソール
let human = { name: 'yamada' }
console.log(human)

1の場合　human.age = 25
２の場合　human['address'] = 'Tokyo'

console.log(human)

//{ name: "yamada" }
//{ name: "yamada",age:25,address:"Tokyo" }

このように入力すると最初に定義していた
humanにageとaddressを追加することができる


プロパティの値を変更する場合
オブジェクト名.プロパティ=変更したい値を記述する
コンソール
見本
let human = {
  name: "yamada",
  age: 25,
  address: 'Tokyo'
}

console.log(human.name)

human.name = "yabe"
console.log(human.name)

//yamada
//yabe
 
これで上書きすることができる


オブジェクトにメソッドを追加して操作する

見本
コンソール
let human = {
  name: "yamada",
  age: 25,
  address: 'Tokyo',

  talk: function(){
    console.log(`私の名前は${human.name}、${human.age}歳です。住所は${human.address}です`)
  }
}

human.talk()

//私の名前はyamada,２５歳です。住所はTokyoです

humanオブジェクトの中にtalkというメソッドを定義。
このメソッドはconsole.logを用いてあらかじめ記述した文字列をコンソール状に表示する
human.talk()でメソッドが実行され、talkメソッドの処理が実行される
 

あらかじめ用意されたオブジェクト
javascriptはブラウザで動くプログラミング言語であるため
あらかじめブラウザを操作するメソッドを持つオブジェクトが定義されている

windowオブジェクト
ブラウザの情報を持っているオブジェクトでブラウザの情報を持つため
ブラウザオブジェクトと呼ばれる
javascriptで予め定義されているメソッドやオブジェクトはすべてwindowオブジェクトのプロパティであると言える

windowオブジェクトの中でも最も頻繁に使用する可能性が高い
オブジェクトがdocumentオブジェクトにあたる

documentオブジェクトとは
ブラウザ上で表示された情報(HTML)を操作することができるオブジェクトのこと
documentオブジェクトは多くのプロパティやメソッドを持っているため

ブラウザのオブジェクトを実際に取得してみる

コンソール
window.alert("ブラウザオブジェクトの取得に成功！")

しっかりとできていれば入力後には画面に反映される
先ほどコンソールに入力した
window.alert("ブラウザオブジェクトの取得に成功！")
windowを省略することができる

実際に省略してできるか確認してみることをお勧めします
基本的に省略すること

次回はHTMLの要素をjavascript上で操作する仕組みを理解する
