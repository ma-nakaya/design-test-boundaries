# design-test-boundaries

要件、変更差分、実行経路、インシデント、既存テストから、言語・フレームワークに依存しないテスト境界とリスクベースのテストケースを設計する Agent Skill です。

テストランナー名やディレクトリ名ではなく、責務、状態所有、データ表現、ランタイム、プロセス、ネットワーク、信頼境界、永続化、外部依存を基準に、unit／component／integration／contract／persistence／E2E の配置を判断します。

## 対応

- Claude Code
- GitHub Copilot CLI
- GitHub Copilot in VS Code
- Copilot cloud agent／code review
- Agent Skills仕様に対応するその他のエージェント

Skill本体は [Agent Skills仕様](https://agentskills.io/specification) の `skills/*/SKILL.md` 構成です。Claude CodeとGitHub Copilotで同じSkillを利用できます。

## 構成

```text
skills/design-test-boundaries/
├── SKILL.md
└── references/
    ├── boundary-catalog.md
    ├── case-design-techniques.md
    ├── dotnet-blazor-profile.md
    ├── stack-boundary-map.md
    └── test-design-template.md
```

中核手順は技術非依存です。.NET／Blazorの資料は、対象技術が該当するときだけ読む任意の技術プロファイルです。個別プロジェクトやPBL固有の業務名、画面名、テストデータは含めていません。

## インストール

GitHub CLI 2.90.0以降の `gh skill` を使う場合、最初に内容を確認できます。

```bash
gh skill preview ma-nakaya/design-test-boundaries design-test-boundaries
```

### GitHub Copilot（ユーザースコープ）

```bash
gh skill install ma-nakaya/design-test-boundaries design-test-boundaries --agent github-copilot --scope user
```

### Claude Code（ユーザースコープ）

```bash
gh skill install ma-nakaya/design-test-boundaries design-test-boundaries --agent claude-code --scope user
```

インストール後の更新は次で確認できます。

```bash
gh skill update design-test-boundaries
```

`gh skill`を使わない場合は、`skills/design-test-boundaries` ディレクトリ全体を次へ配置します。

- GitHub Copilot: `~/.copilot/skills/design-test-boundaries/` または `~/.agents/skills/design-test-boundaries/`
- Claude Code: `~/.claude/skills/design-test-boundaries/`

利用・複製・改変・再配布の条件は [MIT License](LICENSE) を確認してください。

## 主な用途

- どの境界に、どのレベルのテストを置くか決める
- 同値分割、境界値、決定表、状態遷移から必要ケースを絞る
- mock／fake／stubと実依存の使い分けを決める
- 既存テストの意図、重複、欠落、脆さを説明・レビューする
- 実装順序と優先度を含むテスト設計を作る

## 公式情報

- [Agent Skills specification](https://agentskills.io/specification)
- [GitHub Copilot: Adding agent skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
- [Claude: Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
