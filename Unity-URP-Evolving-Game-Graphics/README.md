# Unity URP 提升遊戲畫面｜Unite Now 2020

原始影片由 Unity官方 上傳。[\[Unity 教程\]- （中文字幕）使用URP提升游戏画面效果](https://www.bilibili.com/video/BV1fK4y1a78s/)

講者 Andy Touch 用 Boat Attack 和 Unity 的 URP 範例示範，說明如何依平台調整畫質，以及如何用渲染階段與相機堆疊決定特效作用的物件和畫面。以下時間碼對應 Bilibili 版。

## 渲染技術與時間碼

| 時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [16:56](https://www.bilibili.com/video/BV1fK4y1a78s/?t=1016) | 按畫質設定切換 URP Asset | 不同平台有不同的陰影與抗鋸齒預算。讓低、中、高畫質各自指定 URP Asset，在同一套關卡中分別控制 HDR、抗鋸齒及陰影解析度。 |
| [18:24](https://www.bilibili.com/video/BV1fK4y1a78s/?t=1104) | 即時方向光與晝夜變化 | 如果場景只依賴固定時間的光照，太陽位置改變時就不會更新主光陰影。Boat Attack 用可旋轉的方向光示範即時切換夜間、黎明與白天。 |
| [19:21](https://www.bilibili.com/video/BV1fK4y1a78s/?t=1161) | Shader Graph 三平面地表材質 | 岩石移動或重複擺放後，青苔和水線如果烘死在貼圖上就需要重做。用三平面投影及位置相關的著色規則，讓草覆蓋和水線隨岩石的位置調整。 |
| [20:28](https://www.bilibili.com/video/BV1fK4y1a78s/?t=1228) | 水面幾何與反射選擇 | 水面要表現浪形和倒影，各平台能承受的成本不同。範例可切換頂點位移或細分，以及預烘焙立方體貼圖、反射探針或平面反射。平面反射會補出當前鏡頭可見位置關係下的場景倒影。 |
| [22:28](https://www.bilibili.com/video/BV1fK4y1a78s/?t=1348) | Renderer Feature 與自訂 Render Pass | 單改物件材質很難控制整個畫面何時畫、畫哪些物件。把特效註冊為 Renderer Feature，按事件插入自訂 Pass，也可為不同相機指定不同 Renderer。 |
| [25:28](https://www.bilibili.com/video/BV1fK4y1a78s/?t=1528) | 水下焦散與交界深度 | 只有波動的水面網格，水底缺少焦散，岸邊也少了水面與場景相交的細節。水的 Renderer Feature 提供焦散與深度效果，示範可用除錯模式分別檢查。 |
| [28:18](https://www.bilibili.com/video/BV1fK4y1a78s/?t=1698) | Sobel 描邊與插入時機 | 同一描邊有時只想處理石頭等不透明物件，有時也要考慮天空和水。以 Blit 套用 Sobel 描邊，從不透明物件後移到天空盒或透明物件後，改變效果能讀到的畫面內容。 |
| [32:17](https://www.bilibili.com/video/BV1fK4y1a78s/?t=1937) | 色階化、像素化與距離漸層霧 | 風格化效果需要控制影響範圍，水面也可能需要和地形不同的處理。分別設定 Posterize、像素化和距離霧 Pass，把需要包含透明水面的效果排在透明階段之後。 |
| [36:49](https://www.bilibili.com/video/BV1fK4y1a78s/?t=2209) | Frame Debugger 檢查渲染順序 | 多個特效疊加時，只看最後畫面很難辨識是 Pass 遺漏還是順序不對。逐步查看一幀的繪製，關閉個別 Feature 核對預期事件及輸出。 |
| [42:58](https://www.bilibili.com/video/BV1fK4y1a78s/?t=2578) | Base／Overlay 相機堆疊 | 世界、駕駛艙和 UI 需要不同的視角、可見層和合成順序。Base 相機畫主場景，將駕駛艙和 UI 的 Overlay 相機依序加入堆疊，分別控制各自的層和視野。 |
| [50:49](https://www.bilibili.com/video/BV1fK4y1a78s/?t=3049) | Opaque Texture 與 Scene Color | 要做駕駛艙螢幕的扭曲干擾，需要讀取已畫好的背景。啟用 Opaque Texture，再用 Shader Graph 的 Scene Color 以偏移 UV 取樣。來源是已繪製的不透明內容。 |
| [52:31](https://www.bilibili.com/video/BV1fK4y1a78s/?t=3151) | 第一人稱前景相機 | 手持模型靠近牆面時，容易被場景深度裁切，同一相機的視野也會限制前景構圖。用 Overlay 相機單獨畫手持模型，調整它的視野，並可指定自己的描邊 Renderer。 |
| [55:49](https://www.bilibili.com/video/BV1fK4y1a78s/?t=3349) | Render Objects 與遮擋提示 | 只希望遮在牆後的指定物件顯示提示效果。範例以 Layer Mask 選取敵人，覆寫材質和深度比較，再用獨立 Pass 繪製被遮住的部分。 |
| [58:54](https://www.bilibili.com/video/BV1fK4y1a78s/?t=3534) | SSAO、延遲渲染與材質功能預覽 | 接觸處需要遮蔽感，部分場景也需要更多燈光和材質選項。講者展示 SSAO 與早期延遲渲染，並預告 Shadowmask、Clear Coat 及反射探針改進。這些是演講當時的功能預覽。 |

## 版本與來源

示範使用 Unity 2019.3。若拿範例對照 Unity 6，新功能的自訂渲染階段可參考 Render Graph 手冊。[Unity 6 自訂 Render Pass](https://docs.unity3d.com/6000.0/Documentation/Manual/urp/render-graph-write-render-pass.html)

Bilibili 版本的中文字幕直接印在畫面上。本機保留原影片和獨立字幕檔。

[Unity 英文原講](https://www.youtube.com/watch?v=Bvl9rCVbMas) · [Boat Attack 範例](https://github.com/Unity-Technologies/BoatAttack) · [Universal Rendering Examples](https://github.com/Unity-Technologies/UniversalRenderingExamples)
