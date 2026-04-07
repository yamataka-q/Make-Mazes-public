# Export Notes

## 概要
このメモでは、Make Mazes! における画像出力まわりの考え方を紹介します。

アプリでは、生成した迷路を問題用・解答用のPNG画像として保存できます。  
SVGとして組み立てた迷路データを、ブラウザ上でPNGへ変換してダウンロードする構成です。

## 役割
- ファイル名を安全に整形する
- SVGからPNGへ変換する
- Blobとしてダウンロードさせる
- 保存時のフィードバックを返す

## 実装の考え方
- タイトルに使えない文字を除去する
- SVGを読み込み可能な形式へ変換する
- Canvasへ描画する
- PNG Blobを生成する
- ダウンロードリンクを一時的に作る

## 抜粋コード
```js
export const sanitizeMazeFileNamePart = (value = "") => {
  return String(value)
    .trim()
    .replace(/[\\/:*?"<>|]/g, "-")
    .replace(/\s+/g, "_")
    .replace(/_+/g, "_")
    .replace(/^_+|_+$/g, "")
}

export const downloadBlobFile = (blob, fileName = "maze.png") => {
  if (!blob) return

  const blobUrl = URL.createObjectURL(blob)
  const link = document.createElement("a")

  link.href = blobUrl
  link.download = fileName
  document.body.appendChild(link)
  link.click()
  link.remove()

  URL.revokeObjectURL(blobUrl)
}
```
