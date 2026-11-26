<img src="images/Monaca_store.png" align="right" width="130" alt="Monaca Mascot" />

# TermAddIn_LxiEvent - LXI Event Messaging & Event Logs 拡張エクステンション

👉 [← TermAddIn 統合本体に戻る](https://github.com/Monaca-App/TermAddIn)  
👉 [← Monaca アプリ工房（トップ）に戻る](https://github.com/Monaca-App/store)

---

[![Platform](https://img.shields.io/badge/Platform-Windows%2010%20%2F%2011-blue.svg)](https://www.microsoft.com/)
[![Excel](https://img.shields.io/badge/Excel-2016%20%2F%202019%20%2F%202021%20%2F%20M365-green.svg)](https://www.microsoft.com/excel)
[![Framework](https://img.shields.io/badge/.NET%20Framework-4.8-purple.svg)](https://dotnet.microsoft.com/)
[![Architecture](https://img.shields.io/badge/Architecture-TermAddIn%20Extension-orange.svg)]()
[![Status](https://img.shields.io/badge/Status-Planning%20(Phase%204)-yellow.svg)]()

## 📖 概要

**TermAddIn_LxiEvent** は、Excel 総合通信アドイン **TermAddIn** 上で動作する **LXI 拡張機能（Extended Functions）の LXI Event Messaging および LXI Event Logs 専用エクステンション（`TermAnalysis.LxiEvent`）** です。

BNC トリガケーブルなどのハードウェア配線を敷設することなく、Ethernet（LAN）上のマルチキャスト UDP（標準 `224.0.23.159:5044`）およびユニキャスト TCP を用いて、 **複数 LXI 計測器へのイベント同期トリガ送出（`LAN0`〜`LAN7`・カスタム EventID）、機器側イベントの非同期受信待機、および LXI Event Logs の時系列取得** を Excel シーケンスから実行できます。

> [!NOTE]
> **提供ステータスおよび利用前提（企画中・Phase 4 リリース予定）**  
> 本エクステンションは現在 **企画・設計段階（Phase 4 実装予定）** であり、[TermAddIn 本体](https://github.com/Monaca-App/TermAddIn) 上で動作する選択購入型の独立エクステンションとして提供予定です。  
> 予定価格は **`TermAddIn_LxiEvent` 個別ライセンス（一括 ¥32,000 / 月額 ¥3,000 / 週額 ¥1,000・税別・予定）** を想定しています（ライセンス未配置時は 10 分間の期間限定試用モードで動作予定）。

---

## ✨ 主な機能・特徴

1. **LXI 標準 LAN イベントパケットの Pure C# 構築・解析**
   * LXI 規格フォーマット（マジック `"LXI"` ＋ Domain ＋ EventID 16B［`LAN0`〜`LAN7` 等］＋ SequenceNumber ＋ TimestampSeconds / Nanoseconds ＋ Flags ＋ 任意ペイロード）を `.NET` 標準の `UdpClient` / `TcpClient` で完結処理します。
2. **配線レスでの複数機器イベント同期トリガ ＆ 非同期待機**
   * PC から複数台の LXI 計測器へ LAN イベントを一斉送出できるほか、計測器が発報した測定完了イベントやアラームイベントをタイムアウト付きで非同期待機し、受信時刻・送信元 IP・データを変数へ格納して分岐制御できます。
3. **LXI Event Logs の自動取得・Excel 時系列展開**
   * LXI Event Logs 対応機器（`http(s)://<ip>/lxi/eventlog` 等）からイベント履歴を XML 取得し、検査エビデンスやトラブル解析用として Excel シート・ログファイルへ展開します。
4. **PTP 未来時刻指定トリガ（`LxiSync`）とのシームレス連携**
   * [TermAddIn_LxiSync](https://github.com/Monaca-App/TermAddIn_LxiSync) で算出した未来タイムスタンプを `LxiEvent_Send` に渡すことで、LAN 遅延の揺らぎに左右されない高精度な同時計測トリガを構築できます。
5. **`TermEmulator` 仮想 LAN Event シミュレータ連携**
   * **[TermEmulator](https://github.com/Monaca-App/TermEmulator)** の `LxiEvent` 仮想スレーブ（UDP マルチキャスト参加・模擬イベント送出・イベントログ蓄積、`TermAddIn_LxiEvent.License` 相互共用）および **[c2c](https://github.com/Monaca-App/c2c)** の UDP モニターにより、実機なしでイベント連携シーケンスを検証できます。

---

## 🔌 提供予定コマンド一覧（`LxiEvent_*`）

| コマンド名 | 主な役割・用途 |
| :--- | :--- |
| **`LxiEvent_Open`** | 指定ドメイン・マルチキャスト UDP（既定 `224.0.23.159:5044`）または TCP ポートでイベントリスナーを開始 |
| **`LxiEvent_Close`** | イベントリスナーを安全に停止し、マルチキャストグループを離脱 |
| **`LxiEvent_Send`** | 指定 `EventID`（`LAN0`〜`LAN7` やカスタム名）、フラグ、タイムスタンプ（即時／未来時刻）、任意データを送出 |
| **`LxiEvent_Wait`** | 指定 `EventID` の受信をタイムアウト付きで非同期待機し、受信タイムスタンプ・送信元 IP・ペイロードを変数へ格納（タイムアウト分岐対応） |
| **`LxiEvent_GetLog`** | 指定 IP の LXI 計測器から LXI Event Log を取得して出力・セル展開 |

---

## 💻 動作環境・システム要件

| 項目 | システム要件 |
| :--- | :--- |
| **OS** | Windows 10 / Windows 11 (64bit / 32bit) |
| **対応 Excel** | Microsoft Excel 2016 / 2019 / 2021 / Microsoft 365 (デスクトップ版 32bit / 64bit) |
| **必須アドイン** | [TermAddIn](https://github.com/Monaca-App/TermAddIn) (Excel VSTO アドイン) |
| **ランタイム** | .NET Framework 4.8 / Visual Studio 2010 Tools for Office Runtime（※ NI-VISA / Bonjour 不要） |
| **インターフェース** | Ethernet (UDP マルチキャスト `224.0.23.159:5044` / TCP・IPv4/IPv6 対応) |
| **対応規格** | LXI Event Messaging Extended Function / LXI Event Logs Extended Function |

---

## 🔗 LAN（LXI 規格）独立エクステンション・ラインナップ

| カテゴリ | エクステンション | 主な対象プロトコル・用途 | 予定価格（税別・一括） | ページ |
| :---: | :--- | :--- | :---: | :---: |
| **基本通信 ①** | **`TermAddIn_LxiRaw`** | SCPI Raw Socket (`5025`) ＋ バイナリブロック転送 | ¥32,000 | [詳細を見る](https://github.com/Monaca-App/TermAddIn_LxiRaw) |
| **基本通信 ②** | **`TermAddIn_LxiVxi11`** | VXI-11 (ONC RPC / `111`) ＋ 帯域外 STB・Clear・Trigger・Lock | ¥49,000 | [詳細を見る](https://github.com/Monaca-App/TermAddIn_LxiVxi11) |
| **基本通信 ③** | **`TermAddIn_LxiHiSlip`** | HiSLIP (`4880` / 2ch) ＋ LXI 1.6 TLS 2.0 高速波形転送 | ¥49,000 | [詳細を見る](https://github.com/Monaca-App/TermAddIn_LxiHiSlip) |
| **LXI 拡張 ①** | **`TermAddIn_LxiEvent`** *(本ページ)* | LXI Event Messaging (UDP `5044` / TCP) ＆ Event Logs | ¥32,000 | 本ページ |
| **LXI 拡張 ②** | **`TermAddIn_LxiSync`** | IEEE 1588 PTPv2 時刻同期監視 ＆ Wired Trigger Bus (WTB) | ¥49,000 | [詳細を見る](https://github.com/Monaca-App/TermAddIn_LxiSync) |
| **運用拡張 ③** | **`TermAddIn_LxiMulti`** | 複数 LXI 計測器の同時セッション・エイリアス管理・並列クエリ | ¥32,000 | [詳細を見る](https://github.com/Monaca-App/TermAddIn_LxiMulti) |

---

## 💳 ライセンス・予定価格プラン（TermAddIn_LxiEvent）

> [!NOTE]
> **リリース準備中（予定価格）**  
> 本エクステンションは Phase 4 として開発予定です。以下は設計検討段階の予定価格（税別）であり、正式リリース時にインストーラーおよびオンライン決済リンクを公開します。

| プラン | 予定価格（税別） | ステータス |
| :--- | :---: | :---: |
| **一括購入（買い切りライセンス）**<br>無期限で永続利用できるライセンスです。 | **¥32,000** | 準備中（Phase 4 予定） |
| **月額サブスクリプション**<br>月単位で利用できる定期プランです。 | **¥3,000** / 月 | 準備中（Phase 4 予定） |
| **週額サブスクリプション**<br>短期評価や単発テスト向けのプランです。 | **¥1,000** / 週 | 準備中（Phase 4 予定） |

* ※ **ライセンスファイル名（予定）**: `TermAddIn_LxiEvent.License`
* ※ **配置先**: `%USERPROFILE%\Desktop\Monaca\TermAddIn_LxiEvent.License`（`TermEmulator` 対応仮想スレーブと相互共用可能）
* ※ **ライセンス形態**: 1 ユーザー / 1 PC（ノードロック方式：`PC名` および `ログインユーザー名` に紐づけ）

---

## 📄 ライセンス & サポート

* **開発元**: [Monaca アプリ工房 (Monaca App Store)](https://github.com/Monaca-App/store)
* **親プロジェクト**: [TermAddIn](https://github.com/Monaca-App/TermAddIn)
* **Copyright**: Copyright (C) Monaca App Store 2026 All rights reserved.
* **お問い合わせ**: 不具合報告や機能要望は、GitHub の Issue までお気軽にお寄せください。
