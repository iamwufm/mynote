---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
需求：
实时地将/root/log/目录中新出现地文件采集到HDFS中去
 ^1kNiwmii

/root/log
 不断有新文件进来 ^hMYhkAeJ

Flume agent ^QxSewsup

source：读目录的实现类
channel：内存/本地文件缓存
sink：写数据到HDFS的实现类 ^9vPziJKA

HDFS ^fZduthgf

/flume/collect/ ^gXGF9Ake

配置文件 ^HmKyHe9S

/root/log
 不断有新文件进来 ^GE5SZ0ei

Flume agent ^88eIRfsA

配置文件 ^fnjPSlnR

source：读目录的实现类
channel：内存/本地文件缓存
sink：写数据到HDFS的实现类 ^g78DaI1I

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.9.14",
	"elements": [
		{
			"id": "1kNiwmii",
			"type": "text",
			"x": -183.92852783203125,
			"y": -250.6964340209961,
			"width": 492.265625,
			"height": 69,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 2041769103,
			"version": 171,
			"versionNonce": 698430049,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697424111845,
			"link": null,
			"locked": false,
			"text": "需求：\n实时地将/root/log/目录中新出现地文件采集到HDFS中去\n",
			"rawText": "需求：\n实时地将/root/log/目录中新出现地文件采集到HDFS中去\n",
			"fontSize": 20,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 64,
			"containerId": null,
			"originalText": "需求：\n实时地将/root/log/目录中新出现地文件采集到HDFS中去\n",
			"lineHeight": 1.15
		},
		{
			"id": "p2zNveXtfaiah1SaThRPh",
			"type": "rectangle",
			"x": -153.0714111328125,
			"y": -171.26790618896484,
			"width": 137.71429443359375,
			"height": 140.00006103515625,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1368322799,
			"version": 122,
			"versionNonce": 30997985,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "GPqmeEJhL2lX-WExyFXtw",
					"type": "arrow"
				}
			],
			"updated": 1697424377635,
			"link": null,
			"locked": false
		},
		{
			"id": "hMYhkAeJ",
			"type": "text",
			"x": -149.64288330078125,
			"y": -123.2393783569336,
			"width": 132.4453125,
			"height": 36.8,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 271629583,
			"version": 141,
			"versionNonce": 282755329,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697424162861,
			"link": null,
			"locked": false,
			"text": "/root/log\n 不断有新文件进来",
			"rawText": "/root/log\n 不断有新文件进来",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 33,
			"containerId": null,
			"originalText": "/root/log\n 不断有新文件进来",
			"lineHeight": 1.15
		},
		{
			"id": "2Ij387Rj2-Kdqu56Q0YKP",
			"type": "rectangle",
			"x": -92.21432495117188,
			"y": -11.839225769042969,
			"width": 102,
			"height": 47,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 2126821601,
			"version": 68,
			"versionNonce": 1218484673,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "QxSewsup"
				},
				{
					"id": "GPqmeEJhL2lX-WExyFXtw",
					"type": "arrow"
				}
			],
			"updated": 1697424377635,
			"link": null,
			"locked": false
		},
		{
			"id": "QxSewsup",
			"type": "text",
			"x": -85.68307495117188,
			"y": 2.460774230957032,
			"width": 88.9375,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 775193985,
			"version": 55,
			"versionNonce": 44015311,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697424377635,
			"link": null,
			"locked": false,
			"text": "Flume agent",
			"rawText": "Flume agent",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 14,
			"containerId": "2Ij387Rj2-Kdqu56Q0YKP",
			"originalText": "Flume agent",
			"lineHeight": 1.15
		},
		{
			"id": "Kupn6W_cvzOEByOso6xEd",
			"type": "rectangle",
			"x": -227.92864990234375,
			"y": 59.017860412597656,
			"width": 200,
			"height": 81.14288330078125,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1112677199,
			"version": 68,
			"versionNonce": 77513601,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "a-yNqdk3iBOZjDd8Bx2cz",
					"type": "arrow"
				}
			],
			"updated": 1697424338172,
			"link": null,
			"locked": false
		},
		{
			"id": "9vPziJKA",
			"type": "text",
			"x": -219.21435546875,
			"y": 78.44635772705078,
			"width": 182.256591796875,
			"height": 46.5844363720207,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 959237039,
			"version": 203,
			"versionNonce": 905474017,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697424261380,
			"link": null,
			"locked": false,
			"text": "source：读目录的实现类\nchannel：内存/本地文件缓存\nsink：写数据到HDFS的实现类",
			"rawText": "source：读目录的实现类\nchannel：内存/本地文件缓存\nsink：写数据到HDFS的实现类",
			"fontSize": 13.502735180295854,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 43,
			"containerId": null,
			"originalText": "source：读目录的实现类\nchannel：内存/本地文件缓存\nsink：写数据到HDFS的实现类",
			"lineHeight": 1.15
		},
		{
			"id": "7jIfs1gHUzgsR4OVx2Ure",
			"type": "rectangle",
			"x": -169.0714111328125,
			"y": 180.1607437133789,
			"width": 462.85711669921875,
			"height": 103.42864990234375,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1812057025,
			"version": 77,
			"versionNonce": 1420309679,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "e6a_9QzyzUfgqdHEr7Ep8",
					"type": "arrow"
				}
			],
			"updated": 1697424437597,
			"link": null,
			"locked": false
		},
		{
			"id": "fZduthgf",
			"type": "text",
			"x": 220.071533203125,
			"y": 155.01786041259766,
			"width": 43.5546875,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 361867713,
			"version": 13,
			"versionNonce": 769762561,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697424305406,
			"link": null,
			"locked": false,
			"text": "HDFS",
			"rawText": "HDFS",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 14,
			"containerId": null,
			"originalText": "HDFS",
			"lineHeight": 1.15
		},
		{
			"id": "gXGF9Ake",
			"type": "text",
			"x": 17.78570556640625,
			"y": 219.3035659790039,
			"width": 97.8125,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1225924015,
			"version": 18,
			"versionNonce": 883263425,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697424318871,
			"link": null,
			"locked": false,
			"text": "/flume/collect/",
			"rawText": "/flume/collect/",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 14,
			"containerId": null,
			"originalText": "/flume/collect/",
			"lineHeight": 1.15
		},
		{
			"id": "GPqmeEJhL2lX-WExyFXtw",
			"type": "arrow",
			"x": 0.64276123046875,
			"y": -11.267845153808594,
			"width": 25.14276123046875,
			"height": 30.857147216796875,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 2015924431,
			"version": 41,
			"versionNonce": 1750300431,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697424377717,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-25.14276123046875,
					-30.857147216796875
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "p2zNveXtfaiah1SaThRPh",
				"focus": 0.09153699592017615,
				"gap": 20
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "HQw0EfuU7j_dD02vwxmyU",
			"type": "arrow",
			"x": 8.0714111328125,
			"y": 24.732154846191406,
			"width": 28.57135009765625,
			"height": 156.00009155273438,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 634311009,
			"version": 100,
			"versionNonce": 1374472399,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697424443034,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					28.57135009765625,
					71.42855834960938
				],
				[
					27.4285888671875,
					156.00009155273438
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "a-yNqdk3iBOZjDd8Bx2cz",
			"type": "arrow",
			"x": -136.49993896484375,
			"y": 55.589332580566406,
			"width": 44.57135009765625,
			"height": 27.428558349609375,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 545124239,
			"version": 25,
			"versionNonce": 302941537,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697424377717,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					44.57135009765625,
					-27.428558349609375
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "HmKyHe9S",
				"focus": 1.1162369627351942,
				"gap": 2.857177734375
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "HmKyHe9S",
			"type": "text",
			"x": -203.35711669921875,
			"y": 35.58924102783203,
			"width": 64,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 635218785,
			"version": 61,
			"versionNonce": 1074773295,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "a-yNqdk3iBOZjDd8Bx2cz",
					"type": "arrow"
				}
			],
			"updated": 1697424377717,
			"link": null,
			"locked": false,
			"text": "配置文件",
			"rawText": "配置文件",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 14,
			"containerId": null,
			"originalText": "配置文件",
			"lineHeight": 1.15
		},
		{
			"type": "rectangle",
			"version": 161,
			"versionNonce": 344440559,
			"isDeleted": false,
			"id": "rSgppzP6Zubz35EoarECM",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 123.68760880710698,
			"y": -174.83936309814453,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 137.71429443359375,
			"height": 140.00006103515625,
			"seed": 1493090383,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "5PLuKraRfnNqX7KBL-cU1",
					"type": "arrow"
				}
			],
			"updated": 1697424393317,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 180,
			"versionNonce": 305137935,
			"isDeleted": false,
			"id": "GE5SZ0ei",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 127.11613663913823,
			"y": -126.81083526611326,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 132.4453125,
			"height": 36.8,
			"seed": 654302831,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697424393317,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "/root/log\n 不断有新文件进来",
			"rawText": "/root/log\n 不断有新文件进来",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "/root/log\n 不断有新文件进来",
			"lineHeight": 1.15,
			"baseline": 33
		},
		{
			"type": "rectangle",
			"version": 108,
			"versionNonce": 350816929,
			"isDeleted": false,
			"id": "R9SIPrADX7HedWdRVQ0zi",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 184.5446949887476,
			"y": -15.410682678222656,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 102,
			"height": 47,
			"seed": 1870149775,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "88eIRfsA"
				},
				{
					"id": "5PLuKraRfnNqX7KBL-cU1",
					"type": "arrow"
				},
				{
					"id": "TxTacpOw6-fynhmXXBtEC",
					"type": "arrow"
				}
			],
			"updated": 1697424404500,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 95,
			"versionNonce": 1686769455,
			"isDeleted": false,
			"id": "88eIRfsA",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 191.0759449887476,
			"y": -1.1106826782226555,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 88.9375,
			"height": 18.4,
			"seed": 1478512303,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697424393317,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "Flume agent",
			"rawText": "Flume agent",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "R9SIPrADX7HedWdRVQ0zi",
			"originalText": "Flume agent",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "arrow",
			"version": 118,
			"versionNonce": 482701089,
			"isDeleted": false,
			"id": "5PLuKraRfnNqX7KBL-cU1",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 277.40178117038823,
			"y": -14.839302062988281,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 25.14276123046875,
			"height": 30.857147216796875,
			"seed": 1000382671,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1697424394141,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "rSgppzP6Zubz35EoarECM",
				"focus": 0.09153699592017615,
				"gap": 20
			},
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					-25.14276123046875,
					-30.857147216796875
				]
			]
		},
		{
			"type": "arrow",
			"version": 154,
			"versionNonce": 1353052609,
			"isDeleted": false,
			"id": "e6a_9QzyzUfgqdHEr7Ep8",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 284.830431072732,
			"y": 21.16069793701172,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 59.94403069891297,
			"height": 157.71438598632812,
			"seed": 194096879,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1697424437597,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "7jIfs1gHUzgsR4OVx2Ure",
				"focus": 0.694164087689142,
				"gap": 1.2856597900390625
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					57.14288330078125,
					67.42855834960938
				],
				[
					-2.801147398131718,
					157.71438598632812
				]
			]
		},
		{
			"type": "rectangle",
			"version": 115,
			"versionNonce": 2051086081,
			"isDeleted": false,
			"id": "Aw993bdYz3bzTDf8eaOuR",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 74.6429443359375,
			"y": 65.1606674194336,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 200,
			"height": 81.14288330078125,
			"seed": 148556303,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "TxTacpOw6-fynhmXXBtEC",
					"type": "arrow"
				}
			],
			"updated": 1697424402668,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 136,
			"versionNonce": 551819535,
			"isDeleted": false,
			"id": "TxTacpOw6-fynhmXXBtEC",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 166.93406034658872,
			"y": 61.20142809183176,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 28.280356157317527,
			"height": 29.18355242044504,
			"seed": 439660591,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1697424413341,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "fnjPSlnR",
				"focus": 1.1162369627351945,
				"gap": 3.7195828075262227
			},
			"endBinding": {
				"elementId": "R9SIPrADX7HedWdRVQ0zi",
				"focus": 0.23236617199391352,
				"gap": 1
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					28.280356157317527,
					-29.18355242044504
				]
			]
		},
		{
			"type": "text",
			"version": 108,
			"versionNonce": 1793625825,
			"isDeleted": false,
			"id": "fnjPSlnR",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 99.2144775390625,
			"y": 41.73204803466797,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 64,
			"height": 18.4,
			"seed": 1336726095,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "TxTacpOw6-fynhmXXBtEC",
					"type": "arrow"
				}
			],
			"updated": 1697424402668,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "配置文件",
			"rawText": "配置文件",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "配置文件",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 231,
			"versionNonce": 551775361,
			"isDeleted": false,
			"id": "g78DaI1I",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 85.5146484375,
			"y": 82.01134779299355,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 182.256591796875,
			"height": 46.5844363720207,
			"seed": 38639201,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697424409652,
			"link": null,
			"locked": false,
			"fontSize": 13.502735180295854,
			"fontFamily": 2,
			"text": "source：读目录的实现类\nchannel：内存/本地文件缓存\nsink：写数据到HDFS的实现类",
			"rawText": "source：读目录的实现类\nchannel：内存/本地文件缓存\nsink：写数据到HDFS的实现类",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "source：读目录的实现类\nchannel：内存/本地文件缓存\nsink：写数据到HDFS的实现类",
			"lineHeight": 1.15,
			"baseline": 43
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#f08c00",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 0.5,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 2,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 418.71429443359375,
		"scrollY": 348.7321472167969,
		"zoom": {
			"value": 1
		},
		"currentItemRoundness": "round",
		"gridSize": null,
		"currentStrokeOptions": null,
		"previousGridSize": null,
		"frameRendering": {
			"enabled": true,
			"clip": true,
			"name": true,
			"outline": true
		}
	},
	"files": {}
}
```
%%