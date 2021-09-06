# ローカルで立ち上げたReactアプリケーションにHTTPSでアクセスする

1. 秘密鍵の作成
```
openssl genrsa -out server.key 2048
```

2. 証明書署名要求CSRファイルを作成
```
openssl req -sha256 -new -key server.key -out server.csr -subj '/CN=localhost/O=turner-kl.com/C=jp'
```

3. サブジェクト別名SANの設定をするためのファイルを作成する
```
echo 'subjectAltName = DNS:localhost' > san.config
```

4. 自己署名証明書を作成する
```
openssl x509 -req -sha256 -days 36500 -in server.csr -signkey server.key -out server.crt -extfile san.config
```

5. 自己署名SSL証明書をMacBookで信頼する

6. ReactをHTTPSで動作させる
- 環境変数HTTPSをtrueに設定する
- 環境変数SSL_CRT_FILEに証明書ファイルのファイルパスを設定する
- 環境変数SSL_KEY_FILEに秘密鍵ファイルのファイルパスを設定する
