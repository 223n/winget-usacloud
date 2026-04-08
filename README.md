# winget-usacloud

[usacloud](https://github.com/sacloud/usacloud)を
[winget](https://github.com/microsoft/winget-cli)で配信するための
マニフェスト管理リポジトリです。

## 概要

このリポジトリは、usacloud（さくらのクラウドCLIクライアント）の
winget-pkgsマニフェストを管理し、GitHub Actionsで自動的に
[microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs)へ
PRを送信します。

## 仕組み

1. 毎日18:00（JST）にusacloudの最新リリースを確認
1. 新バージョンが検出された場合、`wingetcreate`でマニフェストを生成
1. `microsoft/winget-pkgs`へPRを自動送信
1. 生成したマニフェストをこのリポジトリにコミット

手動実行でバージョンを指定して実行することもできます。

## ディレクトリ構成

```text
manifests/s/sacloud/usacloud/
└── <version>/
    ├── sacloud.usacloud.yaml
    ├── sacloud.usacloud.installer.yaml
    └── sacloud.usacloud.locale.en-US.yaml
```

## セットアップ

リポジトリのSecrets（Settings > Secrets and variables > Actions）に
以下を登録してください。

| シークレット名 | 説明                                  |
| -------------- | ------------------------------------- |
| `WINGET_PAT`   | `public_repo`スコープを持つGitHub PAT |

## ライセンス

[MIT License](LICENSE)
