- links
	- https://github.com/unlir/XDrive/blob/master/Hardware/XDrive_bom.xlsx
	-
- [[EDA]]
- #unstable
- 相關照片
  collapsed:: true
	- ![image.png](../assets/image_1774499231984_0.png)
	- ![image.png](../assets/image_1774499252244_0.png)
-
- type-c
  collapsed:: true
	- ![image.png](../assets/image_1774503666538_0.png)
	- ![image.png](../assets/image_1774504903112_0.png)
	- ![image.png](../assets/image_1774511233181_0.png)
	-
	- 特點：使用12pin type-c去除了快速TX/RX，只有VBUS,GND,USB2.0(DP/DM兩組可以提供正反無視)
	- USB_DM/USB_DP接下拉電阻22歐姆做阻抗匹配，然後連接到mcu
	- CC1,2和SBUS1,2懸空，前者提供判斷主從設備，後者提供更強傳輸能力，讓Typec可以做為hdmi使用，懸空後不做功能，因為沒有cc1,2的下拉電阻，所以只有typea to typec線能使用
	- VBUS配上濾波電容，使用常見高頻100nF以及50v大電壓因為插拔風險
	- 焊接固定腳與角落腳位為gnd直接連到gnd層
	- 電容電阻被放置在其背面
	- 主要使用1,4層，VBUS為第二層
	- SMD0805提供0.5A的保險絲電流保護
	- 優化：提供cc1,2下拉電阻(5.1k歐姆)這樣我的typec to typec 線才能用
- TB67 h橋馬達供電
  collapsed:: true
	- ![image.png](../assets/image_1774511748560_0.png)
		-
	- ![image.png](../assets/image_1774511763505_0.png)
	- ![image.png](../assets/image_1774514229348_0.png)
	-
	- 使用兩顆TB67H450fng，EL有散熱封裝，0,EL沒有
	- 因為MCU無法提供類比電壓作為VREFA需要的類比訊號，所以透過將數位訊號經由二階RC濾波平滑化成直流電壓
	- TB67H450fng的腳位須提供，IN1,IN2透過01,00,10,11的方式決定正轉反轉煞車自由四個可能(我沒照順序)，VREF為本次運轉的強度，會根據收到的電壓決定提給線圈的電流，VBB為馬達運轉的電壓來源，OUT+, OUT-直接和馬達相連提供一組線圈的電流，LSS微電流採樣端，和一個0.1歐姆電阻串聯接地，他會和VREF命令之電流進行比較，使得馬達線圈電流符合我們的要求
	- 其餘接地，焊盤也可以接地，並且在散熱部分打很多過孔提供更強散熱
	- OUT和馬達對接的部分採用大焊盤提供焊接功能，提供大量過孔用於增大面積
	- 主要使用1,3層，在大電流部分使用大量過孔增加面積，然後使用第三層獲取更大面積導線，然後訊號從第三層到mcu附近才切到第四層
	- 優化：改為TB67H450Afng為更新版本的，能適應低電流情景
- V3.3濾波並聯電容
  collapsed:: true
	- ![image.png](../assets/image_1774514584459_0.png)
	- ![image.png](../assets/image_1774515660888_0.png)
	- 雖然說這個並聯濾波說是要幫助減少雜訊但可以看到右側電容為了方便排列所以離mcu有距離，下方為提供給VCC3.3的焊盤使用，左側為就很近這樣
	- 使用標準100nF for 高頻雜訊
- VCC穩流電容
  collapsed:: true
	- ![image.png](../assets/image_1774516034171_0.png)
	- ![image.png](../assets/image_1774516854987_0.png)
	- 正反兩面一共八顆100uF並聯電容，然後主要使用第一層，由於使用大電流所以可以多放過孔，然後要靠近馬達驅動晶片
- 電流保護組件
  collapsed:: true
	- ![image.png](../assets/image_1774517696230_0.png)
	- ![image.png](../assets/image_1774517868265_0.png)
	- ![image.png](../assets/image_1774517885826_0.png)
	- SS54為蕭特基二極體，順向壓降極低所以適合大電流經過，不會產生過多廢熱，並且切換速度極快，比一般的二極體能更快反制反向電流的衝擊，最大順向電流5A，比馬達驅動最大的3.3A小，然後反向防護最大為40V，只要反接不要超過40v就沒問題，順向壓降約為0.55V
	- SMBJ28CA為突波吸收保護二極體TVS，雙向順態電壓抑制，尾號CA就是雙向保護，特性是反應速度快，出現異常電壓(馬達反電動勢)可以從高阻抗快速崩潰，變成低阻抗路徑把多餘電流接地，反向關斷電壓28V超過工作電壓，擊穿電壓為31v to 34v，所以當電壓不管從哪個方向只要超過這個數字就會導通崩潰，峰值脈衝為600W，換言之能吸收這麼多能量，盡量放在電流進入口來做為保護
	-
	-