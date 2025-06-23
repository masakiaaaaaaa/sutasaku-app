# 学習支援Webアプリ「スタサク」
### 学習を冒険に変える。生徒の「やりたい！」を引き出す次世代型学習プラットフォーム

<p align="center">
  <a href="https://sutasaku.com" target="_blank"><img src="https://img.shields.io/badge/公式サイト-4A90E2?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Official Website"></a>
  <a href="https://sutasaku.com/demo/trial.php" target="_blank"><img src="https://img.shields.io/badge/デモ版を体験-FF6F61?style=for-the-badge&logo=gamedeveloper&logoColor=white" alt="Try Demo"></a>
</p>

<p align="center">
  <img src="<ここにチラシの画像をアップロードしてそのURLを入れる>" alt="スタサク チラシ" width="600"/>
</p>


## 📜 冒険の始まり (プロジェクト概要)

個別指導塾の主任講師として、私は多くの生徒が「学習へのモチベーション維持」という大きな壁に直面しているのを目の当たりにしてきました。この課題を解決するため、単なる学習ツールではなく、**学習そのものが「冒険」になる世界**を構想し、企画・設計から実装・運用までの全てを独力で行ったのが、この「スタサク」です。

生徒は日々の学習という「クエスト」をこなし、努力の証として「ランク」を上げ、「武器」としての知識を磨きます。そして、獲得したポイントで「個性」を示すアバターを手に入れ、仲間と共に「伝説」を創り上げていきます。これは、私が教育現場で感じた課題を、私の持つ技術力で解決へと導いた物語です。

---

## ✨ 冒険の羅針盤 (主要機能)

「スタサク」は、チラシに描かれた4つのクエストを中心に、多彩な機能で生徒の冒険をサポートします。

<details>
<summary><strong>👑 QUEST 1: 出席を「ランク」に変えろ (出席 & ランクシステム)</strong></summary>
<br>

- **出席管理**: NFCやワンタイムパスワード(OTP)による簡単出席登録機能を実装。
- **ランクシステム**: 出席回数や無遅刻、宿題達成度など、複数の条件をクリアすることで「ブロンズ」から「神話」ランクまで昇格。高ランクほどガチャの優良アイテム排出率が向上します。
- **ランキング機能**: 各部門の頑張りを週間/月間/累計で集計し、仲間と競い合う健全な競争環境を提供します。

</details>

<details>
<summary><strong>🎨 QUEST 2: アバターで「個性」を示せ (カスタマイズ & ガチャ)</strong></summary>
<br>

- **ガチャ機能**: 200種類以上のアイテムが排出されるアバターガチャやランキング装飾ガチャを実装。PHPによる確率計算ロジックを構築しました。
- **ポイントシステム**: 学習活動で得たポイントを使い、ガチャやアイテム購入が可能。生徒の努力が直接的な報酬に繋がる設計です。
- **マイページ**: 獲得したアバターやメダルを自由に飾り、自分だけのプロフィールを作成できます。

</details>

<details>
<summary><strong>⚔️ QUEST 3: 知識を「武器」に変えろ (学習コンテンツ)</strong></summary>
<br>

- **多種多様な学習モード**:
  - **漢字/計算**: 小学校〜中学校範囲を網羅した5000問以上の問題データベースと連携。
  - **英単語**: 「ターゲット1900」など市販の有名単語帳と連携した本格的な語彙力強化システム。
  - **受験モード**: 中学/高校/大学受験を模した本格的な育成シミュレーションゲーム。
- **三位一体の学習サイクル**: 全てのコンテンツで「学習 → テスト → 復習」のサイクルを回せるよう設計し、知識の定着を促します。

</details>

<details>
<summary><strong>🤝 QUEST 4: 仲間と「伝説」を創れ (コミュニティ & イベント)</strong></summary>
<br>

- **イベント機能**:
  - **ボスバトル**: 全員参加で巨大なボスを討伐。日々の出席がボスへのダメージに変換されます。
  - **スタフェス**: 2チームに分かれて競う対抗戦。投票や学習への貢献度で勝敗が決定します。
- **保護者連携機能**: 保護者専用アカウントを発行し、お子様の学習状況や先生からの称賛ログをリアルタイムで確認可能。家庭でのコミュニケーションを促進します。

</details>

---

## 🛠️ 冒険の道具 (技術スタック)

- **Backend**: `PHP 8.0`
- **Database**: `MariaDB 10.5` (MySQL互換)
- **Frontend**: `HTML5`, `CSS3`, `JavaScript`
- **Server Environment**: Apache
- **Tools**: `Git`, `GitHub`, `Visual Studio Code`

---

## 🗺️ 世界の設計図 (データベース設計)

`phpMyAdmin SQL Dump.txt`から50以上のテーブル構造を分析・設計しました。正規化を意識し、外部キー制約を用いてデータの整合性を担保しています。以下はその一部です。

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
        datetime attendance_datetime "出席日時"
        int earned_experience "獲得経験値"
    }
    gacha_avatar {
        int id PK
        varchar item_name "アイテム名"
        enum rarity "レアリティ"
    }
    user_gacha_items {
        int id PK
        int user_id FK
        int gacha_item_id FK
        tinyint active_status "装備状態"
    }
    stafes {
        int id PK "スタフェスID"
        varchar title "お題"
    }
    stafes_votes {
        int id PK
        int stafes_id FK
        int user_id FK
        enum team_choice "投票チーム"
    }
    kanji_study_sessions {
        bigint id PK
        int user_id FK
        int section_id FK
        varchar rank_judge "ランク判定"
    }

    users ||--o{ attendance : "records"
    users ||--o{ user_gacha_items : "owns"
    gacha_avatar }|--o{ user_gacha_items : "is a type of"
    users ||--o{ stafes_votes : "votes on"
    stafes ||--o{ stafes_votes : "has"
    users ||--o{ kanji_study_sessions : "plays"
