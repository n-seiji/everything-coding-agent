# everything-claude-code (fork)

[affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) のフォーク。
Claude Code の **plugin marketplace** として公開しつつ、実行用スクリプトは `Codex` でも使えるように調整している。agents / skills / commands / hooks を配布する。

## フォークで変更した点

- 不要なドキュメント（中国語翻訳、Django skills）を削除して軽量化
- `.claude-plugin/marketplace.json` を `n-seiji` namespace で公開
- 旧 `install.sh` ベースのインストール経路（`.claude/settings.json`、`rules/` 含む）は廃止
- 新規コマンドを追加:
  - `/cp` — commit and push
  - `/cpp` — commit, push, and open a draft PR
  - `/draft-pr` — draft PR 作成フロー

## インストール

### Claude Code CLI から導入

```text
/plugin marketplace add n-seiji/everything-claude-code
/plugin install everything-claude-code@n-seiji
```

### Codex で使う場合

Codex には Claude Code の plugin marketplace 互換レイヤはないため、このリポジトリの配布形態はそのままでは使わない。
ただし `scripts/` 配下のランタイム補助スクリプトは `CODEX_*` 環境変数と `~/.codex` を優先するようにしてあり、Codex から直接呼び出しても動く。

### dotfiles 経由（自分用 / 推奨）

[n-seiji/dotfiles](https://github.com/n-seiji/dotfiles) の `home/programs/claude.nix` で
flake input としてこのリポジトリを取り込み、`~/.claude/plugins/marketplaces/n-seiji` に
symlink + `enabledPlugins` で有効化する。`home-manager switch` で反映。

## 配布物

| 種別 | パス | 用途 |
|------|------|------|
| agents | `agents/*.md` | planner, code-reviewer, tdd-guide ほか 13 種の subagent |
| skills | `skills/*/SKILL.md` | tdd, security-review, backend-patterns ほか |
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
