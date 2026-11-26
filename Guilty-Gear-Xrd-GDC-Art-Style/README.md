# 《罪惡裝備 Xrd》GDC 原講筆記

原始影片由 GDC Festival of Gaming 上傳。時間碼對應以下 YouTube 版本。[GuiltyGearXrd's Art Style : The X Factor Between 2D and 3D](https://www.youtube.com/watch?v=yhGjCzxJV3E)

這場演講由開發者本村純也說明《Xrd》的角色製作。核心做法是讓美術能直接控制明暗交界、線條與姿勢，並在建模及動畫階段預覽畫面。36:53 後是觀眾問答。

本機字幕為英文自動字幕。

## 技術與時間碼

| 影片時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [05:01](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=301s) | 用 3D 模型延續 2D 畫風 | 固定視角的精靈圖難以支援必殺技轉鏡和極近特寫。保留 3D 模型的透視與攝影機自由度，再對著色、輪廓和動畫節奏分別做風格化。 |
| [10:40](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=640s) | 即時著色預覽 | 如果建模師只能看裸模，就很難知道顏色、明暗和剪影在遊戲裡會怎樣互相影響。在製作工具裡預覽接近遊戲的著色結果，當場修改並核對。 |
| [14:20](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=860s) | 高面數網格與頂點資料 | 極近特寫會放大貼圖像素。把細節建成幾何，並用頂點法線、顏色和 UV 存資料。演講報告角色平均約 4 萬三角形，數量來自該作的特寫需求。 |
| [16:20](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=980s) | 賽璐璐分界 | 以光向與法線的內積，與閾值比較後決定亮面或暗面。頂點色可局部偏移閾值，讓美術控制應該更容易入陰的地方。 |
| [18:06](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=1086s) | 角色獨立光向 | 場景共用的光向可能使部分角色臉部或站姿不清楚。戰鬥為各角色維持專屬光向，過場可以隨姿勢調整。 |
| [18:34](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=1114s) | 自訂頂點法線 | 幾何上的小起伏在硬分界著色裡容易變成明顯斑塊。手調法線，尤其是臉部，能維持想要的陰影形狀和外部輪廓。 |
| [21:50](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=1310s) | 亮暗配色 | 用底色貼圖定亮面，乘上色調貼圖取得暗面。美術可以單獨決定陰影的色相與亮度，保留不同材質的差異。 |
| [22:59](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=1379s) | 反向殼外輪廓 | 將第二層網格沿法線外擴、剔除正面，讓外緣的背面形成黑線。線寬可依距離和頂點色調整，深度偏移可抑制不需要的線。 |
| [24:17](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=1457s) | 軸對齊的表面線條 | 衣縫等內線在極近鏡頭容易出現鋸齒。貼圖上的色條與 UV 軸對齊，以重疊量控制線寬。做法允許部分 UV 變形，因為該貼圖主要用來提供線條或顏色資料。 |
| [29:03](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=1743s) | 有限動畫與逐格擺姿勢 | 平滑插值讓動作更像連續轉動的 3D 模型。把每張姿勢當成一格可控制停留時間的畫稿，保留動作的重拍與跳躍感。 |
| [30:35](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=1835s) | 高控制度骨架與縮放 | 用約 400–600 根骨骼支援逐格控制小部位。縮放可以做擠壓拉伸、強化透視，也可以讓部分元素隱藏或出現。 |
| [31:46](https://www.youtube.com/watch?v=yhGjCzxJV3E&t=1906s) | 逐姿勢改變畫面比例 | 只改姿勢節奏仍可能看出模型在透視中保持固定形狀。逐格調整四肢、表情與光照，讓每個剪影服務當下的構圖。 |

## 技術參考

[開發者的 GDC 2015 原始投影片](https://media.gdcvault.com/gdc2015/presentations/Motomura_Junya_GuiltyGearXrd.pdf)
