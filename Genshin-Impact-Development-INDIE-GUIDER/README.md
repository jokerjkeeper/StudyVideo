# 《原神》開發及其渲染技術｜專題與官方演講索引

原文（INDIE-GUIDER，編輯 ian）｜[〖開發經驗分享〗專題系列－《原神》開發及其渲染技術](https://indie-guider.games/article/DevStudy-Genshin-Impact)

書籤頁是開發經驗專題入口，公開可讀部分主要提供導讀。下表用弋振中在 Unity 開發者活動分享的《原神》主機管線當技術來源，頁碼可直接對照投影片。[Unity 官方投影片](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi) · [Unity Korea 原講影片](https://www.youtube.com/watch?v=00QugD5u1CU)

| 官方投影片 | 技術 | 解決的問題 | 做法與取捨 |
| --- | --- | --- | --- |
| [第 9–14 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p9) | 分平台管線與風格化 PBR | 主機需要保留高畫質，手機則受不同算力條件限制，材質還要能隨天氣與時間變化。 | 依平台分配渲染功能，用 PBR 維持光照一致性，再給材質保留風格化控制。 |
| [第 17–25 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p17) | 級聯陰影、交錯更新與半影遮罩 | 近處要清楚、遠處要有覆蓋，每幀重畫所有級聯和對全畫面做軟陰影過於昂貴。 | 遠級聯輪流更新，再以低解析度遮罩找出半影，把昂貴取樣集中在明暗交界。 |
| [第 26–35 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p26) | HBAO、AO Volume 與 Capsule AO | 小接縫、靜態大物件及會動的角色需要不同尺度的接觸暗部。 | 用螢幕空間 HBAO 補細節，以烘焙體積資料處理靜態遮蔽，以骨架膠囊近似動畫角色。 |
| [第 36–44 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p36) | 分群延遲光照與混合陰影 | 多盞局部光若逐像素全算並反覆重畫靜態場景，燈光成本會快速增加。 | 對螢幕區塊與深度分群，限制每個群組要評估的燈光，將壓縮的靜態陰影與動態陰影合成。 |
| [第 45–53 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p45) | 視錐體素霧與獨立上帝光 | 整體霧要有深度，局部光束又需要清楚，單靠提高霧量會讓畫面混濁。 | 在視錐體素計算霧與局部燈光，再以獨立的低解析度步進補出方向光束。 |
| [第 54–74 頁](https://www.docswell.com/s/UnityJapan/KWRPQ5-210617-unity-dojo20211mihoyozhenzhongyi#p54) | 可重打光的反射探針與 SSR | 烘焙反射跟不上天氣和晝夜，SSR 又看不到畫面外的物件。 | 分幀更新環境探針，並用 SSR 補畫面內的動態細節。離開螢幕取樣範圍時還有探針作為反射來源。 |
