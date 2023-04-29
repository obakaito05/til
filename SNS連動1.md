SNS認証をする場合

連動させるアプリのAPI側の設定をすることが必要になるので連動させるアプリによって調べること。
facebookの場合はアプリIDとapp secretが作成され後に必要になる。
Googleの場合はクライアントIDとクライアントシークレットにあたる。

gemに記載する必要がある。
gem 'omniauth-facebook'
gem 'omniauth-google-oauth2'
gem "omniauth-rails_csrf_protection"
gem 'omniauth', '~>1.9.1'
と記載する。これはfacebookとgmailのomniauthをインストールする場合。

omniauthとは
Google、Facebook、twitter等のSNSアカウントを用いてユーザー登録やログインなどを実装できるgemのことで
Omniauth認証はCSRF脆弱性が指摘されているため、「omniauth-rails_csrf_protection」というGemをインストールして対策をすることが必要になる

ターミナルにて
bundle install

インストール後
環境変数を設定する。
ターミナル

% vim ~/.zshrc
入力後画面が変わるため、
iを押してインサートモードに移行し、下記を追記する。既存の記述は消去しない。

export FACEBOOK_CLIENT_ID='メモしたアプリID'
export FACEBOOK_CLIENT_SECRET='メモしたapp secret'
export GOOGLE_CLIENT_ID='メモしたクライアントID'
export GOOGLE_CLIENT_SECRET='メモしたクライアントシークレット'

編集が終わったらescapeキーを押してから:wqと入力して保存して終了

再度ターミナルにて
% source ~/.zshrc

環境変数の設定がこれにて完了。
できている確認する場合は
rails cを入力し
irb(main):001:0> ENV['FACEBOOK_CLIENT_ID']を一つずつ入力し確認。

次にアプリ側で環境変数の設定をする
config/initializers/devise.rbのファイルを開いて下記を入力。

config.omniauth :facebook,ENV['FACEBOOK_CLIENT_ID'],ENV['FACEBOOK_CLIENT_SECRET']
config.omniauth :google_oauth2,ENV['GOOGLE_CLIENT_ID'],ENV['GOOGLE_CLIENT_SECRET']
end

SNS認証とユーザー登録のタイミングが異なる仕様であるためSNS認証時にはusersテーブルを使用することができないため新しいテーブルを作成する必要がある。
SNS認証時の情報を保存するSnsCredentialモデルを作成

ターミナルにて
rails g model sns_credential

作成後
db/migrate/XXXXXXXXXXXXXXXX_create_sns_credentials.rbにて
class CreateSnsCredentials < ActiveRecord::Migration[6.0]
 def change
   create_table :sns_credentials do |t|
     t.string :provider　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここから入力
     t.string :uid
     t.references :user,  foreign_key: true　　　　　　　　　　　←ここまで入力

     t.timestamps
   end
 end
end
を入力。
userモデルとのアソシエーションのために外部キーとしてuser_idを持たせる

ターミナルにてテーブルに反映させる
rails db:migrate

deviseにてOmniAuthを使用できるように編集を行う

app/models/user.rb
class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable, :omniauthable, omniauth_providers: [:facebook, :google_oauth2]

omniauthable以降を入力しないと使用することができない状態になっている。
今回はfacebookとgoogleのため[]の中に記載している。

その後アソシエーションを記入する
先ほど作成したSnsCredentialモデルとUserモデルの設定

app/models/user.rb
has_many :sns_credentials

app/models/sns_credential.rb
belongs_to :user

deviseのコントローラーの再設定を行う
ターミナルにて
% rails g devise:controllers users

再設定ができたので次はdeviseのルーティングを変更する必要がある
config/routes.rb
Rails.application.routes.draw do
 devise_for :users, controllers: {　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここから入力
   omniauth_callbacks: 'users/omniauth_callbacks',
   registrations: 'users/registrations'
 }　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここまで入力
  root 'users#index' 
  resources :users, only: :new 
end

アクションの登録
app/controllers/users/omniauth_callbacks_controller.rb
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController

 def facebook
  authorization
 end

 def google_oauth2
  authorization
 end

 private

 def authorization
   @user = User.from_omniauth(request.env["omniauth.auth"])
 end
end

ビューにも反映
app/views/users/new.html.erb
<%= link_to "Facebookで登録", user_facebook_omniauth_authorize_path, method: :post%>
<%= link_to "Googleで登録", user_google_oauth2_omniauth_authorize_path, method: :post%>


これでfacebookとgoogle_oauth2アクションの呼び出しに成功。
続きはSNS連動2へ
