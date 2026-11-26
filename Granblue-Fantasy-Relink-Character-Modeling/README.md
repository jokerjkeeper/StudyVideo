# 《碧藍幻想 Relink》插畫式角色建模筆記

原始影片由 Cygames公式チャンネル 上傳。時間碼對應以下 YouTube 版本。[『GRANBLUE FANTASY: Relink』イラストを再現する為のキャラクターモデル制作事例](https://www.youtube.com/watch?v=rrDDT2nKseg)

Cygames Tech Conference 2024 的開發者演講，說明如何在 PBR 的材質基礎上保留插畫的線、光、筆觸與誇張變形，同時因應四人合作戰鬥的量產與效能成本。

本機附日文自動字幕、YouTube 繁中自動翻譯。

## 技術與時間碼

| 影片時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [05:40](https://www.youtube.com/watch?v=rrDDT2nKseg&t=340s) | 反向殼插畫描邊 | 角色與背景需要清楚分離，線條也要保留插畫感。用外擴後的背面形成輪廓，再分別處理線寬、線條質感和顏色。 |
| [07:10](https://www.youtube.com/watch?v=rrDDT2nKseg&t=430s) | 近遠距線寬 | 近看合適的線到了遠處可能細到消失，反過來調整遠處也會使近景過粗。為不同距離設定線寬並過渡，讓主要輪廓在戰鬥和特寫都清楚。 |
| [08:10](https://www.youtube.com/watch?v=rrDDT2nKseg&t=490s) | 鉛筆質感與色線 | 均一的深色硬線在插畫裡容易顯得突兀。讓線帶有鉛筆般的質感，並像插畫的色線一樣依色調調整。夕陽特寫可以再調線色，以免深線浮在暖色場景上。 |
| [09:20](https://www.youtube.com/watch?v=rrDDT2nKseg&t=560s) | 臉部以視角切換的邊緣光 | 臉部高光若只按幾何自動算，容易跑到不適合的部位。將兩側臉頰及鼻子兩側的遮罩分放在 RGBA，依攝影機與角色相對方向切換使用。 |
| [11:00](https://www.youtube.com/watch?v=rrDDT2nKseg&t=660s) | 身體依輪廓與光向計算邊緣光 | 身體需要隨曲面讓側緣自然提亮。取模型法線與視線向量的內積找出接近輪廓的區域，並納入方向光資訊，讓色彩和強度能依環境調。 |
| [11:50](https://www.youtube.com/watch?v=rrDDT2nKseg&t=710s) | 陰影中的排線 | 把手繪排線直接烘焙到底色，亮處和暗處看起來會一樣重。使用獨立 Alpha 遮罩控制排線，暗部加深，讓筆觸能隨照明改變。 |
| [14:00](https://www.youtube.com/watch?v=rrDDT2nKseg&t=840s) | 從插畫反推材質配色 | 插畫裡的布料、皮革與金屬常靠特定顏色和亮斑區分。把需要固定的色變放進貼圖，把要隨燈光移動的反光交給著色器，讓材料在不同場景仍可辨識。 |
| [15:20](https://www.youtube.com/watch?v=rrDDT2nKseg&t=920s) | 各向異性反射與點狀高光 | 髮束或部分材料的亮帶有方向，較濕亮的表面則需要集中高光。用各向異性反射塑造沿特定方向的亮帶，再用較窄的點狀高光表達濕潤感。髮束反光的寬度與強度可調。 |
| [16:50](https://www.youtube.com/watch?v=rrDDT2nKseg&t=1010s) | 重整臉部陰影形狀 | 直接依幾何打光可能讓眉間、鼻樑出現過重的陰影。調整臉部著色以減少這些搶眼的暗面，保留插畫式表情。 |
| [17:30](https://www.youtube.com/watch?v=rrDDT2nKseg&t=1050s) | 虹膜分層與視差模擬折射 | 眼睛形狀、高光和虹膜深度影響角色神情。重建插畫裡的眼睛層次，並用視差著色器模擬折射感，兼顧畫面的立體感和演講提到的效能、繪製順序限制。 |
| [19:10](https://www.youtube.com/watch?v=rrDDT2nKseg&t=1150s) | 演出時使用武器變形 | 剛硬武器在快速揮動時會少了動畫裡的弧線和速度感。演出中讓模型變形，並用工具輔助產生可變形的幾何，減少逐把武器手工建置的負擔。 |
| [21:10](https://www.youtube.com/watch?v=rrDDT2nKseg&t=1270s) | 四人合作的資產預算 | 角色、武器和 NPC 同時出現，每個資產預算只獨立估算會漏算總成本。先估最重的同屏組合，再把較多面數分配給輪廓和鏡頭容易看見的部位。 |
| [27:10](https://www.youtube.com/watch?v=rrDDT2nKseg&t=1630s) | 依畫面占比切換 LOD | 只靠距離會讓體型大的角色過早顯出低模稜角。依角色在畫面中的占比選 LOD。以 Simplygon 產生簡化模型，再用內部工具鎖住重要形狀。 |
| [29:40](https://www.youtube.com/watch?v=rrDDT2nKseg&t=1780s) | 貼圖解析度預設 | 反覆手改原始貼圖解析度會拖慢最佳化。用預設調整實際使用的解析度，優先縮減畫面較不敏感的部分，例如大量城鎮 NPC。 |
| [32:40](https://www.youtube.com/watch?v=rrDDT2nKseg&t=1960s) | 用暫定模型先定輪廓 | 如果到了高模階段才推翻外形，細節會一起重做。先用暫定模型核對服裝體積、比例與剪影，再往高模和遊戲模型推進。 |
| [36:10](https://www.youtube.com/watch?v=rrDDT2nKseg&t=2170s) | 共用基礎網格與規格 | 大量角色從零建模會讓品質和工時不穩定。從共用的人體與臉部網格起步，再依角色體型修改，並預先規定模型與材質的交付方式。 |
| [37:50](https://www.youtube.com/watch?v=rrDDT2nKseg&t=2270s) | 在 Substance Painter 預覽遊戲著色 | 若每次畫完貼圖都要輸出到引擎才看到排線、陰影或邊緣光，就會增加反覆匯出的成本。用範本配置對應部件的著色器和通道，在 Painter 裡先接近遊戲結果。 |

## 技術參考

[Cygames 官方講義](https://speakerdeck.com/cygames/granblue-fantasy-relink-irasutowozai-xian-suruwei-nokiyarakutamoderuzhi-zuo-shi-li)
