# Shared CodeRabbit review policy

[![CodeRabbit Pull Request Reviews](https://img.shields.io/coderabbit/prs/github/serevy/coderabbit?utm_source=oss&utm_medium=github&utm_campaign=serevy%2Fcoderabbit&labelColor=171717&color=FF570A&link=https%3A%2F%2Fcoderabbit.ai&label=CodeRabbit+Reviews)](https://coderabbit.ai)

個人所有のGitHubリポジトリ間で共通化する、**公開可能なCodeRabbitレビュー規約**の実験的な置き場です。

> Status: **experimental**. CodeRabbit公式の中央設定はGitHub **Organization**向けとして説明されています。
> 個人アカウント `serevy` での自動検出・継承は、まだ確認できていません。

## 方針

- **Public-first / free-tier-first**。有料プランやPrivate化は別途検討・承認してから。
- 根拠・発生条件・具体的な影響に基づくレビューを優先する。
- 推測の断定、style-onlyの指摘、同一原因の重複指摘を抑える。
- セキュリティ、プライバシー、既存の設計判断、互換性を尊重する。
- 各リポジトリ固有の契約・言語・テスト・研究上の判断は**各リポジトリ側**で管理する。
- 非公開設計、内部ネットワーク構成、秘密情報、privateプロジェクトの詳細はここへ載せない。

## ファイル

- [`.coderabbit.yaml`](.coderabbit.yaml): 共通のレビュー設定。Learningsは明示的に `local` とする。
- CodeRabbitの**設定ファイル継承**と、Chatで増える**Learnings**は別物。後者をグローバル化しない。

## 検証手順（段階的に実施）

1. CodeRabbit GitHub Appがこの `serevy/coderabbit` にインストール・アクセスできることを確認する。
2. 共通設定をmainへマージする。
3. 最初の導入先を `serevy/agent-kpt` に限定し、既存 `.coderabbit.yaml` を保持したまま `inheritance: true` を追加するPRを作る。
4. PRで `@coderabbitai configuration` を実行し、中央リポジトリ由来の値（例: `reviews.in_progress_fortune: false`）が反映されるか、既存の `path_instructions` が維持されるか検証する。
5. **個人所有リポジトリで中央自動継承が使えない場合**、同一所有者の設定を明示参照できる `remote_config` を小さなPRで検証する（下記参照）。
6. それでも運用できない場合は、`serevy/.github` 等を規約のソースにして各リポジトリへ同期する。**`.github` 自体がCodeRabbit設定を自動継承するわけではない**。
7. 成功が確認されてから、CodeRabbit導入済みリポジトリへ段階的に展開する。

```yaml
# remote_config代替案: 公式仕様の「同じ所有者」を利用
# 自動中央継承が使えない場合だけ、導入先の設定で試す。
remote_config:
  repository: "coderabbit"
  ref: "main"
  path: ".coderabbit.yaml"
```

### 注意: 設定のマージ

- `inheritance: true` はデフォルトで無効の階層設定マージを有効にする。
- 同じ `path` を持つ `reviews.path_instructions` は、導入先の指示が優先される。特に `agent-kpt` の `**/*` と `.github/workflows/**` は上書きされず残す。
- Learningsに一時的なPR事情を永続保存しない。非公開情報は入力しない。
- 無料プランで未確認のLearnings承認機能や有料機能は、運用の前提にしない。

## 公式資料

- [Central configuration](https://docs.coderabbit.ai/configuration/central-configuration)
- [Configuration inheritance](https://docs.coderabbit.ai/configuration/configuration-inheritance)
- [Shared configuration / remote_config](https://docs.coderabbit.ai/getting-started/yaml-configuration)
- [Learnings](https://docs.coderabbit.ai/knowledge-base/learnings)
