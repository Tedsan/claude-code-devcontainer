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

## 使用方法

### 1. devcontainer で開く
1. VS Code で このプロジェクトを開く
2. コマンドパレット (`Ctrl+Shift+P`) から「Dev Containers: Reopen in Container」を選択
3. コンテナのビルドと起動を待つ

### 2. Claude Code CLI の使用
```bash
# Claude Code CLI のヘルプを表示
claude-code --help

# Claude Code を起動
claude-code
```

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
