Javascriptの戻り値

コンソール
見本
function calc(num1,num2){
  num1*num2
}

const num1 = 3
const num2 = 4
console.log(calc(num1,num2))

本来なら12が出力されることが予想されるが値は出力されない
returnを明示する必要があるので再度記載

コンソール
function calc(num1,num2){
  return num1*num2
}

const num1 = 3
const num2 = 4
console.log(calc(num1,num2))

他の関数定義
関数式
javascriptの関数宣言であれば、function関数名(引数){~}となる
関数式の場合はfunction(引数){}という無名の関数を変数に定義または代入して関数を定義する、という記述となる

関数宣言と関数式の違いとは

関数宣言と関数式では読み込まれるタイミングが異なる

コンソール
hello()

const hello = function(){
  console.log('hello')
}

上記のコードを実行するとUncaught ReferenceError...といった形でエラーが表示される
1行目の時点で関数helloは定義されていないため起こる

コンソール
hello()

function hello(){
  console.log('hello')
}
//hello

この場合はエラーが表示されることはない
javascriptにおいては関数宣言は先に読み込まれるため、このような事象が発生する

無数関数

無数関数は関数なしで関数を定義することができる
メリットとしてはより簡潔なコードが記述できる

// 関数宣言
function hello(){
  console.log('hello')
}

// 関数式（無名関数）
const hello = function(){
  console.log('hello')
}

コンソール
const calc = function(num1,num2){
  return num1*num2
}

const num1 = 3
const num2 = 4
console.log(calc(num1,num2))
//12

即時関数
関数を定義すると同時に実行される構文
関数を定義してから呼び出す手間を省くことができる。

// 無名関数
const countNum = function(num) {
  console.log(num)
}
countNum(1)

// 即時関数
(function countNum(num) {
  console.log(num)
})(1)

()の中にfunctionから始まる関数定義そのものを配置することで、関数を即実行するということができるようになる
その結果関数を呼び出すという手間が省ける

短い記述の関数

アロー関数とは以下のようにfunctionの記述を省略し、代わりに()=>という記述によって
関数を定義する構文のことでより短い記述で関数定義をできるというメリットがある

// 無名関数
const 変数名 = function(){
  処理
}

// アロー関数
const 変数名 = () => {
  処理
}

コンソール
const countNum = (num) => {
  console.log(num)
}
countNum(1)
//1

関数宣言・・・標準的な関数の定義
無名関数・・・関数を多く使用するコードがあるときに使用する。関数名の重複を避けることができる。
即時関数・・・流用する可能性のない関数を定義するときに使用する。ベット関数を定義する手間がない。
アロー関数・・無名関数または即時関数において、より省略した記述をしたい時に使用する。

javascriptにおけるセミコロン
変数定義などの最後にはセミコロンを付与する

const name = "Tom";
const age = 25;

const introduction = `私の名前は${name}、${age}歳です`;
console.log(introduction);

セミコロンをつけないもの
// （1）関数宣言
function hello(){
}

//（2）if文
if (true) {
} else {
}

開発現場ごとにセミコロンをつけるかつけないかルールが分かれているためその状況に応じて対応する必要がある
