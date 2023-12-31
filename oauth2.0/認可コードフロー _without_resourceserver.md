```mermaid

sequenceDiagram
    autonumber

    participant User as User
    participant Client as Client <br>アプリケーション<br>(React.js)
    participant AuthorizationServer as 認可サーバー＆OpenID Provider<br>(Auth0)

    User ->> Client: ログイン/Sign Upボタンを押す
    Client ->> Client: code_challengeとcode_verifier を生成

    Client ->> AuthorizationServer: 認証エンドポイント(/authorize)へリクエスト<br>with code_challenge,code_challenge_method,その他パラメーター
    AuthorizationServer ->> AuthorizationServer: code_challenge,code_challenge_methodを保存
    AuthorizationServer ->> User: ログイン画面にredirect
    User ->> AuthorizationServer: 認証とアクセス許可の同意を行なう
    AuthorizationServer ->>　Client: return 認可コード(code)
    Client ->>　AuthorizationServer: tokenエンドポイントにアクセス(/token) <br>with 認可コード(code),code_verifier,その他パラメーター<br>(アクセストークンとidトークンを取得リクエスト)
    AuthorizationServer ->> AuthorizationServer: code_verifierをcode_challenge_methodで検証し、<br>保存していたcode_challengeになるかをチェック
    AuthorizationServer ->> Client: return id_token, access_token
```