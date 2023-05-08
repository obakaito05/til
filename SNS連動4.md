SNS認証を本番環境で利用するための設定


ここまでできればあとは本番環境で利用するための設定を行います
1. FacebookのSNS認証に関する必要な設定
2. GoogleのSNS認証に関する必要な設定

先にFacebookのSNS認証に関する必要な設定を行います
1. 有効なリダイレクトURI（FacebookAPI）
2. 環境変数（Heroku）
この二点を行います

facebook for developersのリンクを開き、右上のマイアプリをクリックする
作成したアプリをクリック
facebookログインのプルダウンより設定をクリックする
その後、「有効なOAuthリダイレクトURI」に追記し、保存します
記載するアプリのURL↓
https://【アプリのURL】/users/auth/facebook/callback

後半の「/users/auth/facebook/callback」は、app/controllers/usersディレクトリにある
omniauth_callbacks_controllerのfacebookメソッドに処理を渡すため

ここまでできれば
次に環境変数を設定する
FacebookのAPIとやりとりを行うために必要な「アプリID」と「app secret」を、
ローカル環境と同様の形で呼び出せるよう、Heroku上に設定する

ターミナルにて
% heroku config:set FACEBOOK_CLIENT_ID='アプリID'
% heroku config:set FACEBOOK_CLIENT_SECRET='app secret'

入力後設定が正しくできているか確認

ターミナル
% heroku config

最後にHeroku上で挙動を確認
入力後設定が正しくできているか確認
ローカル環境と同様の挙動が確認できれば設定完了

次はGoogleのSNS認証に関する必要な設定
1. 承認済みのリダイレクトURI（GoogleAPI）
2. 環境変数（Heroku）

承認済みのリダイレクトURIを設定
https://console.cloud.google.com/apis/credentials
Facebookと同様に作成した認証情報を選択してクリック
「承認済みのリダイレクトURI」に追記し、保存
記載する内容↓
https://【HerokuアプリのURL】/users/auth/google_oauth2/callback
後半の「/users/auth/google_uauth2/callback」は、app/controllers/usersディレクトリにある、
omniauth_callbacks_controllerのgoogle_uauth2メソッドに処理を渡すため

ここまでできれば環境変数を設定

ターミナル
% heroku config:set GOOGLE_CLIENT_ID='クライアントID'
% heroku config:set GOOGLE_CLIENT_SECRET='クライアントシークレット'

できているかをターミナルにて確認
ターミナル
% heroku config

これで以上になります
