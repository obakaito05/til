ターミナルにて
% cd projects

雛形のアプリをクローンする
% git clone https://github.com/we-b/ajax_app.git

クローンしたアプリに移動
% cd ajax_app

% bundle install

javascriptのパッケージをインストール
% yarn install

データベースの作成
% rails db:create

マイグレーションを実行
% rails db:migrate

テーブルにpostsが入っていたら成功

AjaxAppをGItHubにて管理する。そのために.gitファイルを削除する。クローンしたアプリにはリポジトリ情報が含まれている。
リポジトリ情報を含むGitの情報は.gitというディレクトリの中で管理されている。そのため.Gitディレクトリを削除し不要な情報の削除を行う必要がある

lsコマンドを実行することで.Gitを含む文字列の検索することができる

例）ターミナルにて

% ls -a | grep .git


#出力されるもの
.git
.gitignore
 
 
これらは隠しディレクトリにあたる。
Finderで表示されなくてもターミナル上に表示されれば問題ない。
この表示されたものを削除していく。

rmコマンドを使用する。

rでディレクトリを指定して削除することができる。またfというオプションでディレクトリやファイルの管理制限に関係なく強制的に削除することができる。

ターミナルにて

% rm -rf .git


リポジトリ情報を削除できたらAjaxAppとしてGitHubで管理するための新しいリポジトリを作成する。
ターミナルにてgit initコマンドを実行、ローカルリポジトリを作成。

ターミナル

％ git init
Initialized empty Git repository in /Users/user_name/contents_exp_apps/ajax_app/.git/

あとはGitHubDesktopにて反映させる
マスターブランチで操作しないようにあらかじめブランチは切っておく。名前についてはなんでも良いが誰がみてもこの操作をしているとわかるものにしておく


ここまでできたらパスを変更させる
AjaxAppは投稿されたメモ一覧をトップページに表示させる仕様にする
そのためposts#indexへのパスをルートパスへ変更させる

新規投稿ページへの遷移は行わないので、get'posts/nwe'.to:'posts#new'の記述は削除する


config/routes.rb


Rails.application.routes.draw do

  get 'posts', to: 'posts#index'  # 編集する
  
  get 'posts/new', to: 'posts#new'  # 削除する 
  
  post 'posts', to: 'posts#create'
  
end


これらをこう変更する


config/routes.rb


Rails.application.routes.draw do

  root to: 'posts#index'  
  
  post 'posts', to: 'posts#create'
  
end


メモを降順にする。投稿の新しい順番に変更する。orderメソッドを使用。


app/contollers/posts_controller.rb


class PostsController < ApplicationController

  def index
  
    @posts = Post.order(id: "DESC")
    
  end

end


今回はトップページのみで完結するため
新規投稿ページや投稿完了ページなどは必要ないため削除する。

ajax_app/app/views/posts/new.html.erb
ajax_app/app/views/posts/create.html.erb

ただこれらを削除するとメモの投稿フォームがなくなってしまうのでトップページに投稿フォームが表示されるようにする。

views/posts/index.html.erb

<h1>AjaxApp</h1>

<%= form_with url:  "/posts", method: :post,id: "form" do |form| %>

  <%= form.text_field :content %>
  
  <%= form.submit '投稿する' , id: "submit" %>
  
<% end %>

<div id="list">
  
</div>


ここまでが追加記載

<% @posts.each do |post| %>

<div class="post">
  
  <div class="post-date">
    
    投稿日時：<%= post.created_at %>
    
  </div>
    
  <div class="post-content">
    
    <%= post.content %>
      
  </div>
    
</div>
    
<% end %>
  

タブタイトルを変更するために


app/views/layouts/application.html.erb
  

<!DOCTYPE html>
<html>
  <head>
    <title>AjaxApp</title>

    
を記載する
    

投稿後にトップページへリダイレクトするように編集する
    
app/controllers/posts_controller.rb
    
    
class PostsController < ApplicationController

 def index
                                             
   @posts = Post.order(id: "DESC")
                                             
 end

  #newアクションは使用しないためコメントアウトする
                                             
  #def new
                                             
  #end

 def create
                                             
   Post.create(content: params[:content])
                                             
   redirect_to action: :index  #作成後にトップページへ遷移するために追記する
                                             
 end

end
    
次はjavascriptを使用して実装をしていく
    
 
