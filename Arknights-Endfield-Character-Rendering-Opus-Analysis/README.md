# 《終末地》角色渲染：社群著色器拆解

原始影片由「一里未研所」上傳。[Opus5.5告诉你终末地是怎么渲染角色的](https://www.bilibili.com/video/BV1zhJC6zELX/)

片中逐層開關洛茜模型的光照、臉影、髮光和描邊，展示社群復現的視覺效果。影片文案由 Opus 5.5 編寫。參數與做法以展示的社群插件為準，商用版本的實作需要另查開發者資料。

本機附中文轉錄字幕及帶時間碼的文字稿。字幕依語音轉錄，重要術語已校正，個別口述仍可能有辨識誤差。

## 技術與時間碼

| 影片時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [00:20](https://www.bilibili.com/video/BV1zhJC6zELX/?t=20) | 社群著色器與固定底色 | 先從只有貼圖、沒有光照的模型出發。紅暈、髮根暗部與衣褶可畫在底色上，但不會隨光源改變。這支影片示範社群重現的著色器。 |
| [01:16](https://www.bilibili.com/video/BV1zhJC6zELX/?t=76) | Lambert 和 Half-Lambert | 面片朝向光源的程度用法線與光向的內積表示。示範將內積重映射到 0–1，提供一個可用來查色帶的值，方便控制背光側。 |
| [01:41](https://www.bilibili.com/video/BV1zhJC6zELX/?t=101) | 漫反射 Ramp | 平滑光照容易讓角色像塑膠模型。用受光量查色帶，把連續明暗分成少量色階，色帶同時控制邊界的過渡。 |
| [02:17](https://www.bilibili.com/video/BV1zhJC6zELX/?t=137) | 皮膚暗部 LUT | 把膚色直接乘暗，容易顯得灰濁。依原膚色查陰影色，讓暗部保留偏暖的色相，模擬插畫上對透光肌膚的表現。 |
| [02:45](https://www.bilibili.com/video/BV1zhJC6zELX/?t=165) | 臉部 SDF 陰影 | 一般法線運算可能在鼻翼、嘴角和下巴產生碎陰影。以美術設計的臉部遮罩控制亮暗切換，讓光線角度推進時，陰影沿預定形狀移動。 |
| [03:28](https://www.bilibili.com/video/BV1zhJC6zELX/?t=208) | 以頭部座標判斷光向 | 世界固定的左右方向會使角色轉頭後的臉影失準。以頭部前向和水平光向算角度，再按光的來向鏡像使用遮罩，使陰影隨頭轉動。 |
| [04:04](https://www.bilibili.com/video/BV1zhJC6zELX/?t=244) | Overlay Shadow 疊加陰影 | 瀏海和眼白上的柔暗輪廓不一定要等待即時投影。示範用帶灰階透明度的薄片壓暗額頭或眼白，維持指定的動畫形狀。 |
| [04:34](https://www.bilibili.com/video/BV1zhJC6zELX/?t=274) | 切線方向的各向異性髮光 | 一般鏡面亮點難以表達髮絲沿固定方向形成的光帶。用髮絲切線控制各向異性高光，並以噪聲打散位置、疊加寬度與色調不同的第二層高光。 |
| [05:25](https://www.bilibili.com/video/BV1zhJC6zELX/?t=325) | 髮光位置與明暗收邊 | 高光直接計算時可能太柔或位置難以美術化。用色帶收出較乾淨的邊，再調整兩層髮光的位置。轉動鏡頭可比較即時亮帶與畫在底色上的亮帶。 |
| [06:08](https://www.bilibili.com/video/BV1zhJC6zELX/?t=368) | PBR 服裝加上卡通明暗 | 布料和金屬需要不同的反射反應。用法線貼圖保留褶皺，用金屬度、光滑度和打包貼圖指定各部位材質，再以 Ramp 整理明暗交界，使服裝和臉部畫風一致。 |
| [07:53](https://www.bilibili.com/video/BV1zhJC6zELX/?t=473) | 眼睛 MatCap 與獨立高光 | 虹膜面積小，仍是表情焦點。根據視圖空間法線查 MatCap 以取得可控的鏡片反光，再加一層高光，讓眼睛保有立體感。 |
| [08:32](https://www.bilibili.com/video/BV1zhJC6zELX/?t=512) | 反向殼、平滑法線與線寬 | 外擴描邊在硬邊容易裂開，用另一套平滑法線控制外擴可減少開縫。依鏡頭距離修正線寬，再從底色衍生深色線條，避免遠看消失、近看過粗。 |
| [09:45](https://www.bilibili.com/video/BV1zhJC6zELX/?t=585) | 克制使用 Bloom 與調色 | 後製能把角色統一到同一色調，但過量泛光會把皮膚和白衣糊在一起。逐層開關曝光、泛光與調色 LUT，比較有沒有吃掉先前刻意保留的明暗分層。 |

## 技術參考

[Unity URP Lit Shader Graph 參考](https://docs.unity3d.com/en/current/Manual/urp/prebuilt-shader-graphs-urp-lit.html) · [《罪惡裝備 Xrd》官方簡報的外輪廓說明](https://media.gdcvault.com/gdc2015/presentations/Motomura_Junya_GuiltyGearXrd.pdf#page=24)
