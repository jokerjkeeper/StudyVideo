# Blender 三渲二起始工具包｜Noggi

原始工具頁｜[Noggi's 3D Anime Starter Kit](https://noggi3d.gumroad.com/l/starter-kit)

這套 Blender 資產預先搭好角色著色、臉部陰影與兩種輪廓線工作流。作者的商品頁列出以下功能。

| 技術或工具 | 解決的問題 | 做法與取捨 |
| --- | --- | --- |
| 角色 Cel Shader 與可重用節點組 | 每個新角色都要重建色階與陰影材質，原型很難快速比較。 | 從 Asset Browser 套用現成材質，再用可重用的 Shader Groups 混搭出需要的明暗與材質風格。 |
| 臉部 SDF 著色 | 普通模型法線可能讓鼻翼、眼眶出現零碎暗塊。 | 用臉部 SDF 著色器控制明暗分界的形狀，再按角色微調，讓陰影沿美術指定的輪廓變化。 |
| Inverted Hull 外描邊 | 角色轉動或鏡頭切換時，需要一條跟模型的外輪廓一起動的線。 | 使用外擴背殼的輪廓系統，可調線條樣式。另有引導式網格準備工具，供需要把描邊做進遊戲資產的流程使用。 |
| Grease Pencil Line Art | 只畫背殼外線時，部分內部結構和手繪筆觸難控制。 | 從場景幾何產生 Grease Pencil 筆觸，再用附帶工具與 Geometry Nodes 修飾線條。 |
| Light Rotation Catcher | 著色需要跟燈光方向一致，逐資產配置 Driver 會重複工作。 | 用 Geometry Nodes 複製光源旋轉，把取用方向的設定做成可重用節點。 |
| Shape Key 轉移 | 不同資產需要共用表情或修形時，手動重做每個形狀成本高。 | 以工具搬移 Shape Key，供沿用形變的資產流程使用。 |
