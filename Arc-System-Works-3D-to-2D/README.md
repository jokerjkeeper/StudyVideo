# 《罪惡裝備 Xrd》與《龍珠鬥士Z》三渲二技術筆記

原始影片由「设计门外憨」上傳。下方時間碼對應這個 B 站版本。[《罪恶装备Xrd》与《龙珠斗士Z》动画解析 - 3D转2D视觉技术 / 伪2D动画渲染 / Arc System Works渲染技巧](https://www.bilibili.com/video/BV1WfxZ6uEv4/)

影片以 Arc System Works 的作品說明如何讓 3D 角色保留手繪動畫的線條、明暗和動作節奏。著色、描邊和動畫的實作細節主要來自《罪惡裝備 Xrd》。片中也以《龍珠鬥士Z》示範同類視覺效果，並穿插《罪惡裝備 Strive》的後續案例。

## 模型、明暗與線條

| 影片時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [01:20](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=80)、[01:32](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=92) | 以 3D 模型保留 2D 精靈的外觀 | 固定視角繪製的角色精靈難以支援大幅度轉鏡與極近特寫。3D 幾何讓必殺技可自由取景和改變透視，再用描邊、明暗色塊和動畫節奏延續舊作的畫風。 |
| [02:49](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=169) | 高面數建模與頂點資料 | 角色近景會放大低解析度貼圖的鋸齒和模糊。《Xrd》把重要細節做成幾何，並用頂點顏色、頂點法線和 UV 承載控制資料。GDC 簡報所述的模型平均約 4 萬三角形，用來支撐極近特寫。 |
| [03:04](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=184) | 明暗閾值與手調法線 | 賽璐璐著色的明暗交界陡峭，一點法線起伏就可能在臉上形成大塊雜斑。用光向與法線的內積對照閾值決定亮面或暗面。美術手修頂點法線來整理暗面形狀，並用頂點色通道局部偏移閾值，控制哪些區域更容易入陰。 |
| [03:15](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=195) | 角色專屬光向 | 同一盞場景光不一定能讓兩個角色都呈現清楚、好看的暗面。每個角色有獨立光向，戰鬥中維持穩定，過場則由動畫師配合姿勢與鏡頭調整。 |
| [03:27](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=207) | 亮面底色與陰影色調貼圖 | 用同一個環境色把全部材質壓暗，容易讓皮膚、布料和金屬失去區別。《Xrd》用底色貼圖定亮面，乘上色調貼圖得到陰面。美術可分別決定陰影的亮度與色偏。 |
| [03:37](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=217)、[03:56](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=236) | 反向殼描邊（Inverted Hull） | 角色容易融入背景，還需要像手繪那樣局部粗細有別的外輪廓。把第二層網格沿法線外擴、剔除正面，讓露在模型外緣的背面形成線條。開發者可依鏡頭距離、頂點色調整線寬，並用頂點色控制深度偏移。相較於依深度或法線差異的螢幕空間描邊，反向殼可在建模時預覽並逐處控制。 |
| [04:26](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=266) | 外輪廓的局部修整 | 外擴後的殼可能在臉、薄布或某些鏡頭角度露出多餘線段。需要在特寫和戰鬥鏡頭來回檢查，局部收細、隱藏線條或調整深度，維持連續且乾淨的輪廓。 |
| [04:48](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=288)、[05:08](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=308) | 軸對齊 UV 內部線條 | 衣縫和部件分界在模型表面，特寫會放大直接畫在貼圖上的斜線鋸齒。《Xrd》在貼圖上放沿 U 或 V 軸的色條，再把網格 UV 沿色條對齊。UV 與色條的重疊量控制線寬。這樣可保留乾淨的邊，但 UV 會拉伸，較不適合在同一張圖上畫精細紋理。 |

這些《Xrd》實作可對照開發者本村純也的 GDC 2015 簡報第 16–25 頁。[《The Art Style of Guilty Gear Xrd》原始簡報](https://media.gdcvault.com/gdc2015/presentations/Motomura_Junya_GuiltyGearXrd.pdf#page=16)

## 動作、骨架與製作流程

| 影片時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [05:31](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=331)、[06:01](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=361) | 有限動畫與階梯式姿勢 | 持續平滑插值會把手繪動畫的停格、重拍與跳姿勢抹平。《Xrd》以手擺關鍵姿勢和刻意停留維持動作節奏。這裡控制的是角色姿勢的更新節奏，遊戲的渲染更新可以維持原有幀率。 |
| [06:18](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=378) | 高控制度骨架 | 動畫畫格雖少，但每張都要有足夠資訊。開發者簡報報告《Xrd》每角約 400–600 根骨骼，讓動畫師能逐格控制表情、服裝和身體小部位。這是該作的製作選擇，所需骨數依角色而異。 |
| [06:30](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=390) | 擠壓拉伸與逐姿勢變形 | 幾何保持固定形狀時，快速轉身或透視變化容易顯得僵硬。動畫師會依畫面的剪影改變肢體與局部比例，讓動作更有張力。《Xrd》團隊補上了引擎原本缺少的縮放動畫支援。 |
| [07:00](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=420) | 依格鬥姿勢配置髖關節 | 站姿好看的骨架，大跨步時仍可能顯腿短或限制張腿幅度。影片用髖關節過低作例子，說明應在寬站姿與極端動作中一起驗證從脊椎到腳踝的運動，再調整關節位置。 |
| [07:31](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=451)、[08:41](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=521)、[09:11](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=551) | 2D 分鏡先定輪廓與服裝變形 | 概念圖只畫靜態站姿，無法預先發現戰鬥中服裝遮擋、輪廓糊成一團或透視不夠有力的問題。《Strive》段落展示先畫實際姿勢、鏡頭、誇張表情和褶皺，再讓建模與動畫共用同一份視覺目標。 |
| [09:09](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=549) | 頂點動畫貼圖（VAT） | 片中短暫展示《Strive》的特效範例。把預先製作的頂點動畫資料存進貼圖，播放時由著色器重建變形，適合有大量頂點形變的效果。這個畫面說明的是特效的播放方式。 |

《Xrd》的有限動畫、骨架與逐格變形可對照 GDC 簡報第 27–29 頁。《Strive》的製作例另見開發團隊公開資料。[GDC 2015 原始簡報](https://media.gdcvault.com/gdc2015/presentations/Motomura_Junya_GuiltyGearXrd.pdf#page=27) · [Arc System Works Academy](https://www.arcsystemworks.jp/academy/)

## 如何回看片段

1. 在 [01:00](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=60) 的悟空畫面看臉部色塊、服裝暗面和輪廓如何分開角色與背景。可再用《龍珠鬥士Z》的官方畫面對照動畫質感。表中的骨數、貼圖編碼與光向規則是《Xrd》有公開資料可核對的做法。[《龍珠鬥士Z》官方作品頁](https://www.bandainamcoent.com/games/dragon-ball-fighterz)
2. 對照 [03:56](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=236) 的外輪廓和 [04:48](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=288) 的衣縫。前者跟模型輪廓和視角走，後者貼在表面。兩種線各有一套控制方式。
3. 在 [05:55](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=355) 的動畫比較裡看停留長度，再到 [06:30](https://www.bilibili.com/video/BV1WfxZ6uEv4/?t=390) 看姿勢本身如何改變比例。即使使用相同幀率，停留時機和剪影也會改變動作重量感。
