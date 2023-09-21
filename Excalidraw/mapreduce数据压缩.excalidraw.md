---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
输入端采用压缩 ^QGV4GG4d

无须显示指定使用的编解码方式。
hadoop自动检查文件拓展名，如果拓展名能够匹配，
就会用恰当的编解码方式对文件进行压缩和解压 ^xoQakhYq

企业开发：
1.数据量小于块大小（数据量200m，块大小128），
    重点考虑压缩和解压速度比较快的LZO/Snappy
2.数据量非常大，重点考虑支持切片的Bzip2/LZO ^PrBGWvKn

Mapper输出采用压缩 ^WN39JtxV

企业开发中如何选择：为了减少
mapTask和ReduceTask之间的网络IO

重点考虑压缩和解压缩快的LZO/Snappy ^U38iKQUq

Reduce输出采用压缩 ^jWzQzWz7

看需求：
        如果数据永久保存，考虑压缩率比较高的
Bzip2和Gzip
        如果作为下一个MapReduce输入，需要考虑
数据量和是否支持切片 ^oS7ZDQEs

Map ^DxRpmBjF

Reduce ^htmtLPAS

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.9.14",
	"elements": [
		{
			"id": "CNpyVX7v9HRZRN4YP_dnL",
			"type": "rectangle",
			"x": -14.214263916015625,
			"y": -91.2678451538086,
			"width": 122,
			"height": 31,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1674043185,
			"version": 66,
			"versionNonce": 954925297,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "QGV4GG4d"
				}
			],
			"updated": 1695181813910,
			"link": null,
			"locked": false
		},
		{
			"id": "QGV4GG4d",
			"type": "text",
			"x": -9.214263916015625,
			"y": -84.9678451538086,
			"width": 112,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 253378225,
			"version": 59,
			"versionNonce": 104659967,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695183990023,
			"link": null,
			"locked": false,
			"text": "输入端采用压缩",
			"rawText": "输入端采用压缩",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 14,
			"containerId": "CNpyVX7v9HRZRN4YP_dnL",
			"originalText": "输入端采用压缩",
			"lineHeight": 1.15
		},
		{
			"id": "xoQakhYq",
			"type": "text",
			"x": -58.64276123046875,
			"y": -47.83928680419922,
			"width": 270.9412536621094,
			"height": 40.07865736284887,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 610981567,
			"version": 378,
			"versionNonce": 357361905,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695182068981,
			"link": null,
			"locked": false,
			"text": "无须显示指定使用的编解码方式。\nhadoop自动检查文件拓展名，如果拓展名能够匹配，\n就会用恰当的编解码方式对文件进行压缩和解压",
			"rawText": "无须显示指定使用的编解码方式。\nhadoop自动检查文件拓展名，如果拓展名能够匹配，\n就会用恰当的编解码方式对文件进行压缩和解压",
			"fontSize": 11.617002134159096,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 37,
			"containerId": null,
			"originalText": "无须显示指定使用的编解码方式。\nhadoop自动检查文件拓展名，如果拓展名能够匹配，\n就会用恰当的编解码方式对文件进行压缩和解压",
			"lineHeight": 1.15
		},
		{
			"type": "text",
			"version": 859,
			"versionNonce": 2021354975,
			"isDeleted": false,
			"id": "PrBGWvKn",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -61.4810791015625,
			"y": 8.83561791281382,
			"strokeColor": "#e03131",
			"backgroundColor": "#69db7c",
			"width": 267.0750427246094,
			"height": 53.438209817131835,
			"seed": 1285874321,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695182359702,
			"link": null,
			"locked": false,
			"fontSize": 11.617002134159096,
			"fontFamily": 2,
			"text": "企业开发：\n1.数据量小于块大小（数据量200m，块大小128），\n    重点考虑压缩和解压速度比较快的LZO/Snappy\n2.数据量非常大，重点考虑支持切片的Bzip2/LZO",
			"rawText": "企业开发：\n1.数据量小于块大小（数据量200m，块大小128），\n    重点考虑压缩和解压速度比较快的LZO/Snappy\n2.数据量非常大，重点考虑支持切片的Bzip2/LZO",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "企业开发：\n1.数据量小于块大小（数据量200m，块大小128），\n    重点考虑压缩和解压速度比较快的LZO/Snappy\n2.数据量非常大，重点考虑支持切片的Bzip2/LZO",
			"lineHeight": 1.15,
			"baseline": 50
		},
		{
			"type": "rectangle",
			"version": 190,
			"versionNonce": 1844043199,
			"isDeleted": false,
			"id": "yN6TskUipaTnfh7-piBc_",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 236.21429443359375,
			"y": -99.05345916748047,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"width": 164,
			"height": 29,
			"seed": 1567175505,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "WN39JtxV"
				}
			],
			"updated": 1695184440139,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 234,
			"versionNonce": 612049329,
			"isDeleted": false,
			"id": "WN39JtxV",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 243.08929443359375,
			"y": -93.75345916748047,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"width": 150.25,
			"height": 18.4,
			"seed": 1365811505,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695184440139,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "Mapper输出采用压缩",
			"rawText": "Mapper输出采用压缩",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "yN6TskUipaTnfh7-piBc_",
			"originalText": "Mapper输出采用压缩",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 1104,
			"versionNonce": 473195153,
			"isDeleted": false,
			"id": "U38iKQUq",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 233.2481231689453,
			"y": -46.558391712765136,
			"strokeColor": "#e03131",
			"backgroundColor": "#69db7c",
			"width": 204.5012969970703,
			"height": 53.438209817131835,
			"seed": 425628977,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695184442029,
			"link": null,
			"locked": false,
			"fontSize": 11.617002134159096,
			"fontFamily": 2,
			"text": "企业开发中如何选择：为了减少\nmapTask和ReduceTask之间的网络IO\n\n重点考虑压缩和解压缩快的LZO/Snappy",
			"rawText": "企业开发中如何选择：为了减少\nmapTask和ReduceTask之间的网络IO\n\n重点考虑压缩和解压缩快的LZO/Snappy",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "企业开发中如何选择：为了减少\nmapTask和ReduceTask之间的网络IO\n\n重点考虑压缩和解压缩快的LZO/Snappy",
			"lineHeight": 1.15,
			"baseline": 50
		},
		{
			"type": "rectangle",
			"version": 163,
			"versionNonce": 1314042239,
			"isDeleted": false,
			"id": "2FEvCrz-C8PVVm4r5xTaI",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 532.35693359375,
			"y": -95.3392562866211,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"width": 164,
			"height": 29,
			"seed": 1121543423,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "jWzQzWz7"
				}
			],
			"updated": 1695184177357,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 220,
			"versionNonce": 2110993169,
			"isDeleted": false,
			"id": "jWzQzWz7",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 538.78271484375,
			"y": -90.0392562866211,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"width": 151.1484375,
			"height": 18.4,
			"seed": 1099066655,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695184187095,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "Reduce输出采用压缩",
			"rawText": "Reduce输出采用压缩",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "2FEvCrz-C8PVVm4r5xTaI",
			"originalText": "Reduce输出采用压缩",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 1319,
			"versionNonce": 50245055,
			"isDeleted": false,
			"id": "oS7ZDQEs",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 531.6776351928711,
			"y": -40.02146826882677,
			"strokeColor": "#e03131",
			"backgroundColor": "#69db7c",
			"width": 250.9468536376953,
			"height": 66.7977622714148,
			"seed": 1162653375,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695184317152,
			"link": null,
			"locked": false,
			"fontSize": 11.617002134159096,
			"fontFamily": 2,
			"text": "看需求：\n        如果数据永久保存，考虑压缩率比较高的\nBzip2和Gzip\n        如果作为下一个MapReduce输入，需要考虑\n数据量和是否支持切片",
			"rawText": "看需求：\n        如果数据永久保存，考虑压缩率比较高的\nBzip2和Gzip\n        如果作为下一个MapReduce输入，需要考虑\n数据量和是否支持切片",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "看需求：\n        如果数据永久保存，考虑压缩率比较高的\nBzip2和Gzip\n        如果作为下一个MapReduce输入，需要考虑\n数据量和是否支持切片",
			"lineHeight": 1.15,
			"baseline": 64
		},
		{
			"id": "zwCFMuC3z99Djo0hoPzYE",
			"type": "arrow",
			"x": -12.357269287109375,
			"y": -130.1250228881836,
			"width": 136.18489460149036,
			"height": 0.006958605320136257,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#69db7c",
			"fillStyle": "solid",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 824963615,
			"version": 116,
			"versionNonce": 1511162463,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695184376202,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					136.18489460149036,
					0.006958605320136257
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "fureEaLYKmtEX0YZtKtTX",
				"gap": 3.529308279369015,
				"focus": -0.019609798096490196
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "fureEaLYKmtEX0YZtKtTX",
			"type": "rectangle",
			"x": 127.35693359375,
			"y": -145.41072845458984,
			"width": 52,
			"height": 30,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#228be6",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 750525521,
			"version": 59,
			"versionNonce": 597898719,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "zwCFMuC3z99Djo0hoPzYE",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "DxRpmBjF"
				},
				{
					"id": "LvZ_vov9gCsI5JwTBYXls",
					"type": "arrow"
				}
			],
			"updated": 1695184399173,
			"link": null,
			"locked": false
		},
		{
			"id": "DxRpmBjF",
			"type": "text",
			"x": 137.79443359375,
			"y": -139.61072845458983,
			"width": 31.125,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#228be6",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1992854815,
			"version": 8,
			"versionNonce": 536184223,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695184370812,
			"link": null,
			"locked": false,
			"text": "Map",
			"rawText": "Map",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 14,
			"containerId": "fureEaLYKmtEX0YZtKtTX",
			"originalText": "Map",
			"lineHeight": 1.15
		},
		{
			"type": "rectangle",
			"version": 118,
			"versionNonce": 739247569,
			"isDeleted": false,
			"id": "uGztJ7ZZX1L5nWswXq_Du",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 452.49981689453125,
			"y": -144.1250228881836,
			"strokeColor": "#1971c2",
			"backgroundColor": "#228be6",
			"width": 67,
			"height": 30,
			"seed": 950156831,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "htmtLPAS"
				},
				{
					"id": "6OVvWxEL5zh79Ra0QyvON",
					"type": "arrow"
				}
			],
			"updated": 1695184438045,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 89,
			"versionNonce": 1632270271,
			"isDeleted": false,
			"id": "htmtLPAS",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 458.42559814453125,
			"y": -138.32502288818358,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#228be6",
			"width": 55.1484375,
			"height": 18.4,
			"seed": 1997742655,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695184430899,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "Reduce",
			"rawText": "Reduce",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "uGztJ7ZZX1L5nWswXq_Du",
			"originalText": "Reduce",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"id": "LvZ_vov9gCsI5JwTBYXls",
			"type": "arrow",
			"x": 181.3570556640625,
			"y": -128.98217010498047,
			"width": 272.57135009765625,
			"height": 0.000030517578125,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#228be6",
			"fillStyle": "solid",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 383260991,
			"version": 122,
			"versionNonce": 984624497,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695184434437,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					272.57135009765625,
					0.000030517578125
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "fureEaLYKmtEX0YZtKtTX",
				"focus": 0.09523699582865745,
				"gap": 2.0001220703125
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "6OVvWxEL5zh79Ra0QyvON",
			"type": "arrow",
			"x": 522.2141723632812,
			"y": -128.4194910780438,
			"width": 172.857177734375,
			"height": 0.5626790269366779,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#228be6",
			"fillStyle": "solid",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 154271473,
			"version": 159,
			"versionNonce": 2022777777,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695184438877,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					172.857177734375,
					-0.5626790269366779
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "uGztJ7ZZX1L5nWswXq_Du",
				"focus": 0.05449817811429783,
				"gap": 2.71435546875
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#1971c2",
		"currentItemBackgroundColor": "#228be6",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 4,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 2,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 136.857421875,
		"scrollY": 330.4464416503906,
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