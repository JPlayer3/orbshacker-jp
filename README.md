<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0a0a0a,60:0d1f0d,100:1a4a1a&height=200&section=header&text=orbshacker&fontSize=70&fontColor=4ade80&fontAlignY=55&animation=fadeIn" width="100%"/>

<br/>

[![Python](https://img.shields.io/badge/Python-3.7+-3572A5?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Platform](https://img.shields.io/badge/Platform-Windows%20Only-555555?style=for-the-badge&logo=windows&logoColor=white)](https://github.com/DanielPires2000/orbshacker)
[![Discord](https://img.shields.io/badge/Discord-Game%20Spoofer-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.com)
[![License](https://img.shields.io/badge/License-GPL%20v3-c0392b?style=for-the-badge&logo=opensourceinitiative&logoColor=white)](./LICENSE)
[![Version](https://img.shields.io/github/v/release/DanielPires2000/orbshacker?style=for-the-badge&logo=semanticrelease&color=4ade80&logoColor=white)](https://github.com/DanielPires2000/orbshacker/releases)

<br/>

*だって、オーブのためだけに500GBのゲームをインストールする時間なんてないでしょ。*

<br/>

[使い方](#installation) &nbsp;·&nbsp; [仕組み](#how-it-works) &nbsp;·&nbsp; [Steamモード](#steam-quest-mode) &nbsp;·&nbsp; [使用方法](#usage) &nbsp;·&nbsp; [構成](#project-structure) &nbsp;·&nbsp; [法的注意](#legal-notice)

</div>

<br/>

<div align="center">
<img width="340" alt="image" src="https://github.com/user-attachments/assets/1db237b0-2f57-428f-a004-d707f98a416e" style="border-radius: 16px"/>
</div>

## これは何か

orbshacker は、実際のゲームをインストールせずに Discord Orb クエスト向けの偽のゲームプロセスを作成する Windows ツールです。Discord の公開 API から Discord が期待する正確なプロセス名を取得し、ベース実行ファイルをコピーしてリネームし、バックグラウンドで起動します。Discord はプロセス一覧をスキャンし、必要なプロセスを見つけるとクエストを有効化します。

クライアント改変なし。コード注入なし。怪しいネットワーク通信なし。Discord が確認するのはタスクリスト上のプロセス名だけです。

> **教育目的のみ。** このツールは Discord のゲーム検出システムの仕組みを学び、プロセス操作技術を調査するために提供されています。自己責任で、該当する利用規約に従って使用してください。

<br/>

## 🚨 Steam Quest Mode

一部のゲームは、より高度な検出方法を使っています。Discord はプロセス名だけでなく、Steam がそのゲームのダウンロードを登録済みかどうかも確認します。通常のスポーフィングでは動作しません。Steam Quest Mode なら対応します。

### 仕組み

ツール内でゲーム名を直接検索します。ツールは SteamCMD の公開 API からメタデータを取得し、インストール先ディレクトリ、実行ファイルパス、デポ情報を読み取ります。さらに、Windows レジストリから自動的に Steam ID を取得します。`steamapps/` フォルダに、Steam がダウンロード中に作成するのと同じ形式の偽 `appmanifest_<appid>.acf` ファイルを生成し、`StateFlags 1026`、`LastOwner`、`StagedDepots` などの現実的な値を入れます。偽の実行ファイルは `steamapps/common/<game>/` に直接置かれます。Discord はフォルダをスキャンし、マニフェストを検出し、プロセスが動作していると判断してクエストを有効にします。終了後は自動でクリーンアップします。

**対応:**
`Steam マニフェストを必要とするゲームすべて` &nbsp; `完全自動、手動 AppID 検索不要` &nbsp; `実際の Steam ID を使用` &nbsp; `デモとフルゲームを別々に検索` &nbsp; `終了時に自動クリーンアップ`

> **ヒント:** クエストがデモを対象としている場合は、`"Toxic Commando Demo"` を検索してください。`"Toxic Commando"` では異なる AppID となり、クエストが発動しません。

<br/>

## 特徴

**自動ゲーム検出** は、Discord の公式 API から最新の検出可能なゲームリストを取得します。名前や略称でゲームを検索でき、PUBG、LoL、CSGO などが見つかります。自動起動はすべてバックグラウンドで処理します。

**自己実行タイマー & 埋め込み設定** は、偽のゲームプロセス（スポーファー実行ファイルのリネームコピー）を作成し、ユーザーがダブルクリックするとカウントダウンタイマーを直接実行します。カスタム時間と自動削除設定はバイナリ内部に埋め込まれます。偽プロセスにはコンソールウィンドウが表示されません。

**自動自己破壊 (`AUTO_DELETE`)** は、カウントダウンが終了するとバックグラウンドで偽の実行ファイル、親フォルダ、生成された Steam マニフェストをすべてクリーンアップします。

**マルチゲーム対応** により、複数の偽プロセスを同時に実行して、すべてのオーブクエストを一度に完了できます。ゲームを起動し、Enter を押して別のゲームを選択し、必要なだけ繰り返します。各プロセスは独立して動作し、Discord はそれらすべてを検出します。

**バックアップデータベース** は、Discord の API が利用できない場合に GitHub アーカイブにフォールバックするため、主要ソースが落ちていてもツールは動作を続けます。

**マニュアルモード** は、データベースにないものをスポーフィングしたい場合にカスタム実行ファイル名の指定に対応します。

**美しいインターフェース** は、読み込みアニメーション付きのカラー端末 UI です。プレーンなテキストよりも見やすくなっています。

<br/>

## なぜこの方法が機能するのか

Discord のゲーム検出は Windows のプロセス一覧を読み取ります。`TslGame.exe` があれば PUBG をプレイしていると判断します。そのプロセスが実際のゲームか、リネームされた実行ファイルかを検証する技術的な仕組みはありません。Discord がチェックするのは名前だけです。

この方法を検出するには、Discord は Valorant の Vanguard に匹敵するカーネルレベルのアンチチートを導入する必要があります。プライバシー問題や「軽量チャットアプリ」であるという約束の崩壊も伴います。見た目だけのオーブクエストにそこまでの対応がされることはありません。

**これは何ではないか:** このツールは Discord のクライアントにコードを注入したり、ファイルを改変したり、偽の API リクエストを送信したりしません。そのような方法には痕跡が残ります。Discord は JavaScript が改ざんされた場合を検出できます。本アプローチでは Discord クライアントを完全に無改変のままにします。ツールは Discord の公開 API を使ってゲームリストを取得します。クライアント改変なし。整合性の侵害なし。

<br/>

## 必要条件

Python 3.7 以上、Windows のみ対応。データベース取得にはインターネット接続が必要です。Discord が実行中で、Discord がプロセスをスキャンしている状態でのみスポーファーは動作します。

<br/>

## インストール

```bash
git clone https://github.com/DanielPires2000/orbshacker.git
cd orbshacker
pip install -r requirements.txt
```

<br/>

## 使用方法

```bash
python orbshacker.py
```

またはパッケージエントリポイントを使って:

```bash
python -m orbshacker
```

### メニューの選択肢

`1` Discord データベースを名前または略称で検索

`2` マニュアルモード、カスタム実行ファイル名を入力

`3` Steam 特殊クエストモード

`4` クレジットとプロジェクト情報

`5` 終了

### 15分で全クエストを完了する

ツールを起動します。最初のゲームを選択します。プロセスが起動したら Enter を押してメインメニューに戻ります。別のゲームを選択します。ウィンドウを複数開く必要はありません。すべての偽プロセスは並行して実行されます。Discord はそれらを同時に検出します。15分待って、完了したらすべて閉じてください。

<br/>

## 仕組み

ツールは Discord の公式 API (`/api/v9/applications/detectable`) に接続してライブのゲームリストを取得します。Discord が各ゲームに対して期待する正確なプロセス名を抽出します。スポーファー実行ファイル（またはソースモードではベースの Python インタープリタ）を設定済みフォルダ（デフォルトは `Desktop/Win64/`）にコピーし、ゲームの実行ファイル名に合わせてリネームし、アクティブな設定を直接埋め込みます。

偽のゲーム実行ファイルが動作すると、埋め込まれた設定付きのスタンドアロンのカウントダウンタイマーとして振る舞います。カウントダウンが終了すると、自動削除(`AUTO_DELETE` が有効な場合) 用のバックグラウンド自己破壊スクリプトが自動で起動し、偽ファイルと空の親ディレクトリを削除します。

Steam Quest Mode はさらに一歩進み、`steamapps/` に偽の `appmanifest_<appid>.acf` を生成し、実行ファイルを `steamapps/common/<game>/` に配置します。これにより、Marathon や Toxic Commando のようなゲームで必要な追加のマニフェストチェックを満たします。

<br/>

## プロジェクト構成

```
orbshacker/
├── orbshacker.py          Main entry point
├── orbshacker/
│   ├── __init__.py        Version and author metadata
│   ├── __main__.py        Package entry point, --timer-mode support
│   ├── config.py          Centralized configuration with settings.py overrides
│   ├── faker.py           Fake executable creation and launch logic
│   ├── discord_db.py      Game database loading, search, and selection
│   ├── steam.py           Steam registry helpers and manifest generation
│   ├── updater.py         Auto-update from GitHub releases
│   ├── net.py             HTTP helpers
│   ├── ui.py              Terminal colors, animations, prompts
│   └── errors.py          Custom exception hierarchy
├── tests/                 pytest coverage for pure helpers
├── settings.py            User-editable configuration
├── requirements.txt
└── .github/
    └── workflows/
        └── release.yml    PyInstaller build and GitHub Release automation
```

<br/>

## 設定

ユーザー編集可能な値はプロジェクトルートの `settings.py` にあります。起動時にこのファイルが読み込まれ、`orbshacker/config.py` のデフォルト設定を上書きします。ランタイムの設定や API タイムアウトはここで管理します。アプリケーションのバージョンはビルドに使用された git タグから取得されるため、手動変更は更新検出を壊す可能性があります。

<br/>

## 自動アップデーター

新しいバージョンタグがプッシュされると、GitHub Actions は PyInstaller を使ってスタンドアロンの Windows 実行ファイルをビルドし、GitHub Release として公開します。ツールは起動時に更新をチェックし、新しいバイナリをダウンロードして差し替え、自動的に再起動します。配布された実行ファイルを実行するのに Python インストールは不要です。

<br/>

## 法的注意

**教育目的のみ。商用利用禁止。**

このツールは、Discord のゲーム検出システムの仕組みを学び、プロセス操作技術を調査するための教育および研究目的でのみ提供されています。商用利用、配布、販売は厳禁です。

ユーザーは、適用されるすべての法律、Discord の利用規約、およびその他の関連契約を遵守する責任があります。開発者は悪用を容認せず、本ソフトウェアの使用に起因するいかなる結果についても責任を負いません。保証や保証は一切ありません。自己責任で使用してください。

このツールの悪用は Discord の利用規約に違反する可能性があります。

<br/>

## ライセンス

GPL v3。帰属表示が必要です。改変版も GPL v3 である必要があります。配布にはソースコードの提供が必要です。商用利用は厳禁です。詳細は [LICENSE](./LICENSE) を参照してください。

<br/>

<div align="center">

疑わしい人生の選択で作られました by **Strykey**

<br/>

[![GitHub stars](https://img.shields.io/github/stars/DanielPires2000/orbshacker?style=for-the-badge&color=4ade80&labelColor=1a1a1a)](https://github.com/DanielPires2000/orbshacker/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/DanielPires2000/orbshacker?style=for-the-badge&color=4ade80&labelColor=1a1a1a)](https://github.com/DanielPires2000/orbshacker/network)

<br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:4ade80,40:0d1f0d,100:0a0a0a&height=120&section=footer" width="100%"/>

</div>
