# 《聖火降魔錄 Engage》視覺與動畫筆記

原始影片由 Faerghast 上傳。時間碼對應以下 YouTube 版本。[Fire Emblem Engage Set a Visual Quality Standard](https://www.youtube.com/watch?v=LX2ruU1WXSA)

Faerghast 整理 Intelligent Systems 的訪談並加入觀察。表中的影片時間可直接回看，虹膜遮罩、頂點色描邊與材質 Ramp 的細節由原訪談補充。

本機附英文自動字幕。

## 技術與時間碼

| 影片時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [01:20](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=80s) | 以 3D 模型支援角色介面 | 狀態畫面改用可動的 3D 模型，角色必須承受特寫。團隊兼顧近景臉部、衣服細節與全身輪廓，讓介面和實機都維持同一個角色印象。 |
| [02:20](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=140s) | 獨立眼睛著色器與虹膜分層 | 單純替換眼睛顏色會削弱角色差異。團隊拆解 Mika Pikazo 的虹膜畫法，建立共通規格，並用最多四層遮罩各自控制顏色、特徵或動態。 |
| [02:40](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=160s) | 反向殼描邊與局部線寬 | 固定粗細會讓五官或小裝飾顯得過重。原訪談指出使用背面法，以頂點色紅色通道控制線寬，並讓線色取附近貼圖的顏色來調色。 |
| [02:50](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=170s) | 前髮下的眉毛與睫毛 | 瀏海若完全遮住眉眼，表情閱讀會受影響。團隊提供把眉毛、睫毛顯示到前髮前方的做法，也能依角色設計分部位設定透明。 |
| [02:55](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=175s) | 卡通色階與 PBR 材質資訊 | 大面積的硬色階讓角色像插畫，衣物又需要保留材質感。原訪談說明用法線貼圖在明暗交界保留布料細節，亮側可簡化，並以手繪 Ramp 控制金屬明暗波形。 |
| [03:26](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=206s) | 演出動捕與戰鬥手調 | 談話和過場要有自然的表演，超常的格鬥動作則要有誇張而清楚的姿勢。前者可用動捕，戰鬥動作由動畫師手動製作。 |
| [04:05](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=245s) | 特殊必殺技分鏡 | 角色和鏡頭同時移動時，難以單靠文字描述掌握畫面節點。影片用 Twin Strike 的分鏡示例，讓鏡位、出招、特效和打擊節奏先對齊。 |
| [04:56](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=296s) | 依角色狀態改變戰鬥動作 | 若所有體型、職業或成長階段都播同樣節奏，就不容易看出差異。調整準備動作、連擊節奏及回避或格擋的反應，讓戰鬥能力可從動作讀出。 |
| [05:12](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=312s) | 髮束與披風碰撞處理 | 長髮和可換服裝容易互穿。團隊按角色和服裝組合準備差異設定並調整關節。原訪談指出晃動部件使用 Spring Bone。 |
| [05:44](https://www.youtube.com/watch?v=LX2ruU1WXSA&t=344s) | 用灰模與色光設定驗證地圖 | 地圖既要能玩，又要有自己的氣氛。從平面草圖做成灰模，先核對地形和鏡頭，再訂色彩與燈光，避免完成細部後才發現路線或構圖不清楚。 |

## 技術參考

[Intelligent Systems 開發者訪談（CGWORLD）](https://cgworld.jp/article/202311-cgw303-fee.html)
