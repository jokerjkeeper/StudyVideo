# 《龍珠鬥士Z》的打光、動畫與鏡頭

原始影片由 Ken Xyro 上傳。時間碼對應以下 YouTube 版本。[Dragon Ball FighterZ "Good Graphics" Explained](https://www.youtube.com/watch?v=UA1rR72ivXg)

Ken Xyro 從遊戲畫面出發，拆解《龍珠鬥士Z》如何接近動畫原作。以下記錄影片提出的做法與畫面比較。角色打光的適用範圍和實作細節，仍以該作的開發者資料為準。

本機附英文自動字幕。

## 技術與時間碼

| 影片時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [02:38](https://www.youtube.com/watch?v=UA1rR72ivXg&t=158s) | 場景和角色分開打光 | 如果完全依賴場景光，換場地後角色臉部和衣服的陰影可能差很多。影片觀察到角色在不同場景仍維持相近的明暗配置，有助於保留動畫原作的色塊和辨識度。 |
| [03:12](https://www.youtube.com/watch?v=UA1rR72ivXg&t=192s) | 按鏡頭調整角色明暗 | 必殺技特寫有固定的表情和構圖，角色的一般打光未必適用。片中提出對照原動畫的光影，再依演出手調角色亮面和暗面。 |
| [04:00](https://www.youtube.com/watch?v=UA1rR72ivXg&t=240s) | 角色與環境的亮度平衡 | 角色在暗處如果仍像在戶外一樣亮，可能看起來浮在場景外。影片以自由視角的 3D 場景比較，說明固定角色光的美術收益和空間融合上的取捨。 |
| [05:10](https://www.youtube.com/watch?v=UA1rR72ivXg&t=310s) | 把 3D 模型當作動畫畫稿 | 角色幾何可以提供立體透視，動作仍可由動畫師逐姿勢設計。重點是保留每格的剪影和表情，避免轉動時看起來像同一個硬質物件。 |
| [06:10](https://www.youtube.com/watch?v=UA1rR72ivXg&t=370s) | 停格、換姿勢與減少插值 | 影片用手繪與電腦補間做對照。讓關鍵姿勢按需求停留，再直接換姿勢，可以保留原作的動作節奏。這裡說的是姿勢更新頻率，與遊戲畫面幀率是兩個設定。 |
| [07:45](https://www.youtube.com/watch?v=UA1rR72ivXg&t=465s) | 按鏡頭設計形體與透視 | 必殺技的四肢和臉部可配合主鏡頭誇張。用影片中的自由鏡頭比較同一格的不同角度，能看出哪些形變是為了讓指定構圖更有力。 |
| [08:50](https://www.youtube.com/watch?v=UA1rR72ivXg&t=530s) | 統一驗收鏡頭、姿勢與打光 | 只換鏡頭不一定能接近原作。應在最終鏡頭裡一起檢查姿勢、透視和明暗分布，確認畫面是否讀得出角色的情緒與攻擊方向。 |

## 技術參考

[萬代南夢宮作品頁](https://www.bandainamcoent.com/games/dragon-ball-fighterz) 可核對作品與開發合作。另可和 [New Frame Plus 的動畫比較](https://www.youtube.com/watch?v=kZsboyfs-L4&t=860s) 並看，觀察速度線與停格。
