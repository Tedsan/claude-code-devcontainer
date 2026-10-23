# Claude Code Development Environment

このプロジェクトは、Claude Code CLI を devcontainer 上で動作させるための最低限の開発環境です。

## 特徴

- **Python 3.11** ベースの開発環境
- **Claude Code CLI** がプリインストール済み
- **VS Code** 拡張機能を含む完全な Python 開発環境
- **Jupyter Notebook** サポート
- **コード品質ツール** (Black, Flake8, MyPy, Ruff) の統合

## 含まれるツール

### Claude Code CLI
- `@anthropic-ai/claude-code` がグローバルにインストール済み
- Node.js 22 環境で動作

### Python 開発環境
- Python 3.11
- uv (高速 Python パッケージマネージャー)
- Jupyter Notebook
- データ分析ライブラリ (pandas, numpy, matplotlib, seaborn)
- Web開発ライブラリ (FastAPI, uvicorn)

### コード品質ツール
- **Black** - コードフォーマッター
- **Flake8** - リンター
- **MyPy** - 型チェッカー
- **isort** - インポート整理
- **Ruff** - 高速リンター
- **pytest** - テストフレームワーク

## 前提条件

- Docker（Docker Desktop など）が起動していること
- VS Code と [Dev Containers 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)（`ms-vscode-remote.remote-containers`）

## 使用方法

### 1. devcontainer で開く
```bash
git clone https://github.com/Tedsan/claude-code-devcontainer && code claude-code-devcontainer
```

1. VS Code で このプロジェクトを開く
2. コマンドパレット (`Ctrl+Shift+P`) から「Dev Containers: Reopen in Container」を選択
3. コンテナのビルドと起動を待つ

### 2. Claude Code CLI の使用
```bash
# Claude Code CLI のヘルプを表示
claude --help

# Claude Code を起動
claude
```

初回起動時はブラウザでのログインが必要です。コンテナ内ではログイン後のコールバックが戻ってこない場合があるので、その場合はブラウザに表示されたコードをターミナルに貼り付けてください。

### 3. Python 開発
```bash
# 依存関係のインストール
pip install -r requirements.txt

# Jupyter Notebook の起動
jupyter notebook --allow-root --ip=0.0.0.0

# テストの実行
pytest

# コードフォーマット
black .
```

## Claude Code の設定・認証情報の保持

`~/.claude`（`/home/vscode/.claude`）は名前付きボリューム `claude-code-config-${devcontainerId}` にマウントしているため、Rebuild してもログイン状態や設定が残ります。`CLAUDE_CONFIG_DIR` もこのディレクトリを指すように設定しています。
ログイン情報を完全に消したい場合は、Docker 側でこのボリュームを削除してください。

## ポートフォワーディング

以下のポートが自動的にフォワードされます：
- `8000` - 一般的な Web アプリケーション
- `8080` - 代替 Web ポート
- `3000` - フロントエンド開発

## カスタマイズ

必要に応じて以下のファイルを編集してください：
- `requirements.txt` - Python 依存関係の追加
- `.devcontainer/devcontainer.json` - VS Code 設定や拡張機能
- `.devcontainer/Dockerfile` - システムレベルの依存関係
