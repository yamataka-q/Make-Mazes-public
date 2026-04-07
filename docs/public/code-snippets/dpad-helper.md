# D-pad Helper

## 概要
このメモでは、Make Mazes! の描画補助キーとして用意している
十字キーUIの考え方を紹介します。

小さなセルをスマホやタブレットで正確に操作しづらい場面を補助するため、
描画中に上下左右へ1セルずつ進めるUIを用意しています。

## 役割
- 描画モード中の補助操作
- 利用可能な方向だけを有効化
- 開閉式で、必要なときだけ表示

## 実装の考え方
- ルートの現在位置を取得する
- 進める方向だけを有効にする
- UIを開閉する
- ボタン押下で指定方向へ1セル分移動させる

## 抜粋コード
```js
const setOpen = (nextOpen) => {
  isOpen = Boolean(nextOpen)
  dpadRoot.classList.toggle("is-open", isOpen)
  toggleButton.setAttribute("aria-expanded", String(isOpen))
  controls.setAttribute("aria-hidden", String(!isOpen))
}

const refresh = () => {
  const viewState = getViewState?.() || {}
  const isVisible = Boolean(viewState.isVisible)
  const availableDirections = viewState.availableDirections || {}

  dpadRoot.classList.toggle("is-visible", isVisible)
  dpadRoot.setAttribute("aria-hidden", String(!isVisible))
  toggleButton.disabled = !isVisible
}
```
