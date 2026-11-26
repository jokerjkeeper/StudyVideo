# 《明日方舟：終末地》風格角色渲染｜UE5 課程筆記

- 原始影片：[【补档】【虚幻5 开源课程】如何制作终末地的角色渲染](https://www.bilibili.com/video/BV17fBFB4EFX/)
- Bilibili 搬運者：次次先生
- 課程作者：新楊 XIYAG
- 共 16 段，約 6 小時 33 分。時間碼從各段開頭計算。

這是社群教學用 UE5 重現《終末地》風格角色渲染的做法。筆記依課程的實作與示範整理，片中對遊戲實作的推測屬於講師分析。

## 分段索引

| 分段 | 主題 | 片長 |
| --- | --- | ---: |
| [P01](https://www.bilibili.com/video/BV17fBFB4EFX/?p=1) | 前言 | 09:04 |
| [P02](https://www.bilibili.com/video/BV17fBFB4EFX/?p=2) | 拆解渲染效果 | 17:19 |
| [P03](https://www.bilibili.com/video/BV17fBFB4EFX/?p=3) | 反推貼圖用途 | 24:15 |
| [P04](https://www.bilibili.com/video/BV17fBFB4EFX/?p=4) | 皮膚材質 | 48:39 |
| [P05](https://www.bilibili.com/video/BV17fBFB4EFX/?p=5) | 拆解服裝材質 | 17:47 |
| [P06](https://www.bilibili.com/video/BV17fBFB4EFX/?p=6) | 服裝基礎材質 | 21:29 |
| [P07](https://www.bilibili.com/video/BV17fBFB4EFX/?p=7) | 服裝質感 | 41:48 |
| [P08](https://www.bilibili.com/video/BV17fBFB4EFX/?p=8) | 臉部基礎色 | 27:59 |
| [P09](https://www.bilibili.com/video/BV17fBFB4EFX/?p=9) | 頭髮基礎 | 38:46 |
| [P10](https://www.bilibili.com/video/BV17fBFB4EFX/?p=10) | 頭髮細化 | 17:56 |
| [P11](https://www.bilibili.com/video/BV17fBFB4EFX/?p=11) | 頭髮高光 | 35:41 |
| [P12](https://www.bilibili.com/video/BV17fBFB4EFX/?p=12) | 眼睛視差 | 11:00 |
| [P13](https://www.bilibili.com/video/BV17fBFB4EFX/?p=13) | 眉毛細化 | 11:52 |
| [P14](https://www.bilibili.com/video/BV17fBFB4EFX/?p=14) | 瀏海陰影 | 18:53 |
| [P15](https://www.bilibili.com/video/BV17fBFB4EFX/?p=15) | 等距邊緣高光 | 33:38 |
| [P16](https://www.bilibili.com/video/BV17fBFB4EFX/?p=16) | 描邊 | 16:39 |

## 渲染技術與解決的問題

| 分段與時間碼 | 技術 | 解決的問題與課程做法 |
| --- | --- | --- |
| [P01 00:35](https://www.bilibili.com/video/BV17fBFB4EFX/?p=1&t=35) | 材質框架與課程目標 | 在動手前先確認各部件共同需要的輸入與可重用的渲染邏輯。這讓後續皮膚、服裝、臉和頭髮能各自試驗，不必每節重做整套材質。 |
| [P01 04:02](https://www.bilibili.com/video/BV17fBFB4EFX/?p=1&t=242) | 用較低成本拆出卡通暗面 | 依教學先辨認底色、邊緣暗部與高光。只對要改善的那一層做開關比較，能分辨細節是貼圖提供還是由材質計算。 |
| [P02 00:59](https://www.bilibili.com/video/BV17fBFB4EFX/?p=2&t=59) | PBR 與 NPR 的分工 | 在保留衣物反光和材質反應的同時，另用風格化明暗處理臉或皮膚，避免讓同一套寫實反應支配整個角色。 |
| [P02 03:18](https://www.bilibili.com/video/BV17fBFB4EFX/?p=2&t=198) | 把臉部覆蓋陰影拆出 | 瀏海附近的暗形需要在指定位置維持清楚。課程把額外的陰影模型列為獨立層，可和臉部基本著色分開測試。 |
| [P03 09:16](https://www.bilibili.com/video/BV17fBFB4EFX/?p=3&t=556) | 辨認貼圖及通道用途 | 只憑檔名容易把顏色、遮罩、法線混在一起。先認底色，再逐通道單獨顯示，從白黑分布和部位對應判斷用途。 |
| [P03 11:34](https://www.bilibili.com/video/BV17fBFB4EFX/?p=3&t=694) | 用材質實例做通道測試 | 建立測試材質實例，把候選通道逐個切到同一預覽輸出，使皮膚與衣物交界值能在相同光照下比較。 |
| [P04 04:04](https://www.bilibili.com/video/BV17fBFB4EFX/?p=4&t=244) | 區分 sRGB 顏色與線性資料 | 底色是供顯示的顏色，遮罩和參數是數值資料。若把後者經過顏色轉換，閾值和混合比例會偏移。先檢查貼圖匯入設置再調光。 |
| [P04 22:31](https://www.bilibili.com/video/BV17fBFB4EFX/?p=4&t=1351) | 皮膚陰影與次表面外觀 | 皮膚陰影若像均勻壓黑會偏灰。課程比較暗部與次表面設定，將顏色與光照反應分開調，確認面部和手腳都保有想要的層次。 |
| [P05 01:43](https://www.bilibili.com/video/BV17fBFB4EFX/?p=5&t=103) | 統一向量座標空間並正規化 | 光照和法線的點積需要同一空間的單位向量。先核對切線空間、世界空間的來源與轉換，避免角色一轉向就出現錯誤光照。 |
| [P05 10:22](https://www.bilibili.com/video/BV17fBFB4EFX/?p=5&t=622) | 先拆分衣服材質的區域 | 布料和金屬的反射與高光需求不同。先找出標記金屬等區域的通道，再選要保留的漫反射、亮邊或局部高光。 |
| [P06 09:32](https://www.bilibili.com/video/BV17fBFB4EFX/?p=6&t=572) | 搭建服裝的基礎參數 | 把底色、金屬度、高光、粗糙度分成可單獨調整的輸入，後續可以保留相同框架，只用材質實例更換布料、配件與顏色。 |
| [P06 16:06](https://www.bilibili.com/video/BV17fBFB4EFX/?p=6&t=966) | 確認法線貼圖方向 | 同一張法線貼圖在不同座標慣例中可能讓凸凹看起來反向。以可辨識的縫線測試 OpenGL 和引擎所需的法線方向，再調著色。 |
| [P07 00:09](https://www.bilibili.com/video/BV17fBFB4EFX/?p=7&t=9) | 分開看 AO 與服裝照明 | 衣褶的位置需要能讀到遮蔽，但陰影太黑會吃掉布料。先單看 AO，再和照明合成，辨認是哪一層造成過暗或失去結構。 |
| [P07 17:34](https://www.bilibili.com/video/BV17fBFB4EFX/?p=7&t=1054) | 調整服裝高光 | 布料與配件的高光需要不同的強度與範圍。用影片的開關和參數對比檢查，避免過亮的高光把原本的底色或摺痕蓋住。 |
| [P08 00:41](https://www.bilibili.com/video/BV17fBFB4EFX/?p=8&t=41) | 把臉拆成底色、內線、陰影 | 臉的陰影不穩定時，先查底色和內線是否正確，再修改陰影。這能保留原本的表情設計，不用為了修光影重畫整張臉。 |
| [P08 17:35](https://www.bilibili.com/video/BV17fBFB4EFX/?p=8&t=1055) | 用遮罩限制局部改色 | 整張臉一起改色會把原有輪廓或陰影弄髒。用局部遮罩區分要調的範圍，讓混合不影響其他區域。 |
| [P09 04:04](https://www.bilibili.com/video/BV17fBFB4EFX/?p=9&t=244) | 用球形法線整理頭髮光影 | 每束頭髮的幾何起伏可能把明暗切碎。以較平滑的球形法線統整大形，再調色帶，能保留髮型輪廓而不讓光影出現太多小塊。 |
| [P09 27:46](https://www.bilibili.com/video/BV17fBFB4EFX/?p=9&t=1666) | 控制髮色與陰影過渡 | 塗黑所有背光髮束會沒有層次。以色帶控制明暗過渡和暗部顏色，讓不同髮束保持方向感。 |
| [P10 04:36](https://www.bilibili.com/video/BV17fBFB4EFX/?p=10&t=276) | 用可控的光源位置細調髮部效果 | 為指定髮部效果取一個光源位置，可以在材質裡調整，不必改變真實場景燈。片中示範由藍圖經材質參數集合將位置傳給材質。 |
| [P11 02:51](https://www.bilibili.com/video/BV17fBFB4EFX/?p=11&t=171) | 轉到切線空間計算方向性髮光 | 髮束的亮帶要跟著髮絲方向，先把用於高光的方向轉進髮束的切線空間，再檢查角色、鏡頭和光源各自轉動時光帶的變化。 |
| [P11 08:05](https://www.bilibili.com/video/BV17fBFB4EFX/?p=11&t=485) | 建立髮光光環 | 頭髮亮部需要可以控制的集中帶狀形狀。先驗證高光帶位置，再調色和範圍，避免把其他髮色層一起沖亮。 |
| [P12 06:28](https://www.bilibili.com/video/BV17fBFB4EFX/?p=12&t=388) | 高度資訊驅動虹膜視差與高光 | 眼睛近景若只有平面貼圖會像貼紙。課程把高度與高光分到貼圖通道，配合 UV 取樣控制視角下的位移，使虹膜和光點呈現深度差。 |
| [P13 00:46](https://www.bilibili.com/video/BV17fBFB4EFX/?p=13&t=46) | 半透明眉毛材質 | 瀏海會遮住眉毛，但表情仍要清楚。示範從獨立材質調整透明和顯示，再檢查帶有突出的眉毛模型在不同角度的遮擋。 |
| [P14 06:08](https://www.bilibili.com/video/BV17fBFB4EFX/?p=14&t=368) | 獨立控制瀏海陰影 | 臉部暗形只需落在瀏海覆蓋範圍，宜跟基本臉部材質分開。使用專門的陰影區域來維持特定輪廓與位置。 |
| [P14 08:43](https://www.bilibili.com/video/BV17fBFB4EFX/?p=14&t=523) | 依深度與後方顏色調整覆蓋效果 | 半透明陰影若直接填單一深色，容易像貼一塊色紙。課程討論用深度決定投影位置，以及取得後方表面的色彩後再混合的做法。 |
| [P15 10:25](https://www.bilibili.com/video/BV17fBFB4EFX/?p=15&t=625) | 用附近像素的場景深度抓邊緣 | 局部高光需要跟隨幾何邊界，直接抓色差會被貼圖花紋干擾。課程使用偏移取樣比較周圍的場景深度，讓偵測依據來自深度變化。 |
| [P15 30:49](https://www.bilibili.com/video/BV17fBFB4EFX/?p=15&t=1849) | 用自訂深度模板限制邊緣高光 | 同一個後處理會掃到其他物體的邊緣。替指定物件設定 Custom Stencil 值，讓高光遮罩只涵蓋選定的渲染部件。 |
| [P16 02:31](https://www.bilibili.com/video/BV17fBFB4EFX/?p=16&t=151) | 沿頂點法線外推的背面描邊 | 模型輪廓需要在不同角度可見。將外殼沿法線外推並剔除正面，由原網格蓋住外殼中央，只讓外殼露出的部分形成線條。 |
| [P16 08:00](https://www.bilibili.com/video/BV17fBFB4EFX/?p=16&t=480) | 用深度淡化減輕描邊穿插 | 兩塊近距離的描邊外殼可能互相穿插。課程提到以 Depth Fade 淡化相交處的線條，減輕重疊造成的硬邊。 |

## 字幕來源

Bilibili 的這 16 段影片沒有可匯出的字幕軌。本機逐段產出中文字幕（SRT）與文字稿（TXT），來源為中文語音自動轉錄，專有名詞與節點名稱可能有辨識誤差。
