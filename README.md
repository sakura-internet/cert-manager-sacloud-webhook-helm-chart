# さくらのクラウド対応 Cert manager プラグイン 📦

![](https://img.shields.io/badge/version-v1.0.0-green)

さくらのクラウドに構築したKubernetes上で最新版のCert managerが利用可能となります。以下の手順で開発したWebhookプラグインのインストールすることでCert managerがさくらのクラウドに対応します。

## インストール

### Cert manager

まず最初に以下の手順でCert managerをインストールします(バージョンは定義更新していただいて問題ありません)。

```bash
$ kubectl create namespace cert-manager
$ helm repo add jetstack https://charts.jetstack.io
$ helm repo update

$ kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.crds.yaml

$ helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.0.4
```

### Webhookプラグイン

次にHelmチャートとしてCert managerのwebhookプラグインをインストールします。

```bash
$ helm repo add cert-manager-sacloud-webhook https://sakura-internet.github.io/cert-manager-sacloud-webhook-helm-chart/
$ helm install cert-manager-sacloud-webhook cert-manager-sacloud-webhook/cert-manager-sacloud-webhook --namespace cert-manager --version v1.0.0
```

### テスト

ここからテストとしてNginxをデプロイします。Issuer及びCertificateリソースを作成し証明書のデータを含んだSecretリソースを指定しTLS通信を行います(以下のyamlは適宜変更が必要です)。

```bash
$ kubectl apply -f examples/cert-manager-sacloud-webhook/issuer.yaml
$ kubectl apply -f examples/cert-manager-sacloud-webhook/certificate.yaml
$ kubectl apply -f examples/cert-manager-sacloud-webhook/nginx.yaml
```

## 開発

以下のコマンドでHelmチャートにLintをかけパッケージングを行います。

```bash
$ helm lint charts/cert-manager-sacloud-webhook
$ helm package charts/cert-manager-sacloud-webhook
```

上記のコマンド実行後`charts/cert-manager-sacloud-webhook-<VERSION>.tgz`が生成されます。

# 参考

- https://cert-manager.io/docs/concepts/webhook/
- https://helm.sh/docs/topics/chart_repository/

