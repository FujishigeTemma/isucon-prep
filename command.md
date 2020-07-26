# Makeコマンドヘルプ

## 初期設定

DBの認証情報を変更してください。
```
DB_HOST:= ##DBホストアドレス##
DB_PORT:= ##DBポート番号##
DB_USER:= ##DBユーザー##
DB_PASS:= ##DBパス##
DB_NAME:= ##DBネーム##
```

プロジェクトのルートパスを設定してください。
`PROJECT_ROOT:= ##プロジェクトルートディレクトリ##`

ビルド先の設定をしてください。
`BUILD_DIR:= ##バイナリ生成先##`
`BIN_NAME:= ##生成バイナリ名##`

## コマンド一覧

### セットアップ
`make setup`を実行後、赤字の指示に従って
- データのバックアップ
- Nginxのログ出力設定
- ssh-keygen
- GitHubへの公開鍵の追加
- slackcat接続

を行ってください。

### ベンチループ
1. masterにマージされる。
1. `make update`を実行し、レスポンスコードが正常か確認する。
1. `dstat`,`htop`,`make bench`を起動し、ベンチを回す。
1. ベンチが回り終えたら`make analytics`で、各種分析結果をSlackへ送信。
1. この時、スコアが良ければ`make tag TAG={{ベンチスコア}}`でtagを打っておく。

### 各種コマンド
- `update`: `pull build restart curl`を行う
	- `pull`: pullとgo modulesのダウンロードを行う
- `build`: コードをビルドする
- `restart`: アプリケーションをリスタートする
- `curl`: ヘルスチェック
- `status`: アプリケーションのステータス確認
- `rollback`: 指定したコミットの状態に戻す
	- `reset`: `git reset --hard`
- `bench`: `stash-log log`を行う
- `stash-log`: アクセスログとスロークエリログを`~/logs`以下に移行し、nginxとmysqlをリスタートする
- `log`: `journalctl`
- `tag`: 現在のブランチの最新のコミットにタグを打つ
- `analytics`: `kataru dumpslow digestslow pprof`を行う
- `kataru`: Nginxのアクセスログをkataribeで分析し、結果をSlackのkataribeチャンネルに送信する
- `dumpslow`: スロークエリログをmysqldumpslowで分析し、結果をSlackのslowqueryチャンネルに送信する
- `digestslow`: スロークエリログをpt-query-digestで分析し、結果をSlackのslowqueryチャンネルに送信する
- `pprof`: 現在のコードをpprofで分析した結果を、Slackのpprofチャンネルに送信する
- `slow-on/off`: スロークエリログの出力をON/OFFする
- `prune`: `stash-log slow-off pull build curl`を行う