# 営業代行アプリ開発

## カテゴリー

受託開発・Web アプリ

## 担当工程

要件定義・基本設計・実装・テスト

## 担当期間

2022 年 12 月〜2023 年 12 月

## 職種

バックエンドエンジニア・フロントエンドエンジニア

## 資料

- [AWS 構成図・ER 図](https://drive.google.com/file/d/1ne7f9odqxXHBlYsq3vh-3IGxWtzfXA7v/view?usp=sharing)
- [画面モック](https://www.figma.com/file/zSK0Se3KwbBEWRSIBZAq3k/%E7%84%A1%E9%A1%8C?type=design&node-id=0%3A1&mode=design&t=QzlWJD6gaR0p7AH1-1)

## 使用技術

- Nuxt・Vue・Typescript・Pinia：環境構築・共通部品作成・共通コンポーネント作成・　各ページ作成
- Java・SpringBoot・Junit：環境構築・共通部品作成・各種 API 作成・各種バッチ作成・API レベルでの自動テスト
- MySQL：SQL 作成
- Redis：セッション管理に使用
- Docker：AWS ECR に Push する Docker イメージ作成
- AWS：インフラチームが構築したためほとんど触っていない
- GitLabCI：インフラチームが構築したためほとんど触っていない
- Figma：画面モック作成時に使用

## 機能

- 営業リストの作成
- 営業リストへの営業メール配信
  - 営業メールの開封率などの結果確認
- 問い合わせフォームへの配信
  - リンククリック数などの結果確認
- レポート閲覧
  - AB テスト
  - 営業マンごとの営業成績
- 日程調整作業自動化
  - Google・Outlook カレンダーとの連携
  - Zoom・Google Meet・Teams Web 会議サービスとの連携

## チーム構成

【要件定義】

- メンバー 2 名（アドバイザー的立ち位置 1 名）

2 週間ごとにお客様に Figma で作成した画面のモックをお見せして、承認をもらっていました。
業務内容は以下の通りです。

- 画面モック作成
- 技術調査

【基本設計】

- メンバー 2 名（アドバイザー的立ち位置 1 名）

1 週間ごとにお客様に画面の基本設計をお見せして、承認をもらっていました。
業務内容は以下の通りです。

- テーブル設計
- API 設計
- 画面の基本設計

【開発】

- リーダー 1 名（私）
- メンバー 5 名（アドバイザー的立ち位置 1 名）

開発フローは 1 週間ごとのタスクを割り振り、私がレビューするという形でした。
リーダーとして以下の業務を行いました。

- 機能開発
  - API 開発
  - フロントエンドのページ開発
- レビュー
- 質問対応
- テスト
- デプロイ用 Docker ファイル作成

## 実績・取り組み

【フロントエンド開発・共通コンポーネントの作成】

このプロジェクトは業務システムではなく Web サービスであったため UI フレームワークを使うことができなかったためテキストボックス・ページネーションなどを自作する必要がありました。
チームメンバーがフロントエンド開発に入る前に共通コンポーネントを一人で作成しました。各コンポーネントのロジックは Composable 関数、見た目はコンポーネントに記述することでコンポーネントから分離しました。通知コンポーネントのようなページをまたいだデータ保持が必要な場合は、Provide によって最上位コンポーネントに Composable 関数を注入しました。また、通知コンポーネントのように SSR 時に生成されるデータが存在する場合は、Nuxt3 の SSR フレンドリーな useState 関数を使用することでサーバーサイドで生成されたデータをハイドレーション時に復元できるようにもしました。

【フロントエンド開発・モダンな技術の導入】

前職の会社では Vue2・Nuxt2・Javascript・SPA を使用することが多かったが開発後の保守フェーズのことを考え Vue3・Nuxt3・Typescript・SSR を導入しました。しかしチームメンバーはこれらの技術に触れたことがなかったため質問対応が大変でした。useAsyncData という SSR フレンドリーな fetch 関数を使用すると、SSR 時に API を呼び出す場合にサーバー側とクライアント側の両方で API 呼び出しを行ってしまうバグが発生しました。ソースコードを読むことによって useAsyncData に渡す関数が undefined や null を返却する場合はサーバー側でデータを取得できなかったと判定されクライアント側でもう一度 API を呼び出してデータを取得するように実装されていることが原因であるとわかりました。また Pinia という状態管理ライブラリーを使用していたためデータを取得しても useAsyncData にデータをキャッシュする必要がなかったため useAsyncData に渡す関数が undefined を返却していることも原因でした。そこで useAsyncData に渡す関数は必ず取得したデータを返却するようにチームに共有して開発効率を上げることができました。また、質問対応した場合はドキュメントにまとめてチームメンバーに公開することで質問対応に割かれる時間を削減することができました。

【バックエンド開発・外部サービスとの連携】

日程調整機能を作成するには Google カレンダーや Outlook カレンダーのように外部サービスを利用する必要がありました。
しかし各外部サービスごとに異なるライブラリーを使用しなければならないため、素直に実装すると外部サービスを利用する場合必ず条件分岐しなければならなくなってしまいます。またチームメンバーに API 単位でタスクを割り振ると外部サービスを呼び出すメソッドを重複して実装してしまう可能性もありました。
この問題を避けるために先にインターフェースを定義して、外部サービス用のアダプターをチームメンバーに実装してもらい、その後外部サービスを利用する API の実装をしてもらいました。
この取り組みによってチームメンバーの作業効率を上げることができたと思います。
