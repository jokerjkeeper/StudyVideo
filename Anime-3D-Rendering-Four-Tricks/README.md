# 二次元 3D 渲染的四種修正方法

原始影片由「海盗CG」上傳。[二次元3D渲染的4种作弊方法](https://www.bilibili.com/video/BV1w7tm65EVk/)

影片用 Blender 示範四種處理方式。常駐陰影補固定細節，SDF 控制臉部隨光變化，自訂法線整理立體曲面，固定鏡頭則可以直接補畫或合成。

本機附中文轉錄字幕及帶時間碼的文字稿。字幕依語音轉錄，重要術語已校正，個別口述仍可能有辨識誤差。

## 技術與時間碼

| 影片時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [01:44](https://www.bilibili.com/video/BV1w7tm65EVk/?t=104) | 常駐陰影 | 即時卡通著色不一定能保留衣物接縫、鼻下或眼瞼這些小暗部。可在底色繪製、用專用材質指定，或用半透明薄片疊暗。它們不跟場景光一起變，宜放在形狀上長期需要的暗處。 |
| [03:46](https://www.bilibili.com/video/BV1w7tm65EVk/?t=226) | 在模型上指定固定陰面 | 若還沒畫完整貼圖，可以選取局部面，分配固定色的陰影材質。想要更細的形狀，可先展 UV 再繪製著色器的 AO 遮罩。這種遮罩和隨燈光移動的陰影可以疊用。 |
| [05:36](https://www.bilibili.com/video/BV1w7tm65EVk/?t=336) | 由多個角度的陰影圖生成臉部 SDF | 臉部陰影需要跟光向變化，又要保持設計過的邊界。分角度先畫出明暗，再合成供著色器比較或查表的紋理。範例主要覆蓋水平轉光，固定光源仰角的限制要一併考慮。 |
| [07:30](https://www.bilibili.com/video/BV1w7tm65EVk/?t=450) | 為臉部陰影圖對齊 UV | 通用 SDF 的三角形暗面、鼻側和中心線需要對上新模型。只展需要受控的臉部區域，排除口腔等部分，對齊正中線並保持左右對稱。用除錯輸出確認閾值範圍。 |
| [11:17](https://www.bilibili.com/video/BV1w7tm65EVk/?t=677) | 手修法線以保留幾何輪廓 | 表面小起伏會讓硬分界出現碎暗塊。修改著色用的法線能改變亮暗分界，同時保留實際網格、輪廓和透視。相比臉部 SDF，這也可用在身體或髮束。 |
| [13:22](https://www.bilibili.com/video/BV1w7tm65EVk/?t=802) | 骨架變形後再轉移法線 | 靜態臉看起來理想，播動畫後仍可能跑出不希望的暗面。影片用同拓撲的副本當來源，在骨架修改器之後轉移自訂法線，再檢查大表情和轉頭結果。 |
| [14:18](https://www.bilibili.com/video/BV1w7tm65EVk/?t=858) | 用平面、球形或全身代理網格整理法線 | 平面最快，但臉可能整片一起亮或一起暗。球形代理讓臉部較自然地掃過明暗交界，簡化的全身代理則能統整衣褶和肌肉造成的碎陰影。以頂點組限定轉移部位。 |
| [19:26](https://www.bilibili.com/video/BV1w7tm65EVk/?t=1166) | 用 Grease Pencil 在固定鏡頭補畫 | 有些陰面只需在特定構圖裡修正。把 Grease Pencil 放在場景前、跟著攝影機，在攝影機視圖補畫。角色跨格或換鏡位時通常要重畫，較適合可控制鏡頭的動畫演出。 |
| [20:14](https://www.bilibili.com/video/BV1w7tm65EVk/?t=1214) | 分開輸出顏色與光影再合成 | 如果補畫會蓋掉原本的材質細節，可以把顏色和照明分成不同渲染層，在合成時增減陰影。這樣能保留底色並集中修正單鏡頭的明暗。 |

## 技術參考

[Blender Data Transfer 修改器](https://docs.blender.org/manual/en/latest/modeling/modifiers/modify/data_transfer.html) · [Blender Studio 的三渲二著色與合成案例](https://studio.blender.org/blog/shading-and-rendering-of-wing-it/)
