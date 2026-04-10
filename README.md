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

2段構成のワークフロー（`publish.yml`）で自動配信を行っています。

1. **detect job**（毎日18:00 JST）: usacloudの最新リリースを確認し、未登録バージョンの場合`workflow_dispatch`でpublish jobを再トリガー
2. **publish job**: `wingetcreate`でマニフェストを生成し、`microsoft/winget-pkgs`へPRを自動送信、生成したマニフェストをこのリポジトリにコミット

手動実行でバージョンを指定して実行することもできます。

初回登録用に `initial-submit.yml` も用意しています（手動実行のみ）。

## ディレクトリ構成

```text
manifests/s/sacloud/usacloud/
└── <version>/
    ├── sacloud.usacloud.yaml
    ├── sacloud.usacloud.installer.yaml
    ├── sacloud.usacloud.locale.en-US.yaml
    └── sacloud.usacloud.locale.ja-JP.yaml
```

## セットアップ

リポジトリのSecrets（Settings > Secrets and variables > Actions）に
以下を登録してください。

| シークレット名 | 説明                                  |
| -------------- | ------------------------------------- |
| `WINGET_PAT`   | `public_repo`スコープを持つGitHub PAT |

## ライセンス

[MIT License](LICENSE)
