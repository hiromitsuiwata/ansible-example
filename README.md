# ansible-example

PowerShellの場合、`vagrant ssh`をしたときに次のエラーで接続に失敗することがある。
vagrant@127.0.0.1: Permission denied (publickey, gssapi-keyex, gssapi-with-mic).
https://github.com/bkenro/centos7dsm/issues/12

以下のように環境変数を設定することで`vagrant ssh`ができるようになる。
```
$Env:VAGRANT_PREFER_SYSTEM_BIN = '0'
vagrant ssh controller
```
