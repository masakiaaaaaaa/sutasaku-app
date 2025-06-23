# 学習支援Webアプリ「スタサク」

**学習を冒険に変える。生徒の「やりたい！」を引き出すゲーミフィケーションプラットフォーム**

![SUTASAKU Demo](./sutasaku_demo.gif)
*(デモGIF：出席登録→ポイント獲得→ガチャを引く→アバターが変わる、という一連の流れを見せる)*

## 📜 プロジェクト概要
このプロジェクトは、私がアルバイトとして勤務する個別指導塾の「生徒の学習モチベーション維持」と「講師の指導効率化」という2つの課題を解決するために、企画・設計・開発の全てを独力で行ったWebアプリケーションです。生徒は日々の学習活動を通じてポイントを獲得し、アバターのカスタマイズやランキング、イベントへの参加といったゲーム要素を楽しみながら、自律的な学習習慣を身につけることができます。

## ✨ 主要機能
提供された「スタサク 公式ガイドブック」に基づき、以下の多彩な機能を実装しています。

- **👤 ユーザー管理**: 生徒・保護者・管理者アカウント、OTP/招待コードによる新規登録、パスワードリセット
- **✅ 出席管理**: NFC/OTPによる簡単出席登録、遅刻・宿題達成度の記録
- **🎮 ゲーミフィケーション**:
  - **ポイント/ランクシステム**: 学習活動に応じたポイント付与と、努力を可視化するランク制度
  - **ガチャ機能**: 20種類以上のアバターガチャ、ランキング装飾ガチャ（ランクによる排出率変動あり）
  - **ランキング機能**: 出席・宿題など複数部門での週間/月間/累計ランキング
  - **イベント機能**: 全員参加の「ボスバトル」、チーム対抗の「スタフェス」
- **🧠 学習コンテンツ**:
  - **漢字/計算/英単語**: 5000問以上の問題データベースと連携した学習・テスト・復習システム
  - **受験モード**: 中学/高校/大学受験を模した本格シミュレーションゲーム
- **👨‍👩‍👧 保護者向け機能**: 学習状況、出席カレンダー、先生からの称賛ログを確認できる専用ダッシュボード

## 🛠️ 技術スタック
- **Backend**: PHP 8.0
- **Database**: MariaDB 10.5 (MySQL互換)
- **Frontend**: HTML5, CSS3, JavaScript
- **Server**: Apache (推測)
- **Tools**: Git, GitHub, Visual Studio Code

## 💾 データベース設計 (ER図)
`phpMyAdmin SQL Dump.txt`から主要なエンティティを抽出し、以下のように設計しました。正規化を意識し、外部キー制約を用いてデータの整合性を担保しています。

```mermaid
erDiagram
    users {
        int id PK "生徒ID"
        varchar nickname "ニックネーム"
        int starlog_points "ポイント"
        varchar user_rank "ランク"
    }
    attendance {
        int id PK
        int user_id FK
        datetime attendance_datetime
        int earned_experience
    }
    gacha_avatar {
        int id PK
        varchar item_name
        enum rarity
    }
    user_gacha_items {
        int id PK
        int user_id FK
        int gacha_item_id FK
        tinyint active_status
    }
    stafes {
        int id PK "スタフェスID"
        varchar title "お題"
    }
    stafes_votes {
        int id PK
        int stafes_id FK
        int user_id FK
        enum team_choice
    }
    kanji_study_sessions {
        bigint id PK
        int user_id FK
        int section_id FK
        varchar rank_judge
    }

    users ||--o{ attendance : "records"
    users ||--o{ user_gacha_items : "owns"
    gacha_avatar }|--o{ user_gacha_items : "is a type of"
    users ||--o{ stafes_votes : "votes on"
    stafes ||--o{ stafes_votes : "has"
    users ||--o{ kanji_study_sessions : "plays"
