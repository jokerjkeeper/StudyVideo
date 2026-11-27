# Blender 程序化動漫特效｜Noggi

原始工具頁｜[The Procedural 3D Anime VFX Toolkit](https://noggi3d.gumroad.com/l/anime_vfx)

Noggi 的 Blender 特效資產包。作者公開列出 16 種預設，並說明外觀和動畫都可在修改器面板調整。以下整理公開頁面列出的預設各自能解決的畫面問題，資產內部的節點細節以隨附手冊為準。[Gumroad 商品說明](https://noggi3d.gumroad.com/l/anime_vfx?layout=profile) · [Superhive 作者刊載版](https://superhivemarket.com/products/the-procedural-anime-vfx-toolkit)

| 技術或預設 | 解決的問題 | 用法與取捨 |
| --- | --- | --- |
| Geometry Nodes 與可動畫參數 | 每個鏡頭重建特效物件、模擬和關鍵幀，會拖慢試做。 | 把效果放入場景，在修改器調整外觀和動畫進度。可對進度以外的參數設關鍵幀，也可疊用多個效果。作者以 Eevee 為目標，Cycles 需要另行調整。 |
| Speed Lines 與 Wind Streak | 快速運動只靠位移不一定能讀出方向或速度。 | 速度線用成束線條強調動勢，風線用可辨識的流向暗示空氣運動。鏡頭上應留空間給主體的剪影。 |
| Directional Sparks 與 Directional Debris | 撞擊時要讓觀眾看出接觸點和力量朝哪裡傳遞。 | 用定向火花表現短促的亮點，用定向碎屑留下較持久的飛散形狀，可分別調整並疊在同一事件上。 |
| Dust and Smoke Trails | 主體已經移過，觀眾仍需要看見它的路徑和速度變化。 | 沿運動方向使用塵煙拖尾，把運動留下的痕跡與主體分層控制。 |
| Impact Crater | 撞擊瞬間很短，地面需要保留能讀出位置與規模的形變。 | 以撞擊坑提供留在接觸面的破壞輪廓，與飛散粒子、煙塵分開安排。 |
| Meteor Streak 與 Shockwave Ball | 飛行物需要長方向感，衝擊波則需要從撞擊點向外擴張。 | 用流星尾跡指向行進方向，以球狀衝擊波傳達向外傳播的體積。 |
