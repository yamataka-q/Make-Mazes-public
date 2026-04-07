# Storage Notes

## 概要
このメモでは、Make Mazes! の一時保存の考え方を紹介します。

迷路エディタの状態やプレビュー情報は、
ブラウザの sessionStorage を使って一時的に保持しています。  
これにより、ページ移動後もすぐに状態を戻しやすくしています。

## 役割
- エディタ状態の保持
- プレビュー用データの保持
- 保存期限の管理
- 期限切れデータの破棄

## 実装の考え方
- 保存時に `savedAt` と `expiresAt` を持たせる
- ラップした形式で保存する
- 読み込み時に有効期限を確認する
- 期限切れなら削除する

## 抜粋コード
```js
const STORAGE_SCHEMA_VERSION = 1
const MAZE_STORAGE_TTL_MINUTES = 30
const MAZE_STORAGE_TTL_MS = MAZE_STORAGE_TTL_MINUTES * 60 * 1000

const buildStorageEnvelope = (storageKey, data) => {
  const ttlMs = STORAGE_TTL_BY_KEY[storageKey] || MAZE_STORAGE_TTL_MS
  const savedAt = new Date().toISOString()
  const expiresAt = new Date(Date.now() + ttlMs).toISOString()

  return {
    version: STORAGE_SCHEMA_VERSION,
    savedAt,
    expiresAt,
    data
  }
}
```
