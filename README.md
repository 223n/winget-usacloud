# winget-usacloud

[usacloud](https://github.com/sacloud/usacloud)を
[winget](https://github.com/microsoft/winget-cli)で
配信するためのマニフェスト管理リポジトリです。

## 概要

このリポジトリは、
[usacloud（さくらのクラウドCLIクライアント）](https://github.com/sacloud/usacloud)のwinget-pkgsマニフェストを管理し、
GitHub Actionsで自動的に[microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs)へPRを送信します。

## 仕組み

2段構成のワークフロー（`publish.yml`）で自動配信を行っています。

1. **detect job**（毎日18:00 JST）
   - usacloudの最新リリースを確認し、未登録バージョンの場合`workflow_dispatch`で`publish job`を再トリガー
2. **publish job**
   - `wingetcreate`でマニフェストを生成
   - → `winget validate`で検証
   - → ローカルマニフェストでテストインストール
   - → `microsoft/winget-pkgs`へPRを自動送信
   - → 生成したマニフェストをこのリポジトリにコミット

手動実行でバージョンを指定して実行することもできます。

初回登録用に `initial-submit.yml` を用意しています（手動実行のみ）。

## ディレクトリ構成

```text
winget-usacloud/
├── .github/
│   └── workflows/
│       ├── publish.yml                             # 自動検出・生成・PR送信（detect/publishの2段構成）
│       └── initial-submit.yml                      # 初回登録用（手動実行のみ）
├── manifests/                                      # winget-pkgsマニフェスト管理ディレクトリ
│   └── s/sacloud/usacloud/
│       └── <version>/                              # バージョンごとのwinget-pkgsマニフェスト
│           ├── sacloud.usacloud.yaml               # version マニフェスト
│           ├── sacloud.usacloud.installer.yaml     # installer マニフェスト
│           ├── sacloud.usacloud.locale.ja-JP.yaml  # defaultLocale（ja-JP）
│           └── sacloud.usacloud.locale.en-US.yaml  # locale（en-US）
├── .envrc                                          # ローカル環境変数（direnv／git管理外）
├── .gitattributes                                  # 改行コード設定（eol=lf）
├── .gitignore                                      # git管理外ファイル
├── LICENSE                                         # ライセンステキスト（MIT License）
└── README.md                                       # READMEファイル
```

> `manifests/s/sacloud/usacloud/<version>/` 配下はリリースごとに増えていきます。
> （例: `1.21.0`、`1.22.0`、`1.22.1`）
> `<version>` は1ディレクトリ分の構成を示しています。

## セットアップ

リポジトリの`Secrets`（`Settings` > `Secrets and variables` > `Actions`）に
以下を登録してください。

| シークレット名 | 説明                                  |
| -------------- | ------------------------------------- |
| `WINGET_PAT`   | `public_repo`スコープを持つGitHub PAT |

## ライセンス

[MIT License](LICENSE)
