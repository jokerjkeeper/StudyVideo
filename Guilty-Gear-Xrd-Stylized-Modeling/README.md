# 《罪惡裝備》風格化模型與著色拆解

原始影片由 Noggi 上傳。時間碼對應以下 YouTube 版本。[Why Guilty Gear's 'Bad' 3D Models Look Perfectly 2D](https://www.youtube.com/watch?v=234m7y8D3cE)

Noggi 以檢視到的角色模型和 Blender 示範拆解效果。模型觀察與 Blender 重現提供可測試的做法，尤其適合對照畫面和資產長相為什麼不同。

本機字幕為英文自動字幕。

## 技術與時間碼

| 影片時間 | 技術 | 解決的問題與做法 |
| --- | --- | --- |
| [00:39](https://www.youtube.com/watch?v=234m7y8D3cE&t=39s) | 反向殼與臉部深度遮罩 | 反向殼會在臉頰和五官處冒出多餘線段。用頂點色標記臉部，只把這部分殼往遠離鏡頭的方向推，使多餘線被原模型遮住。 |
| [03:06](https://www.youtube.com/watch?v=234m7y8D3cE&t=186s) | 非球形眼睛與獨立虹膜 | 球體眼睛不容易吻合動漫眼裂。將眼型設計成由正面與外眼角方向組成的曲面，再獨立控制虹膜，可保留正面及斜角的眼型。 |
| [03:39](https://www.youtube.com/watch?v=234m7y8D3cE&t=219s) | 用斜角剪影設計臉部 | 顴骨的邊線決定四分之三側面有多飽滿。從常用鏡頭檢查顴骨、嘴角和輪廓。較深的 V 形嘴部讓遠側嘴角在斜角時能自然被嘴唇遮住。 |
| [04:10](https://www.youtube.com/watch?v=234m7y8D3cE&t=250s) | 為誇張表情配置拓撲 | 布線均勻的中性臉，在極大表情裡未必好用。片中模型把部分三角形拉長，換取更容易控制的表情變形和權重。 |
| [07:40](https://www.youtube.com/watch?v=234m7y8D3cE&t=460s) | 用內積控制卡通著色 | 用法線和光方向的內積得到受光程度，再用閾值變成色塊。這讓 Blender 示範可明確替換光向、閾值和遮罩，逐一觀察效果。 |
| [10:50](https://www.youtube.com/watch?v=234m7y8D3cE&t=650s) | 由視線方向控制邊緣光 | 比較表面法線與視線方向，找出接近剪影的區域。再用遮罩或閾值控制邊緣光出現的位置，減少無關亮邊。 |
| [13:20](https://www.youtube.com/watch?v=234m7y8D3cE&t=800s) | 貼圖通道打包 | 多份灰階資料可放入同一張圖的不同通道，再在著色器拆開。片中拆解的 ILM 貼圖用不同通道控制基礎閾值、高光範圍和內線等效果。 |
| [17:10](https://www.youtube.com/watch?v=234m7y8D3cE&t=1030s) | 高光遮罩和局部明暗控制 | 一般高光可能在皮衣上形成大片亮條。限制高光出現的區域，再用頂點資料調整局部入陰傾向，可以保留特意畫好的亮面與暗面。 |
| [18:40](https://www.youtube.com/watch?v=234m7y8D3cE&t=1120s) | 額外陰影網格 | 光照算出的暗面未必符合帽沿、裝飾或動作需要的暗影形狀。影片展示用半透明幾何壓暗底下區域，並以骨架驅動，讓陰影位置可隨姿勢修改。 |
| [20:04](https://www.youtube.com/watch?v=234m7y8D3cE&t=1204s) | 從代理形狀轉移法線 | 對髮束與臉部直接計算光照會讓光影太碎。用較簡單的平面或代理網格提供法線，再轉到細節模型，讓明暗塊跟著整理過的方向走。 |
| [24:10](https://www.youtube.com/watch?v=234m7y8D3cE&t=1450s) | 變形後維持自訂法線 | 模型做完骨架變形後，著色可能偏離靜態預覽。影片用 Blender 示範在 Armature 之後用 Data Transfer 補回法線，檢查大表情或極端姿勢下的亮暗是否仍合意。 |

## 技術參考

[Xrd 開發者的 GDC 簡報](https://media.gdcvault.com/gdc2015/presentations/Motomura_Junya_GuiltyGearXrd.pdf) 可用來交叉對照頂點色、法線與描邊。
