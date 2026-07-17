# Horror Corridor V2

嚴格第一人稱視角的生存恐怖世界探索 Demo，畫面中沒有任何角色、身體或手部——你就是那雙眼睛。用 LingBot-World 生成的場景影片串接而成，在異化成無限惡夢長廊的走廊裡移動、轉身、待機。

## 世界觀

一條原本普通的台式住宅走廊，被異化成永遠走不出去的惡夢長廊。Resident Evil / Silent Hill 式的生存恐怖美學，found footage（偽記錄片）質感的畫面紋理。色調鎖定在 **sickly cold green-white 螢光燈色**——快壞掉的日光燈管，平坦、無陰影的病態照明，禁止出現 purple / blue / neon / 科幻感的配色。牆面滲水、細裂縫蔓延，安靜到有質地。

V2 版本採用 384×656 生成 + local_attn_size 18 + 4x-ClearRealityV1 放大鏈，畫質與世界銜接的連貫性都比前一版更穩定。

## 怎麼玩

打開 `index.html`，用以下按鍵探索：

| 鍵位 | 動作 |
|---|---|
| `W` | 前進 |
| `A` | 左移 |
| `S` | 後退 |
| `D` | 右移 |
| `E` | 待機 |
| `R` | 轉身 |

畫面左下角有疊加的虛擬按鍵，手機單手操作也能玩。左上角 🔇/🔊 可切換背景音樂，第一次按方向鍵時會自動嘗試播放（瀏覽器限制，需使用者先互動才能出聲）。

灰掉的按鈕代表目前站的位置還沒有那個方向的記憶。

## 資料夾結構

```
.
├── index.html
├── videos/        場景影片
└── music/
    └── bgm.mp3    背景音樂
```

## 技術說明

- 影片由 [LingBot-World](https://github.com/Robbyant/lingbot-world) 生成，透過 ComfyUI (`ComfyUI_Rebels_LingBotWorld` 節點包) 產出
- 生成參數：384×656 原生解析度、local_attn_size 18、sink_size 4，CLIP 透過 MultiGPU 分流到第二張顯卡，騰出主卡空間
- 生成後接放大鏈：`ImageUpscaleWithModel`(4x-ClearRealityV1) → `ImageScale`(bilinear，維持 9:16 不變形) → 補幀
- 每條 Prompt 固定鎖定：Survival horror game first-person perspective, Resident Evil / Silent Hill aesthetic, sickly cold green-white fluorescent color grading, found footage horror texture, no character/body/hands visible in frame
- 場景以節點圖 (graph) 串接，每個節點對應一個場景位置，邊 (edge) 對應一段動作影片
- 純前端實作，無後端依賴，可直接用瀏覽器開啟或部署在任何靜態網頁託管服務

## 建議瀏覽器

Chrome 或 Edge。若要錄影功能穩定運作，建議搭配「匯出可攜包 (ZIP)」版本使用，避免影片跨網域來源導致瀏覽器擋下畫面擷取。

---

vplab · 虛擬製作研習社
