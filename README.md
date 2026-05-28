# everything-coding-agent (fork)

[affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) のフォーク。
Claude Code と Codex の両方で使える coding agent plugin として、agents / skills / commands / hooks を配布する。

## フォークで変更した点

- 不要なドキュメント（中国語翻訳、Django skills）を削除して軽量化
- `.claude-plugin/marketplace.json` を `n-seiji` namespace で公開
- `.codex-plugin/plugin.json` を追加し、Codex から `skills/` を読み込めるようにした
- 旧 `install.sh` ベースのインストール経路（`.claude/settings.json`、`rules/` 含む）は廃止
- 新規コマンドを追加:
  - `/cp` — commit and push
  - `/cpp` — commit, push, and open a draft PR
  - `/draft-pr` — draft PR 作成フロー

## インストール

### Claude Code CLI から導入

```text
/plugin marketplace add n-seiji/everything-coding-agent
/plugin install everything-coding-agent@n-seiji
```

### Codex で使う場合

Codex は `.codex-plugin/plugin.json` から `skills/` を読み込む。Webwright と同じく、リポジトリを marketplace として追加してから plugin browser で install する。

```text
codex plugin marketplace add n-seiji/everything-coding-agent
codex
/plugins
```

ローカル checkout を使う場合:

```text
codex plugin marketplace add /absolute/path/to/everything-coding-agent
```

Codex では Claude Code の top-level `commands/` はそのまま slash command としては読まれないため、共通化した workflow は `skills/everything-coding-agent/` に配置する。まず `/review-pr` 相当を `skills/everything-coding-agent/commands/review-pr.md` として提供している。

### dotfiles 経由（自分用 / 推奨）

[n-seiji/dotfiles](https://github.com/n-seiji/dotfiles) の `home/programs/claude.nix` で
flake input としてこのリポジトリを取り込み、`~/.claude/plugins/marketplaces/n-seiji` に
symlink + `enabledPlugins` で有効化する。`home-manager switch` で反映。

## 配布物

| 種別 | パス | 用途 |
|------|------|------|
| agents | `agents/*.md` | planner, code-reviewer, tdd-guide ほか 13 種の subagent |
| skills | `skills/*/SKILL.md` | tdd, security-review, backend-patterns ほか |
| shared commands | `skills/everything-coding-agent/commands/*.md` | Codex / Claude Code 両対応の skill command |
| commands | `commands/*.md` | `/cp`, `/cpp`, `/draft-pr` ほか slash command |
| hooks | `hooks/` | PreToolUse / PostToolUse / Stop |

## 配布外（dotfiles 側で管理）

`rules/`、`settings.json`、`plugins/config.json` は
[plugin reference](https://code.claude.com/docs/en/plugins-reference) の component
仕様に含まれず、プラグインからは配布できない。ユーザ環境への配置は dotfiles で行う。
この前提は Claude Code 向けで、`hooks/hooks.json` も Claude Code 用マニフェストのまま管理している。

- `~/.claude/rules/` — dotfiles `home/files/claude/rules/` を symlink
- `~/.claude/settings.json` — dotfiles の base + `enabledPlugins` をマージしてビルド
- `~/.claude/plugins/config.json` — dotfiles で配置

## 追加コマンド

| コマンド | 説明 |
|----------|------|
| `/cp` | 変更をステージ → コミット → プッシュ |
| `/cpp` | 変更をステージ → コミット → プッシュ → draft PR 作成 |
| `/draft-pr` | draft PR 作成フロー（ブランチチェック付き） |

upstream 由来のコマンド（`/plan`, `/tdd`, `/code-review`, `/build-fix` など）もそのまま利用可能。

## Upstream

- Upstream: [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code)
- License: MIT
