# Unity Toon Shader｜官方卡通材質參數

原始程式庫｜[Unity Toon Shader](https://github.com/Unity-Technologies/com.unity.toonshader)

UTS3 提供賽璐璐式的三段配色與獨立的高光、邊緣光和描邊。官方列出 Built-in、URP 和 HDRP 的支援情形，各版本與管線可用功能有差異。[Unity 官方手冊](https://docs.unity3d.com/Packages/com.unity.toonshader@latest/) · [官方材質設定](https://docs.unity3d.com/ja/Packages/com.unity.toonshader@0.9/manual/Parameter-Settings.html) · [官方管線功能差異](https://docs.unity3d.com/ja/Packages/com.unity.toonshader@0.8/manual/FeatureModel_en.html)

| 技術或功能 | 解決的問題 | 做法與取捨 |
| --- | --- | --- |
| Base／1st Shading／2nd Shading | 單一底色乘陰影無法精準呈現動畫色指定。 | 分別指定亮部、淺影和深影的顏色或貼圖，讓同一材質可維持設計好的三段色相。 |
| Shading Step 與 Feather | 明暗切換位置和過渡寬度如果綁在一起，調一個參數會同時改壞兩者。 | 用 Step 定色階界線、Feather 調過渡帶寬，分別控制色塊面積與硬軟。 |
| Shadow Control Maps | 鼻翼、衣領等指定位置有時需要保留固定暗形，單靠即時燈光會跑掉。 | 以線性控制貼圖指定第一、第二影色的落點，和即時明暗一同決定分布。 |
| Scene Light Effectiveness | 場景燈色變化可能把角色原定陰影和高光配色一起染掉。 | 分項設定場景燈對底色、影色和其他效果的影響，保留需要鎖定的角色配色。 |
| Highlight 與 Rim Light | 主要亮點和剪影亮邊負責不同的形體資訊。 | 獨立調高光與邊緣光的範圍、色彩與適用區域，在維持臉和衣裝明暗分界的同時增加亮點。 |
| MatCap 與 Angel Ring | 髮束和裝飾需要可預期的反射外觀或環狀亮帶。 | MatCap 以視角對應的貼圖表現表面反光，Angel Ring 可配置風格化髮光。進階控制項依材質模式與版本而異。 |
| Outline Width Map 與鏡頭距離 | 手、頭髮和衣裝一起用固定線寬，在特寫或遠景容易失去比例。 | 用貼圖配置局部線寬，搭配鏡頭距離與描邊色設定控制輪廓。 |
| Emission | 顯示器、符號或飾件需要保有自身亮度。 | 使用自發光貼圖及相關控制為指定區域加亮，並保留獨立調色的自由。 |
