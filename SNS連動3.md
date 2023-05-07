SNS認証時にはパスワード入力を不要にしよう。
ここまででほとんど完了ですが、最後にSNS認証の時だけパスワードを自動生成するという機能の実装をする

アソシエーションにオプションをつける
取得したFacebook（またはGoogle）の情報を、外部キーが無くても保存できるオプションを付与する

app/models/sns_credential.rb

class SnsCredential < ApplicationRecord
 belongs_to :user, optional: true
end

optional: true とは
レコードを保存する際に、外部キーの値がない場合でも保存ができるオプションのこと

新規登録時は、「nickname」「email」の値をビューに表示させることで、新規登録を行う
しかし、パスワードを非表示にする場合は、SnsCredentialモデルのレコード情報で条件分岐を行う
「user_id」が生成されていないタイミングでも保存する必要がある

Userモデルを編集する
SNS認証を行ったかの判断をするために、snsに入っているsns_idをビューで扱えるようにするため、コントローラーに渡す

app/models/user.rb

class User < ApplicationRecord
 devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable, :omniauthable, omniauth_providers: [:facebook, :google_oauth2]

 has_many :sns_credentials

 def self.from_omniauth(auth)
   sns = SnsCredential.where(provider: auth.provider, uid: auth.uid).first_or_create
   # sns認証したことがあればアソシエーションで取得
   # 無ければemailでユーザー検索して取得orビルド(保存はしない)
   user = User.where(email: auth.info.email).first_or_initialize(
     nickname: auth.info.name,
       email: auth.info.email
   )
   # userが登録済みの場合はそのままログインの処理へ行くので、ここでsnsのuser_idを更新しておく
   if user.persisted?
     sns.user = user
     sns.save
   end
   { user: user, sns: sns }　　　　　　　　　　　　　　　　　　　　　←ここのみ記載
 end
end

SnsCredentialモデルにoptional: trueを付与したので、sns = SnsCredential.where(provider: auth.provider, uid: auth.uid).first_or_createの行で、
外部キーがないレコードが保存される

コントローラーを編集する
Userモデルから送られたデータをビューで扱えるようにする
@userには「nickname」と「email」の情報をもたせて、SNS認証の判断はsns_idで行う為、idだけビューで扱えるようにする

app/controllers/users/omniauth_callbacks_controller.rb

Class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController
 #中略
 def authorization
   sns_info = User.from_omniauth(request.env["omniauth.auth"])　　　　　　←ここから
   @user = sns_info[:user]　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここまで

   if @user.persisted?
     sign_in_and_redirect @user, event: :authentication
   else
     @sns_id = sns_info[:sns].id　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここから
     render template: 'devise/registrations/new'
   end
 end
end

ビューを編集する
ビューでは、SNS認証を行っているか、行っていないかの条件分岐を記述する

app/views/devise/registrations/new.html.erb

<div class="field">
    <%= f.label :birthday %><br />
    <%= f.date_select :birthday, start_year: 1950, end_year: 2019 %>
  </div>

  <div class="field">
    <%= f.label :email %><br />
    <%= f.email_field :email, autofocus: true, autocomplete: "email" %>
  </div>

 <%if @sns_id.present? %>　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここから
   <%= hidden_field_tag :sns_auth, true %>
 <% else %>
   <div class="field">
     <%= f.label :password %>
     <% @minimum_password_length %>
     <em>(<%= @minimum_password_length %> characters minimum)</em>
     <br />
     <%= f.password_field :password, autocomplete: "new-password" %>
   </div>

   <div class="field">
     <%= f.label :password_confirmation %><br />
     <%= f.password_field :password_confirmation, autocomplete: "new-password" %>
   </div>
 <% end %>　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここまで

# 中略

<% end %>

今回はこのコントローラーを上書きするためにcontrollers/users/registrations_controller.rbを作成する
このコントローラー内でcreateアクションの動作を変更する
  
コントローラーを編集する

app/controllers/users/registrations_controller.rb
 
class Users::RegistrationsController < Devise::RegistrationsController
 # before_action :configure_sign_up_params, only: [:create]
 # before_action :configure_account_update_params, only: [:update]


 # GET /resource/sign_up
 # def new
 #   super
 # end

 # POST /resource
 def create　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここから
   if params[:sns_auth] == 'true'
     pass = Devise.friendly_token
     params[:user][:password] = pass
     params[:user][:password_confirmation] = pass
   end
   super
 end　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　←ここまで
                                                                      
rubyでは継承先のクラスでsuperメソッドを使うことで親モデルの同名メソッドをそのまま実行する
devise本来のregistrations#createアクションが実行されるということ。
今回の親モデルは、Userモデルに紐付いているdeviseになる
params[:sns_auth]を取得した時だけDevise.friendly_tokenを使ってパスワードを自動生成する
あとの処理はsuperメソッドでdeviseのregistrations#createを実行させて完成
                                                                      
FacebookのSNS認証に関する必要な設定
                                                                      
1. 有効なリダイレクトURI（FacebookAPI）
2. 環境変数（Heroku）

を次回設定する                                                                      
