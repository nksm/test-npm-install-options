# npm install オプションテスト

このプロジェクトは、`npm install` コマンドの `--omit` と `--include` オプションの動作を検証するためのテストリポジトリです。

## 概要

npm の依存関係管理において、特定のカテゴリの依存関係を含めたり除外したりする方法を実証します。

## 依存関係の構成

```json
{
  "dependencies": {
    "colors": "^1.4.0"
  },
  "devDependencies": {
    "dotenv": "^16.3.1"
  },
  "optionalDependencies": {
    "yaml": "^2.3.4"
  }
}
```

## テスト結果

### 1. `npm install --omit=optional`

optionalDependencies を除外してインストールします。

- ✅ colors (dependencies)
- ✅ dotenv (devDependencies)
- ❌ yaml (optionalDependencies) - **除外される**

インストールされるパッケージ数: **2 個**

### 2. `npm install --include=prod --include=dev`

production と development の依存関係を明示的に含めてインストールします。

- ✅ colors (dependencies)
- ✅ dotenv (devDependencies)
- ✅ yaml (optionalDependencies) - **デフォルトで含まれる**

インストールされるパッケージ数: **3 個**

## 主な違い

| オプション                     | 動作                         | optionalDependencies の扱い |
| ------------------------------ | ---------------------------- | --------------------------- |
| `--omit=optional`              | 指定したカテゴリを**除外**   | 除外される                  |
| `--include=prod --include=dev` | 指定したカテゴリを**含める** | デフォルトで含まれる        |

## まとめ

- `--omit` オプションは特定のカテゴリを**除外**するための設定
- `--include` オプションは特定のカテゴリを**含める**ための設定
- optionalDependencies を除外したい場合は `--omit=optional` を使用する
- `--include` を使用しても、明示的に除外しない限り optionalDependencies は含まれる

## 使用例

```bash
# optionalDependencies を除外してインストール
npm install --omit=optional

# production のみインストール（dev と optional を除外）
npm install --omit=dev --omit=optional

# production と development のみインストール
npm install --include=prod --include=dev --omit=optional
```
