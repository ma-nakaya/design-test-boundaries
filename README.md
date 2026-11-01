# design-test-boundaries

要件、変更差分、実行経路、インシデント、既存テストから、言語・フレームワークに依存しないテスト境界と、リスクベースのテストケースを設計する Agent Skill です。

## 主な用途

- どの境界に、どのレベルのテストを置くか決める
- 同値分割、境界値、決定表、状態遷移から必要ケースを絞る
- mock / fake / stub と実依存の使い分けを決める
- 既存テストの意図、重複、欠落、脆さを説明・レビューする
- 実装順序と優先度を含むテスト設計を作る

## 判断の考え方

テストの配置は、テストランナー名やディレクトリ名ではなく、責務、状態所有、データ表現、ランタイム、プロセス、ネットワーク、信頼境界、永続化、外部依存を基準に判断し、unit / component / integration / contract / persistence / E2E へ割り当てます。

設計は次の流れで進め、成果物を追跡可能にします。

```text
contract → execution path → boundaries → risks → test levels → cases → evidence
```

中核手順は技術非依存です。.NET / Blazor の資料は、対象技術が該当するときだけ読む任意の技術プロファイルとして分離しています。個別プロジェクトやPBL固有の業務名、画面名、テストデータは含めていません。

## 対応

Skill本体は [Agent Skills仕様](https://agentskills.io/specification) の `skills/*/SKILL.md` 構成で、次の環境で同じSkillを利用できます。

- Claude Code
- GitHub Copilot CLI
- GitHub Copilot in VS Code
- Copilot cloud agent / code review
- Agent Skills仕様に対応するその他のエージェント

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

## インストール

GitHub CLI 2.90.0以降の `gh skill` を使う場合、最初に内容を確認できます。

```bash
gh skill preview ma-nakaya/design-test-boundaries design-test-boundaries
```

### GitHub Copilot（プロジェクトスコープ: cloud agent / code review）

対象リポジトリのルートで実行すると、`.github/skills/design-test-boundaries/` へ配置されます。

```bash
gh skill install ma-nakaya/design-test-boundaries design-test-boundaries --dir .github/skills
```

### GitHub Copilot（ユーザースコープ）

```bash
gh skill install ma-nakaya/design-test-boundaries design-test-boundaries --agent github-copilot --scope user
```

### Claude Code（ユーザースコープ）

```bash
gh skill install ma-nakaya/design-test-boundaries design-test-boundaries --agent claude-code --scope user
```

`gh skill` を使わない場合は、`skills/design-test-boundaries` ディレクトリ全体を次へ配置します。

- GitHub Copilot: `~/.copilot/skills/design-test-boundaries/` または `~/.agents/skills/design-test-boundaries/`
- Claude Code: `~/.claude/skills/design-test-boundaries/`

## 更新

```bash
gh skill update design-test-boundaries
```

## ライセンス

MIT License。利用・複製・改変・再配布の条件は [LICENSE](LICENSE) を確認してください。

## 公式情報

- [Agent Skills specification](https://agentskills.io/specification)
- [GitHub Copilot: Adding agent skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
- [Claude: Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
