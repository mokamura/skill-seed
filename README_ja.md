# skill-seed

[English](README.md)

Claude Code の作業中に発見した汎用パターンをスキル化候補として自動蓄積し、あとからまとめてレビューできる仕組み。

## 背景

作業中に「これスキルにしたら便利そう」と思うパターンがあっても、その場で提案されると作業の流れが途切れる。
かといって何もしないと忘れる。

skill-seed は以下のアプローチでこの問題を解決する:

1. Claude が作業中にパターンに気づいたら `candidates.md` に **黙って追記**
2. ユーザーが好きなタイミングで `/review-skill-candidates` を実行してバッチレビュー
3. 承認した候補だけが skill / command として作成される

## インストール

### 1. CLAUDE.md に以下のスニペットを追記

`~/.claude/CLAUDE.md`（グローバル）またはプロジェクトの `CLAUDE.md` に以下を追記する:

````markdown
## Skill Discovery

When you notice a pattern matching any of the triggers below during work, append it to `~/.claude/skills/skill-seed/candidates.md`.
Do NOT suggest to the user. Silently append only.

**Triggers** (1 or more):
- Completed a multi-step workflow that could be reused across projects
- Repeated the same procedure/pattern 2+ times
- Implemented a workaround or complex procedure

**Append format**:
```
### {YYYY-MM-DD}: {kebab-case-name}
- **context**: {what task was being performed when this pattern was detected}
- **summary**: {what this pattern does in one line}
- **reason**: {why this is a skill candidate}
- **type**: skill | command
```

**Rules**:
- Read candidates.md before appending to check for duplicates
- Do not record trivial operations (1-2 lines) or project-specific patterns
- Do not create the skill — only accumulate
- Review happens when the user runs `/review-skill-candidates`
````

### 2. skill をコピー

```bash
cp -r skills/skill-seed ~/.claude/skills/skill-seed
```

### 3. command をコピー

```bash
cp commands/review-skill-candidates.md ~/.claude/commands/review-skill-candidates.md
```

## 使い方

### 候補の蓄積（自動）

通常の作業をするだけでよい。Claude が以下の条件に該当するパターンに気づいたら `~/.claude/skills/skill-seed/candidates.md` に自動で追記する。

- 他プロジェクトでも再利用できそうなパターン
- 同じ手順を2回以上繰り返した
- ワークアラウンドや複雑な手順を実装した

### 候補のレビュー（手動）

```
/review-skill-candidates
```

蓄積された候補を1件ずつ確認し、承認 / 却下 / 保留 を選択する。
承認された候補は `~/.claude/skills/` または `~/.claude/commands/` に作成される。

## ファイル構成

```
skill-seed/
├── CLAUDE.md                              # コントリビューター向けプロジェクトガイド
├── skills/
│   └── skill-seed/
│       ├── SKILL.md                       # 評価基準・フォーマット定義
│       └── candidates.md                  # 候補蓄積ファイル
└── commands/
    └── review-skill-candidates.md         # /review-skill-candidates コマンド
```

## 評価基準

以下の4条件中2つ以上に該当するパターンが候補として記録される。

| 条件 | 説明 |
|------|------|
| 再利用性 | 他プロジェクトでも使える |
| 反復性 | 2回以上同じパターンを実行した |
| 複雑性 | 手順や専門知識が必要 |
| 安定性 | 頻繁に変わらない手順 |

## License

MIT
