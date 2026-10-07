# Overview HTML

ソースに基づく全体像を、単一ファイルのHTMLで説明するAgent Skillです。CodexとClaudeで利用できます。

コードやアーキテクチャの説明では、クリックできる構成図、処理のステップ解説、実際のソース抜粋、データ変換の説明を作ります。日本語・英語に対応し、生成HTMLは外部依存なしでオフライン閲覧できます。

## Claude Code

```sh
mkdir -p ~/.claude/skills
git clone https://github.com/Takase0917/overview-html.git ~/.claude/skills/overview-html
```

Claude Codeで次のように依頼してください。

```text
/overview-html このプロジェクトの構成とデータの流れを日本語のHTMLで説明して
```

プロジェクトだけで使う場合は、クローン先を `.claude/skills/overview-html` にします。更新は `git -C ~/.claude/skills/overview-html pull --ff-only` で行えます。

## Claude（Web / Desktop）

[Releases](https://github.com/Takase0917/overview-html/releases) から `overview-html.zip` をダウンロードし、ClaudeのカスタムSkills設定でアップロードしてください。利用にはSkillsとコード実行が使える環境が必要です。設定画面や対応条件は [Claude公式ドキュメント](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) を参照してください。

Web版はアップロードされた資料を根拠に説明します。ローカルリポジトリの調査やブラウザ検証には、それらにアクセスできる環境が必要です。

## Codex

```sh
mkdir -p ~/.codex/skills
git clone https://github.com/Takase0917/overview-html.git ~/.codex/skills/overview-html
```

```text
$overview-html この実装の全体像と確認済みのテスト結果をHTMLにして
```

既存の同名フォルダがある場合は、内容を確認してから導入してください。

## 内容

- `SKILL.md`: 共通の指示と検証手順
- `assets/overview-template.html`: 単一ファイルHTMLのひな形
- `agents/openai.yaml`: Codex用の表示情報。Claudeの利用には不要です。

標準の出力先は `docs/<topic>-overview.html` です。コード説明のインタラクションは、調査した実装に合わせてエージェントが追加します。ブラウザ検証ができない場合は未検証の範囲を報告します。

## License

MIT
