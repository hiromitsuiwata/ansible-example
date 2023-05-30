# ansible-example

## コンテナ

```bash
# ネットワークの作成
podman network create pod-net

# Podの作成
podman pod create --net pod-net --name controller-pod
podman pod create --net pod-net --name target2-pod
podman pod create --net pod-net --name target1-pod

# Ansibleのコントローラーのイメージのビルドと実行
podman build -t ansible-controller container/controller
podman run -d --pod controller-pod --name=controller ansible-controller

# Ansibleで操作されるホストのイメージのビルドと実行
# sshクライアントの公開鍵を取り出してsshサーバのauthorized_keysに配置する形でビルド
podman cp controller:/root/.ssh/id_ed25519.pub container/target/authorized_keys
podman build -t ansible-target container/target

podman run -d --pod target1-pod --name=target1 ansible-target
podman run -d --pod target2-pod --name=target2 ansible-target

ssh target1-pod
ssh target2-pod
ansible all --list-hosts
ansible all -m ping

```

## Vagrant

PowerShellの場合、`vagrant ssh`をしたときに次のエラーで接続に失敗することがある。
vagrant@127.0.0.1: Permission denied (publickey, gssapi-keyex, gssapi-with-mic).
https://github.com/bkenro/centos7dsm/issues/12

以下のように環境変数を設定することで`vagrant ssh`ができるようになる。
```
$Env:VAGRANT_PREFER_SYSTEM_BIN = '0'
vagrant ssh controller
```
