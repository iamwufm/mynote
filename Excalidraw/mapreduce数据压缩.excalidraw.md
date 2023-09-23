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
			"type": "rectangle",
			"version": 98,
			"versionNonce": 259976279,
			"isDeleted": false,
			"id": "CNpyVX7v9HRZRN4YP_dnL",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -14.84906344943576,
			"y": -116.02974022759332,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"width": 122,
			"height": 31,
			"seed": 1674043185,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "QGV4GG4d"
				},
				{
					"id": "zwCFMuC3z99Djo0hoPzYE",
					"type": "arrow"
				}
			],
			"updated": 1695371502139,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 90,
			"versionNonce": 1719888695,
			"isDeleted": false,
			"id": "QGV4GG4d",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -9.84906344943576,
			"y": -109.72974022759333,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"width": 112,
			"height": 18.4,
			"seed": 253378225,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695371501873,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "输入端采用压缩",
			"rawText": "输入端采用压缩",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "CNpyVX7v9HRZRN4YP_dnL",
			"originalText": "输入端采用压缩",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 447,
			"versionNonce": 1088207351,
			"isDeleted": false,
			"id": "xoQakhYq",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -98.00792778862854,
			"y": -76.41075897216797,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"width": 305.2792968382602,
			"height": 45.158070882814144,
			"seed": 610981567,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695371510130,
			"link": null,
			"locked": false,
			"fontSize": 13.089295908062072,
			"fontFamily": 2,
			"text": "无须显示指定使用的编解码方式。\nhadoop自动检查文件拓展名，如果拓展名能够匹配，\n就会用恰当的编解码方式对文件进行压缩和解压",
			"rawText": "无须显示指定使用的编解码方式。\nhadoop自动检查文件拓展名，如果拓展名能够匹配，\n就会用恰当的编解码方式对文件进行压缩和解压",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "无须显示指定使用的编解码方式。\nhadoop自动检查文件拓展名，如果拓展名能够匹配，\n就会用恰当的编解码方式对文件进行压缩和解压",
			"lineHeight": 1.15,
			"baseline": 42
		},
		{
			"type": "text",
			"version": 903,
			"versionNonce": 361133337,
			"isDeleted": false,
			"id": "PrBGWvKn",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -96.4017333984375,
			"y": -19.73585425515493,
			"strokeColor": "#e03131",
			"backgroundColor": "#69db7c",
			"width": 292.4605752719124,
			"height": 58.51752161183667,
			"seed": 1285874321,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695371514120,
			"link": null,
			"locked": false,
			"fontSize": 12.721200350399279,
			"fontFamily": 2,
			"text": "企业开发：\n1.数据量小于块大小（数据量200m，块大小128），\n    重点考虑压缩和解压速度比较快的LZO/Snappy\n2.数据量非常大，重点考虑支持切片的Bzip2/LZO",
			"rawText": "企业开发：\n1.数据量小于块大小（数据量200m，块大小128），\n    重点考虑压缩和解压速度比较快的LZO/Snappy\n2.数据量非常大，重点考虑支持切片的Bzip2/LZO",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "企业开发：\n1.数据量小于块大小（数据量200m，块大小128），\n    重点考虑压缩和解压速度比较快的LZO/Snappy\n2.数据量非常大，重点考虑支持切片的Bzip2/LZO",
			"lineHeight": 1.15,
			"baseline": 54.99999999999999
		},
		{
			"type": "rectangle",
			"version": 207,
			"versionNonce": 322535447,
			"isDeleted": false,
			"id": "yN6TskUipaTnfh7-piBc_",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 231.13494873046875,
			"y": -116.1963695949978,
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
			"updated": 1695371515833,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 251,
			"versionNonce": 1999597785,
			"isDeleted": false,
			"id": "WN39JtxV",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 238.00994873046875,
			"y": -110.89636959499781,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"width": 150.25,
			"height": 18.4,
			"seed": 1365811505,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695371515833,
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
			"version": 1132,
			"versionNonce": 1873474071,
			"isDeleted": false,
			"id": "U38iKQUq",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 230.7084503173828,
			"y": -66.87584234210541,
			"strokeColor": "#e03131",
			"backgroundColor": "#69db7c",
			"width": 257.955841344372,
			"height": 67.40641050072558,
			"seed": 425628977,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695371518874,
			"link": null,
			"locked": false,
			"fontSize": 14.653567500157736,
			"fontFamily": 2,
			"text": "企业开发中如何选择：为了减少\nmapTask和ReduceTask之间的网络IO\n\n重点考虑压缩和解压缩快的LZO/Snappy",
			"rawText": "企业开发中如何选择：为了减少\nmapTask和ReduceTask之间的网络IO\n\n重点考虑压缩和解压缩快的LZO/Snappy",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "企业开发中如何选择：为了减少\nmapTask和ReduceTask之间的网络IO\n\n重点考虑压缩和解压缩快的LZO/Snappy",
			"lineHeight": 1.15,
			"baseline": 64
		},
		{
			"type": "rectangle",
			"version": 201,
			"versionNonce": 940275767,
			"isDeleted": false,
			"id": "2FEvCrz-C8PVVm4r5xTaI",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 529.8172607421877,
			"y": -111.84719763861762,
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
				},
				{
					"id": "6OVvWxEL5zh79Ra0QyvON",
					"type": "arrow"
				}
			],
			"updated": 1695371533431,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 257,
			"versionNonce": 984648377,
			"isDeleted": false,
			"id": "jWzQzWz7",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 536.2430419921877,
			"y": -106.54719763861762,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#69db7c",
			"width": 151.1484375,
			"height": 18.4,
			"seed": 1099066655,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695371533431,
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
			"version": 1363,
			"versionNonce": 1863313559,
			"isDeleted": false,
			"id": "oS7ZDQEs",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 525.963252597385,
			"y": -67.95797136127467,
			"strokeColor": "#e03131",
			"backgroundColor": "#69db7c",
			"width": 277.1848921777912,
			"height": 73.78187956742173,
			"seed": 1162653375,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695371538120,
			"link": null,
			"locked": false,
			"fontSize": 12.831631229116818,
			"fontFamily": 2,
			"text": "看需求：\n        如果数据永久保存，考虑压缩率比较高的\nBzip2和Gzip\n        如果作为下一个MapReduce输入，需要考虑\n数据量和是否支持切片",
			"rawText": "看需求：\n        如果数据永久保存，考虑压缩率比较高的\nBzip2和Gzip\n        如果作为下一个MapReduce输入，需要考虑\n数据量和是否支持切片",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "看需求：\n        如果数据永久保存，考虑压缩率比较高的\nBzip2和Gzip\n        如果作为下一个MapReduce输入，需要考虑\n数据量和是否支持切片",
			"lineHeight": 1.15,
			"baseline": 70.00000000000003
		},
		{
			"type": "arrow",
			"version": 118,
			"versionNonce": 1176848313,
			"isDeleted": false,
			"id": "zwCFMuC3z99Djo0hoPzYE",
			"fillStyle": "solid",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -12.357269287109375,
			"y": -130.1250228881836,
			"strokeColor": "#1971c2",
			"backgroundColor": "#69db7c",
			"width": 136.18489460149036,
			"height": 0.006958605320136257,
			"seed": 824963615,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1695371502139,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "CNpyVX7v9HRZRN4YP_dnL",
				"focus": -1.908796358126622,
				"gap": 14.095282660590271
			},
			"endBinding": {
				"elementId": "fureEaLYKmtEX0YZtKtTX",
				"gap": 3.529308279369015,
				"focus": -0.019609798096490196
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
					136.18489460149036,
					0.006958605320136257
				]
			]
		},
		{
			"type": "rectangle",
			"version": 59,
			"versionNonce": 597898719,
			"isDeleted": false,
			"id": "fureEaLYKmtEX0YZtKtTX",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 127.35693359375,
			"y": -145.41072845458984,
			"strokeColor": "#1971c2",
			"backgroundColor": "#228be6",
			"width": 52,
			"height": 30,
			"seed": 750525521,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
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
			"type": "text",
			"version": 8,
			"versionNonce": 536184223,
			"isDeleted": false,
			"id": "DxRpmBjF",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 137.79443359375,
			"y": -139.61072845458983,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#228be6",
			"width": 31.125,
			"height": 18.4,
			"seed": 1992854815,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695184370812,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "Map",
			"rawText": "Map",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "fureEaLYKmtEX0YZtKtTX",
			"originalText": "Map",
			"lineHeight": 1.15,
			"baseline": 14
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
			"type": "arrow",
			"version": 123,
			"versionNonce": 2023478777,
			"isDeleted": false,
			"id": "LvZ_vov9gCsI5JwTBYXls",
			"fillStyle": "solid",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 181.3570556640625,
			"y": -128.98217010498047,
			"strokeColor": "#1971c2",
			"backgroundColor": "#228be6",
			"width": 272.57135009765625,
			"height": 0.000030517578125,
			"seed": 383260991,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1695371492888,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "fureEaLYKmtEX0YZtKtTX",
				"gap": 2.0001220703125,
				"focus": 0.09523699582865745
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
					272.57135009765625,
					0.000030517578125
				]
			]
		},
		{
			"type": "arrow",
			"version": 183,
			"versionNonce": 1168165785,
			"isDeleted": false,
			"id": "6OVvWxEL5zh79Ra0QyvON",
			"fillStyle": "solid",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 522.2141723632812,
			"y": -127.95841465677074,
			"strokeColor": "#1971c2",
			"backgroundColor": "#228be6",
			"width": 246.10268296681159,
			"height": 2.937508219072342,
			"seed": 154271473,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1695371533431,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "uGztJ7ZZX1L5nWswXq_Du",
				"gap": 2.71435546875,
				"focus": 0.05449817811429783
			},
			"endBinding": {
				"elementId": "2FEvCrz-C8PVVm4r5xTaI",
				"focus": 1.908531641315916,
				"gap": 13.960347493489593
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
					246.10268296681159,
					2.937508219072342
				]
			]
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
		"scrollX": 122.44417783949103,
		"scrollY": 241.1120769356726,
		"zoom": {
			"value": 0.9
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