🌐 English | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [हिन्दी](README.hi.md) | [Türkçe](README.tr.md)

---
name: data-analysis
description: EvoLink API を使用したAI駆動のデータ分析。意思決定優先の方法論と統計的厳密性。evolink.ai 提供。
---

# データ分析アシスタント

意思決定優先の方法論と統計的厳密性を備えたAI駆動のデータ分析。

[Evolink.ai](https://evolink.ai?utm_source=clawhub&utm_medium=skill&utm_campaign=data-analysis) 提供

## 使用場面

ユーザーが以下のことを行う必要がある場合、このスキルを使用してください。
- CSV、Excel、JSON ファイルからデータを分析する
- パターン、傾向、または異常を発見する
- 指標とKPIを理解する
- 仮説検定またはA/Bテストを実行する
- コホート分析またはファネル分析を実行する
- データ品質の問題をデバッグする
- 意思決定のための洞察を生成する

**核心原則**: 意思決定のない分析は単なる計算です。分析がXとYのどちらを示すかによって何が変わるのかを常に明確にしてください。

## 使用方法

```bash
{baseDir}/scripts/analyze.sh <ファイルパス> "<分析の質問>"
```

## 設定

| 変数 | デフォルト | 必須 | 説明 |
|---|---|---|---|
| `EVOLINK_API_KEY` | - | はい | あなたのEvoLink APIキー |
| `EVOLINK_MODEL` | `claude-opus-4-6` | いいえ | 分析用モデル。[EvoLink API](https://docs.evolink.ai/en/api-manual/language-series/claude/claude-messages-api?utm_source=clawhub&utm_medium=skill&utm_campaign=data-analysis)でサポートされている任意のモデルに切り替えることができます。 |
| `DATA_ANALYSIS_SAFE_DIR` | `$HOME/.openclaw/workspace` | いいえ | ローカルファイルアクセスが許可されているディレクトリ |

👉 [無料APIキーを取得](https://evolink.ai/signup?utm_source=clawhub&utm_medium=skill&utm_campaign=data-analysis)

## 例

```bash
bash scripts/analyze.sh sales_data.csv "今四半期の収益上位3つの要因は何ですか？"
```

出力:
```
📊 分析中: sales_data.csv
❓ 質問: 今四半期の収益上位3つの要因は何ですか？

🔍 分析結果:

1. **製品カテゴリA** - 240万ドル (合計の40%)
   - 前四半期比15%成長
   - エンタープライズセグメントが牽引

2. **地理的拡大** - 180万ドル (合計の30%)
   - APAC地域の新規市場
   - 前四半期比3倍成長

3. **既存顧客へのアップセル** - 120万ドル (合計の20%)
   - アップグレードオファーのコンバージョン率25%
   - 平均取引額: 5万ドル

📈 信頼度: 高 (n=1,247件のトランザクション)
⚠️ 注意点: 第4四半期にはホリデーシーズンが含まれます
💡 推奨事項: APAC地域での拡大とエンタープライズ顧客へのアップセルを強化する
```

## 方法論

### 1. 意思決定優先
- 意思決定者と質問を特定する
- 結果に基づいて何が変わるかを明確にする
- 計算前に期限を設定する

### 2. 統計的厳密性
- サンプルサイズの十分性を確認する
- 公平な比較グループを確保する
- 複数の比較を考慮する
- 不確実性 (信頼区間) を定量化する
- 効果量が意味のあるものであることを確認する

### 3. 出力基準
- 方法論ではなく、洞察から始める
- 不確実性を定量化する (点推定ではなく範囲で)
- 制限事項を明確に述べる
- 次のステップを推奨する

## セキュリティ

**⚠️ データ転送に関する警告**

このスキルは、データファイルの**全コンテンツ**を読み取り、分析のために`api.evolink.ai`に送信します。**以下の情報を含むファイルではこのスキルを使用しないでください。**
- APIキー、トークン、または認証情報
- 個人識別情報 (PII)
- 機密性の高いビジネスデータ
- 外部サービスに送信したくない機密情報

このスクリプトはセキュリティチェック (ディレクトリ制約、シンボリックリンク拒否、ファイル名ブラックリスト、サイズ/MIME検証) を実装していますが、任意のデータファイルに機密情報が含まれていないことを**保証することはできません**。

**認証情報とネットワーク**

EvoLink APIを呼び出すには`EVOLINK_API_KEY`が必要です。データファイルの内容と分析の質問は、処理のために`api.evolink.ai`に送信されます。EvoLinkはデータを処理し、分析結果を返します。処理後にデータは保存されません。

**ファイルアクセス**

このスキルは、指定されたデータファイル (CSV、Excel、JSON) をローカルファイルシステムから読み取ります。ファイルは`DATA_ANALYSIS_SAFE_DIR` (デフォルト: `$HOME/.openclaw/workspace`) 内にある必要があります。

セキュリティ検証:
- `realpath -e`によるパス解決 (ファイルが存在し、シンボリックリンクを解決する必要があります)
- シンボリックリンクの入力は明示的に拒否されます
- 末尾のスラッシュ比較によるディレクトリ制約
- ファイル名ブラックリスト: `.env*`、`*.key`、`*.pem`、`*.p12`、`*.pfx`、`id_rsa*`、`authorized_keys`、`config.json`、`.bash_history`、`.ssh`、`shadow`、`passwd`
- ファイルサイズ制限: 最大50MB
- MIME検証: `text/csv`、`text/plain`、`application/json`、`application/vnd.ms-excel`、`application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`のみが許可されます

**ネットワークアクセス**

- **EvoLink API** (`api.evolink.ai`) - データの送信と分析結果の受信

すべてのネットワーク呼び出しはcurlを使用しており、スクリプトソースで監査できます。

**永続性と権限**

このスキルは、他のスキルやシステム設定を変更しません。昇格された永続的な権限を要求しません。

## リンク

- [GitHub](https://github.com/EvoLinkAI/data-analysis-skill-for-openclaw)
- [APIリファレンス](https://docs.evolink.ai/en/api-manual/language-series/claude/claude-messages-api?utm_source=clawhub&utm_medium=skill&utm_campaign=data-analysis)
- [コミュニティ](https://discord.com/invite/5mGHfA24kn)
- [サポート](mailto:support@evolink.ai)
