# 《原神》PS4 渲染技術筆記

原始影片由 Unity Korea 上傳。下方時間碼對應這個 YouTube 版本。[유나이트 서울 2020 - 원신 콘솔 플랫폼 개발 경험 및 렌더링 파이프라인 기술 소개 Track3-1](https://www.youtube.com/watch?v=00QugD5u1CU)

弋振中（米哈遊）在 Unite Seoul 2020 介紹《原神》如何把風格化畫面移植到 PS4。整套管線以物理光照維持晝夜與天氣變化下的畫面一致性，再針對陰影、光束、反射和 HDR 各自保留美術控制。以下效能數字是演講當時的個案量測。

點時間碼可跳到原影片。投影片頁碼對應[Unity 官方投影片](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi)。[原始演講](https://www.youtube.com/watch?v=00QugD5u1CU)

## 主機管線與風格化光照

| 時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [02:50](https://www.youtube.com/watch?v=00QugD5u1CU&t=170s) | 主機、手機兩套管線 | 既有 Unity 引擎已針對手機深度客製化，單靠切換目標平台無法交付 PS4 版。團隊讓 PC 和主機共用主機管線，Android 和 iOS 走手機管線，製作內容及選擇技術時同時考量兩邊的成本。 |
| [03:27](https://www.youtube.com/watch?v=00QugD5u1CU&t=207s) | 以 PBR 為基礎的風格化光照 | 晝夜和天氣持續變化，材質得在不同光照下保持一致。團隊用 PBR 建立共同基準，按美術需要調整材質的光照模型。部分模型也允許偏離能量守恆，維持動畫感。 |
| [03:56](https://www.youtube.com/watch?v=00QugD5u1CU&t=236s) | 高解析度渲染與輸出 | 清晰輪廓和細節對動畫風格很重要。當時 PS4 Pro 採原生 4K，標準 PS4 先以 1440p 渲染，再輸出 1080p，讓畫面更銳利。 |
| [04:12](https://www.youtube.com/watch?v=00QugD5u1CU&t=252s) | 運算著色器與非同步運算 | 超過半數主機管線功能使用運算著色器。支援非同步運算時，可讓合適的運算工作與其他 GPU 工作重疊，減少它們落在單幀關鍵路徑上的時間。 |
| [06:34](https://www.youtube.com/watch?v=00QugD5u1CU&t=394s) | 渲染功能取捨 | 開發期短，團隊優先選成熟、適合美術風格、可與其他功能互相配合的做法。後面的上帝光和 HDR 都用這個原則解決具體的畫面問題。 |

[官方投影片第 9–14 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p9)

## 方向光陰影

| 時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [07:20](https://www.youtube.com/watch?v=00QugD5u1CU&t=440s)、[08:16](https://www.youtube.com/watch?v=00QugD5u1CU&t=496s) | 八級級聯陰影貼圖（CSM） | 開放世界要同時保留近處葉片的陰影細節，以及遠景陰影覆蓋。分成八級處理不同距離，演講展示約 800 公尺的覆蓋。增加級數也會增加 CPU 繪製呼叫和 GPU 陰影運算。 |
| [08:50](https://www.youtube.com/watch?v=00QugD5u1CU&t=530s) | 級聯快取與交錯更新 | 八級每幀重畫會增加 CPU 負擔。近端四級每幀更新，遠端四級每幀輪流更新其中一級，因此每幀只更新五級。演講給的更新界線是每級至少每八幀更新一次。 |
| [09:15](https://www.youtube.com/watch?v=00QugD5u1CU&t=555s) | 螢幕空間軟陰影與卜瓦松圓盤取樣 | 硬邊和重複取樣圖樣在畫面上很顯眼。陰影階段使用 11 個卜瓦松圓盤取樣點，並旋轉取樣圖樣，讓半影更柔和、減少條帶。 |
| [10:00](https://www.youtube.com/watch?v=00QugD5u1CU&t=600s)、[10:48](https://www.youtube.com/watch?v=00QugD5u1CU&t=648s) | 低解析度半影遮罩 | 軟陰影只在明暗交界有用。用寬、高各四分之一的遮罩分類陰影內、陰影外與半影，昂貴的取樣只留給半影。用少量取樣估計每個 4 × 4 區塊，再模糊遮罩擴張邊緣，補回漏標。投影片顯示陰影階段約從 2.0–2.6 ms 降到 1.3–1.7 ms。影片另報告遮罩生成約 0.3 ms。 |

[官方投影片第 17–25 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p17)

## 環境光遮蔽（AO）

| 時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [12:29](https://www.youtube.com/watch?v=00QugD5u1CU&t=749s)、[12:54](https://www.youtube.com/watch?v=00QugD5u1CU&t=774s) | HBAO（基於地平線的環境光遮蔽） | 人物或物件進入大範圍陰影後，少了接觸處的細微暗部就容易顯得浮起來。HBAO 用螢幕空間資料補小範圍遮蔽，例如接縫或物件與地面的接觸處。 |
| [13:24](https://www.youtube.com/watch?v=00QugD5u1CU&t=804s) | AO Volume | 桌椅投到地面的大範圍遮蔽超出 HBAO 主要負責的範圍。在靜態物件的局部座標中離線烘焙遮蔽資料，存到體積貼圖供執行時取樣。 |
| [14:24](https://www.youtube.com/watch?v=00QugD5u1CU&t=864s)、[15:08](https://www.youtube.com/watch?v=00QugD5u1CU&t=908s) | Capsule AO | 動畫角色不斷變形，固定烘焙的遮蔽跟不上姿勢。用綁在骨架上的膠囊體近似四肢與軀幹，隨動畫更新，計算方向性遮蔽，讓地板與附近牆面都有柔和的角色影響。 |
| [16:05](https://www.youtube.com/watch?v=00QugD5u1CU&t=965s)、[16:53](https://www.youtube.com/watch?v=00QugD5u1CU&t=1013s) | 半解析度 AO 與合併濾波 | 降解析度能省運算，但會帶來雜訊，也可能讓值跨物件邊界滲漏。用雙邊濾波模糊與升採樣，再把原本兩次模糊與一次升採樣合併成一次 Compute Pass。透過 LDS 共用中間結果，每個執行緒輸出四個像素，減少重算與反覆讀寫。 |

[官方投影片第 26–35 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p26)

## 局部光照與陰影貼圖

| 時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [17:23](https://www.youtube.com/watch?v=00QugD5u1CU&t=1043s) | 分群延遲光照（Clustered Deferred Lighting） | 視野內有大量燈光時，逐像素遍歷全部光源代價太高。把螢幕切成 64 × 64 像素圖塊，沿深度再切成 16 群，依群組篩出相關光源。演講的系統支援視野內最多 1,024 盞局部光。 |
| [18:19](https://www.youtube.com/watch?v=00QugD5u1CU&t=1099s) | 局部光的混合陰影 | 多盞燈同時投影時，全量重繪靜態場景成本太高。將預烘焙的靜態陰影與每幀動態物件的陰影合成。貼圖解析度依距離和優先順序分配，支援近百盞局部光即時投影。 |
| [18:55](https://www.youtube.com/watch?v=00QugD5u1CU&t=1135s)、[19:45](https://www.youtube.com/watch?v=00QugD5u1CU&t=1185s) | 自訂深度壓縮與四元樹 | 大量預烘焙的陰影貼圖增加容量和 I/O，通用 BCn 又會造成可見瑕疵。每個 2 × 2 深度區塊以深度平面或浮點數封裝成 32 位元，高精度為 64 位元，再用四元樹合併資料。標準 PS4 用 Compute Shader 解壓一張 1K × 1K 貼圖約 0.05 ms。 |
| [21:46](https://www.youtube.com/watch?v=00QugD5u1CU&t=1306s) | 壓縮精度的取捨 | 示例中一張 2K × 2K 貼圖由 8 MB 降為預設精度約 274.4 KB，或高精度約 583.5 KB。高精度的壓縮比仍約 14:1，這組畫面與未壓縮版本肉眼難以區分。 |

[官方投影片第 36–44 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p36)

## 體積霧與上帝光

| 時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [22:35](https://www.youtube.com/watch?v=00QugD5u1CU&t=1355s)、[23:47](https://www.youtube.com/watch?v=00QugD5u1CU&t=1427s) | 視錐體素霧 | 霧需要有空間深度，並能被局部燈光和投影貼圖照亮。將視錐切成與光照分群對齊的體素，注入分區霧參數與局部光，沿視線步進累積散射。用時序濾波混合多幀，減少閃爍。講者報告 PS4 Pro 的 GPU 開銷通常低於 1 ms。 |
| [24:21](https://www.youtube.com/watch?v=00QugD5u1CU&t=1461s)、[24:47](https://www.youtube.com/watch?v=00QugD5u1CU&t=1487s) | 獨立上帝光階段（God Ray Pass） | 體素霧的解析度讓光束邊緣偏糊，而且提高光束強度往往得加濃霧。另用寬、高各減半的貼圖做光線步進，最多取樣五級陰影，再依美術參數和霧混合。光束可以更銳利，畫面仍維持清透。 |

[官方投影片第 45–53 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p45)

## 影像式光照與反射

| 時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [26:50](https://www.youtube.com/watch?v=00QugD5u1CU&t=1610s)、[27:30](https://www.youtube.com/watch?v=00QugD5u1CU&t=1650s) | 即時反射探針 | 固定烘焙的立方體貼圖跟不上晝夜和天氣。離線保留場景的精簡 G-buffer，執行時以當前光照重新打光，再逐級卷積產生 mip 鏈並做 BC6H 壓縮。運算著色器可同時計算六面，探針跨幀輪流更新，分攤成本。 |
| [29:14](https://www.youtube.com/watch?v=00QugD5u1CU&t=1754s) | 環境光探針與球諧函數（SH） | 需要隨環境變動的低頻漫射光照。從已重新打光的反射資料擷取環境光，存成三個頻帶的球諧係數，隨對應反射探針一起更新。 |
| [29:52](https://www.youtube.com/watch?v=00QugD5u1CU&t=1792s)、[30:13](https://www.youtube.com/watch?v=00QugD5u1CU&t=1813s) | 陰影 SH 與局部光 SH | 探針重打光若沒有陰影，遮陰地面也可能太亮。離線烘焙不同時段的陰影 SH，執行時按時間插值後套用。局部光資訊同樣可編成 SH，給暗處低成本的反彈光。 |
| [31:37](https://www.youtube.com/watch?v=00QugD5u1CU&t=1897s) | 室內遮罩與探針分類 | 室外和室內光照差異大，套錯探針會讓室內偏色或偏亮。用美術放置的室內網格標記像素，套用正確的室內或室外探針，兩者之間做平滑過渡。 |
| [33:00](https://www.youtube.com/watch?v=00QugD5u1CU&t=1980s)、[34:00](https://www.youtube.com/watch?v=00QugD5u1CU&t=2040s) | 螢幕空間反射（SSR）與延遲反射合成 | 探針提供環境反射，SSR 補上畫面內的即時細節。以 Hi-Z 加速追蹤，取樣前幀色彩並做時序濾波。PS4 Pro 的 SSR 約 1.5 ms。沒有 SSR 結果時仍有探針反射，合成時納入 AO 以抑制漏光。 |

[官方投影片第 54–74 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p54)

## HDR 顯示與色彩

| 時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [34:18](https://www.youtube.com/watch?v=00QugD5u1CU&t=2058s)、[35:44](https://www.youtube.com/watch?v=00QugD5u1CU&t=2144s) | HDR10 輸出路徑 | 支援標準動態範圍以外的亮度和色域。HDR 使用 PQ（SMPTE ST 2084）及 Rec. 2020，原 SDR 色調映射與色彩分級改為 HDR 路徑。UI 先畫到獨立目標，再與場景合成。10,000 尼特是 PQ 的訊號上限。 |
| [36:49](https://www.youtube.com/watch?v=00QugD5u1CU&t=2209s) | 寬色域調色 LUT | 讓美術能製作 HDR 調色，同時控制執行成本。將白平衡、HDR 色彩分級和色域擴張合併到小型查找表的生成階段。演講報告約 0.05 ms。 |
| [37:17](https://www.youtube.com/watch?v=00QugD5u1CU&t=2237s) | HDR 與 SDR 亮度範圍對齊 | 通用 ACES RRT + ODT 的畫面不符合本作風格。團隊曾先把 HDR 畫面與色調映射結果混合，讓 SDR 亮度範圍內的畫面更接近原版。後來把色調映射曲線合入 LUT，省去這道混合。 |
| [37:46](https://www.youtube.com/watch?v=00QugD5u1CU&t=2266s)、[38:20](https://www.youtube.com/watch?v=00QugD5u1CU&t=2300s) | 以 OOTF 模擬 SDR 的亮度差 | SDR 的 sRGB 編碼與 BT.1886 顯示響應不會精確互相抵消，HDR 的 PQ 編碼和反向 PQ 則會。HDR 末端加一條 OOTF 曲線，模擬該差異，特別是讓低亮度區域更接近 SDR。 |
| [38:49](https://www.youtube.com/watch?v=00QugD5u1CU&t=2329s)、[40:52](https://www.youtube.com/watch?v=00QugD5u1CU&t=2452s) | 在著色器重現色相偏移 | 美術依靠 SDR 色調映射讓高亮橘色火焰的中心偏黃。純保色相的 HDR 流程會讓它維持橘色。團隊用著色器重現偏移並併入調色 LUT，保留原素材的效果。演講另提到以溫度計算顏色的黑體輻射方案，但本案選擇沿用既有素材的色彩控制。 |

[官方投影片第 75–80 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p75)
