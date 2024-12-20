# Width, Height

> CSS3 增加讓 Width, Height 可以設置 min-content, max-content, fit-content，這些相對於根據內部元素所設置，而先前的 width: 80% 這種是根據外部元素 (parent) 所相對設置。

當 CSS 中的父元素沒有明確設置 `height` 或 `width` 值時，子元素的寬度和高度會根據一些特定的規則來決定：

**1. 寬度 Width**

默認情況下，塊級元素（如 `div`、`p` 等）的寬度會自動擴展以填滿父元素的整個寬度，除非對父元素或子元素設置了具體的寬度。

* **塊級元素 block**（如 `div`、`p`）：
  * 如果父元素沒有設置 `width`，子元素會自動佔滿父元素的整個寬度。
  * 你可以通過 `width` 屬性來控制子元素的寬度，例如 `width: 50%` 或 `width: 200px`。
* **行內元素 inline**（如 `span`、`a`）：
  * 行內元素的寬度會根據內容自適應，除非你對它們設置了具體的寬度。

**2. 高度 Height**

高度的處理和寬度有所不同。默認情況下：

* **塊級元素**：
  * 如果父元素沒有設置明確的高度（例如 `height`、`min-height`），子元素的高度會根據其內容來決定。也就是說，子元素的高度會隨著內容多少來增長。
  * 如果父元素有具體的高度設置，並且子元素的高度設置為百分比值（如 `height: 100%`），那麼子元素會按照父元素的高度進行調整。但這要求父元素必須有明確的高度設定。
  * 你也可以使用 `min-height` 來確保子元素至少有一個最小高度，無論父元素的高度如何。
* **行內元素**：
  * 行內元素的高度不會被單獨控制，它們的高度會根據元素內部文字的行高自動調整。

**關鍵點：**

* **百分比值（如 `height: 100%; width: 100%`）**：若要使用百分比值來設置高度或寬度，父元素必須有明確的高度或寬度設定（無論是像素、百分比，或通過 `min-height`/`min-width` 設置）。
* **依賴內容的大小**：如果父元素沒有設置高度或寬度，且子元素也沒有明確的大小設定，那麼子元素的高度和寬度會根據其內容來決定。

**範例：**

```html
<div class="parent">
  <div class="child">子元素內容</div>
</div>

<style>
  .parent {
    /* 父元素沒有設置高度或寬度 */
    background-color: lightgrey;
  }

  .child {
    width: 50%; /* 子元素寬度設為父元素的 50% */
    height: 100px; /* 固定高度 100px */
    background-color: lightblue;
  }
</style>
```

在這個範例中：

* 子元素的寬度會是父元素寬度的 **50%**。
* 因為父元素沒有設置寬度，子元素的寬度將依賴於視窗的寬度或其包含塊的寬度的 50%。
* 子元素的高度為 **100px**，即使父元素沒有設置高度，它的高度也會自動根據子元素來調整。
