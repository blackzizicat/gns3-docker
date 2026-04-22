# GNS3 環境構築手順

## 構成

- GNS3サーバ: Dockerコンテナ (`gns3/gns3-server`)
- GNS3 GUI: Windows インストール
- IOS エミュレータ: Dynamips (コンテナ内蔵)

```
Windows 11
├── GNS3 GUI (Windows)
└── Docker Desktop (WSL2バックエンド)
    └── gns3server コンテナ
        ├── Dynamips
        └── /root/GNS3/images/IOS/ (IOSイメージ)
```

## ディレクトリ構成

```
gns3/
├── README.md
├── docker-compose.yml
└── ios-images/
    ├── c3725-adventerprisek9-mz124-15.bin
    ├── c3745-advipservicesk9-mz.124-25d.bin
    └── c7200-advipservicesk9-mz.152-4.S5.bin
```

## 前提条件

- Windows 11
- WSL2 インストール済み
- Docker Desktop (Windows) インストール済み

## 手順

### 1. GNS3サーバの起動

```bash
cd C:\Users\kshinjo\work\gns3
docker compose up -d
```

停止する場合:

```bash
docker compose down
```

### 2. GNS3 GUI のインストール

gns3.com から Windows用 All-in-one インストーラをダウンロードしてインストールする。

### 3. GNS3 GUI の初期設定

初回起動時のセットアップウィザードで以下を設定する:

- **「Run on a remote server」** を選択
- Host: `localhost`
- Port: `3080`
- Enable authentication: **チェックしない**

### 4. IOSイメージの登録

GNS3 GUI から以下の手順で3機種を登録する:

1. Edit → Preferences → Dynamips → IOS routers → New
2. IOSイメージファイルを指定 (コンテナ内パス: `/root/GNS3/images/IOS/`)
3. idlePC値を計算して設定

## 対応IOSイメージ

| モデル | ファイル名 |
|--------|-----------|
| c3725 | c3725-adventerprisek9-mz124-15.bin |
| c3745 | c3745-advipservicesk9-mz.124-25d.bin |
| c7200 | c7200-advipservicesk9-mz.152-4.S5.bin |

## 備考

- IOSイメージはライセンスの関係でDockerイメージに含めることができない。環境移行時は `ios-images/` ディレクトリごとコピーする。
- `gns3-data` という名前のDockerボリュームにGNS3のプロジェクトや設定が保存される。
