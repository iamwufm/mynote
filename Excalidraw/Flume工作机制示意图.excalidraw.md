---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
各种各样的数据源 ^PsRl4hrK

各种各样的目标存储系统 ^aGTOUpOx

Flume agent ^qMCsUQ8A

读数据 ^7DXe8nZN

缓存数据 ^3hTC72ZT

写数据 ^h5un5MIt

source
组件 接口 ^Gp0426xl

sink
组件 接口 ^vnoQJOb4

内存、本地磁盘文件 ^2rD7l9NQ

event：head(描述信息)，boby(数据) ^10qXh0Is

文件 ^3FZDqoPV

kafka消息 ^VWo2JlrA

hbase的row ^eIVeAVdy

mysql-record ^0S4SkHuF

读本地磁盘文件
的source实现类 ^7Vp0M6iy

读kafka数据的
的source实现类 ^Z8JVc1JB

写数据到HDFS的
sink实现类 ^Nj2qdbwQ

写数据到Hbase的
sink实现类 ^H0xSvgfu

HDFS ^npnZ0kGY

Hbase ^rCTQ8d62

Flume工作机制：
        
可以启动Flume的agent程序进行数据采集
每个agent程序中包含三大组件
(内部的数据流转：event)：
source
channel
sink

agent程序可以根据需要在多台机器上启动

 ^XEfMzDjj

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.9.14",
	"elements": [
		{
			"id": "PBLmwR7CRwSFFd4CaPWyk",
			"type": "rectangle",
			"x": -193.0714111328125,
			"y": -301.41070556640625,
			"width": 501,
			"height": 42,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#a5d8ff",
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
			"seed": 1568824097,
			"version": 202,
			"versionNonce": 1137956993,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "PsRl4hrK"
				}
			],
			"updated": 1697423445398,
			"link": null,
			"locked": false
		},
		{
			"id": "PsRl4hrK",
			"type": "text",
			"x": -6.5714111328125,
			"y": -289.61070556640624,
			"width": 128,
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
			"seed": 137152225,
			"version": 196,
			"versionNonce": 1985260559,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423445399,
			"link": null,
			"locked": false,
			"text": "各种各样的数据源",
			"rawText": "各种各样的数据源",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 14,
			"containerId": "PBLmwR7CRwSFFd4CaPWyk",
			"originalText": "各种各样的数据源",
			"lineHeight": 1.15
		},
		{
			"type": "rectangle",
			"version": 328,
			"versionNonce": 76284719,
			"isDeleted": false,
			"id": "yQJBt1NgFRWTXQiptSNg4",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -149.21429443359375,
			"y": 258.3035659790039,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#a5d8ff",
			"width": 442,
			"height": 47,
			"seed": 1354887137,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "aGTOUpOx"
				}
			],
			"updated": 1697423479292,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 363,
			"versionNonce": 1552698177,
			"isDeleted": false,
			"id": "aGTOUpOx",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -16.21429443359375,
			"y": 272.6035659790039,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 176,
			"height": 18.4,
			"seed": 1906002881,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697423479292,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "各种各样的目标存储系统",
			"rawText": "各种各样的目标存储系统",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "yQJBt1NgFRWTXQiptSNg4",
			"originalText": "各种各样的目标存储系统",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"id": "GuyOWmWKtyDU7GpLd96uu",
			"type": "rectangle",
			"x": -94.21417236328125,
			"y": -150.12496185302734,
			"width": 375.999755859375,
			"height": 277.71435546875,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
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
			"seed": 526040673,
			"version": 121,
			"versionNonce": 1761484705,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "JduBXMB2KiriJxqn9BAxM",
					"type": "arrow"
				},
				{
					"id": "MTr7ruAWCAYCazoxzX-dc",
					"type": "arrow"
				}
			],
			"updated": 1697423289829,
			"link": null,
			"locked": false
		},
		{
			"id": "qMCsUQ8A",
			"type": "text",
			"x": 179.500244140625,
			"y": -174.12493133544922,
			"width": 88.9375,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 988948993,
			"version": 80,
			"versionNonce": 1935616001,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423042567,
			"link": null,
			"locked": false,
			"text": "Flume agent",
			"rawText": "Flume agent",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 14,
			"containerId": null,
			"originalText": "Flume agent",
			"lineHeight": 1.15
		},
		{
			"id": "VEofW5CT4Uz7cn0i0bkdZ",
			"type": "rectangle",
			"x": -62.357177734375,
			"y": -122.41072845458984,
			"width": 163,
			"height": 54,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
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
			"seed": 838355631,
			"version": 126,
			"versionNonce": 1419642959,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "7DXe8nZN"
				}
			],
			"updated": 1697423333323,
			"link": null,
			"locked": false
		},
		{
			"id": "7DXe8nZN",
			"type": "text",
			"x": -4.857177734375,
			"y": -104.61072845458985,
			"width": 48,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1216360175,
			"version": 82,
			"versionNonce": 1382066209,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423333323,
			"link": null,
			"locked": false,
			"text": "读数据",
			"rawText": "读数据",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 14,
			"containerId": "VEofW5CT4Uz7cn0i0bkdZ",
			"originalText": "读数据",
			"lineHeight": 1.15
		},
		{
			"type": "rectangle",
			"version": 165,
			"versionNonce": 215884399,
			"isDeleted": false,
			"id": "jmZNCS_zHztbUncY0IiBt",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -60.50006103515625,
			"y": -37.41075897216797,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 160,
			"height": 56,
			"seed": 488383183,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "3hTC72ZT"
				}
			],
			"updated": 1697423010225,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 136,
			"versionNonce": 1365164033,
			"isDeleted": false,
			"id": "3hTC72ZT",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -12.50006103515625,
			"y": -18.610758972167968,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 64,
			"height": 18.4,
			"seed": 1984753903,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697423010226,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "缓存数据",
			"rawText": "缓存数据",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "jmZNCS_zHztbUncY0IiBt",
			"originalText": "缓存数据",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "rectangle",
			"version": 228,
			"versionNonce": 197642735,
			"isDeleted": false,
			"id": "74bj9U-_-opDNruuaDM3h",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -59.7857666015625,
			"y": 57.303565979003906,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 166,
			"height": 50,
			"seed": 1263289569,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "h5un5MIt"
				},
				{
					"id": "0erNgODmWfJPGCXLl4tol",
					"type": "arrow"
				}
			],
			"updated": 1697423458854,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 207,
			"versionNonce": 1563706913,
			"isDeleted": false,
			"id": "h5un5MIt",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -0.7857666015625,
			"y": 73.1035659790039,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 48,
			"height": 18.4,
			"seed": 840106177,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697423013044,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "写数据",
			"rawText": "写数据",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "74bj9U-_-opDNruuaDM3h",
			"originalText": "写数据",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"id": "Gp0426xl",
			"type": "text",
			"x": 46.92877197265625,
			"y": -98.69634246826172,
			"width": 51.29115295410156,
			"height": 27.583001369706643,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 932697231,
			"version": 154,
			"versionNonce": 1067757167,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423022433,
			"link": null,
			"locked": false,
			"text": "source\n组件 接口",
			"rawText": "source\n组件 接口",
			"fontSize": 11.992609291176802,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 25,
			"containerId": null,
			"originalText": "source\n组件 接口",
			"lineHeight": 1.15
		},
		{
			"type": "text",
			"version": 204,
			"versionNonce": 447503041,
			"isDeleted": false,
			"id": "vnoQJOb4",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 52.13446044921875,
			"y": 79.51206529415059,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 51.29115295410156,
			"height": 27.583001369706643,
			"seed": 741782703,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697423019271,
			"link": null,
			"locked": false,
			"fontSize": 11.992609291176802,
			"fontFamily": 2,
			"text": "sink\n组件 接口",
			"rawText": "sink\n组件 接口",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "sink\n组件 接口",
			"lineHeight": 1.15,
			"baseline": 25
		},
		{
			"type": "text",
			"version": 210,
			"versionNonce": 2129267855,
			"isDeleted": false,
			"id": "2rD7l9NQ",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 71.42583465576172,
			"y": -15.059284803505669,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 107.909912109375,
			"height": 13.791500684853322,
			"seed": 1548231055,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697423028783,
			"link": null,
			"locked": false,
			"fontSize": 11.992609291176802,
			"fontFamily": 2,
			"text": "内存、本地磁盘文件",
			"rawText": "内存、本地磁盘文件",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "内存、本地磁盘文件",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"id": "A4BEsM3eN-7fjp3rghevK",
			"type": "rectangle",
			"x": 62.357177734375,
			"y": -18.69634246826172,
			"width": 122.85723876953125,
			"height": 22.2857666015625,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
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
			"seed": 2106157153,
			"version": 73,
			"versionNonce": 1516456719,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423025886,
			"link": null,
			"locked": false
		},
		{
			"id": "10qXh0Is",
			"type": "text",
			"x": 93.78570556640625,
			"y": -38.12494659423828,
			"width": 176.1062469482422,
			"height": 12.53008414981037,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 663145583,
			"version": 222,
			"versionNonce": 952427695,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423034502,
			"link": null,
			"locked": false,
			"text": "event：head(描述信息)，boby(数据)",
			"rawText": "event：head(描述信息)，boby(数据)",
			"fontSize": 10.895725347661193,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 10,
			"containerId": null,
			"originalText": "event：head(描述信息)，boby(数据)",
			"lineHeight": 1.15
		},
		{
			"id": "JduBXMB2KiriJxqn9BAxM",
			"type": "arrow",
			"x": -76.80245843945062,
			"y": -199.83928680419922,
			"width": 29.013093665170828,
			"height": 30.825170766492334,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1401427087,
			"version": 413,
			"versionNonce": 314230881,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423339992,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-29.013093665170828,
					-30.825170766492334
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "7Vp0M6iy",
				"focus": 0.2989567874863018,
				"gap": 2.857269287109361
			},
			"endBinding": {
				"elementId": "3FZDqoPV",
				"focus": 0.25687125366996477,
				"gap": 2.203326547960799
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "3FZDqoPV",
			"type": "text",
			"x": -126.21429443359375,
			"y": -251.26778411865234,
			"width": 32,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 62280481,
			"version": 88,
			"versionNonce": 544082273,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "JduBXMB2KiriJxqn9BAxM",
					"type": "arrow"
				}
			],
			"updated": 1697423293522,
			"link": null,
			"locked": false,
			"text": "文件",
			"rawText": "文件",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 14,
			"containerId": null,
			"originalText": "文件",
			"lineHeight": 1.15
		},
		{
			"type": "text",
			"version": 100,
			"versionNonce": 925462671,
			"isDeleted": false,
			"id": "VWo2JlrA",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -1.2144775390625,
			"y": -252.32502288818358,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 70.2421875,
			"height": 18.4,
			"seed": 1232529295,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "MTr7ruAWCAYCazoxzX-dc",
					"type": "arrow"
				}
			],
			"updated": 1697423288070,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "kafka消息",
			"rawText": "kafka消息",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kafka消息",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"id": "MTr7ruAWCAYCazoxzX-dc",
			"type": "arrow",
			"x": 28.884894644476688,
			"y": -189.24852669175584,
			"width": 23.085330389013812,
			"height": 37.44799888197463,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1089352737,
			"version": 202,
			"versionNonce": 1259148961,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423350183,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					23.085330389013812,
					-37.44799888197463
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "Z8JVc1JB",
				"focus": -0.28771357523414154,
				"gap": 1
			},
			"endBinding": {
				"elementId": "VWo2JlrA",
				"focus": -0.6910863350631905,
				"gap": 7.228497314453122
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"type": "text",
			"version": 115,
			"versionNonce": 1381307201,
			"isDeleted": false,
			"id": "eIVeAVdy",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 100.52166748046875,
			"y": -252.03925628662108,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 85.375,
			"height": 18.4,
			"seed": 2006794063,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697423297275,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "hbase的row",
			"rawText": "hbase的row",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "hbase的row",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 119,
			"versionNonce": 2042478049,
			"isDeleted": false,
			"id": "0S4SkHuF",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 213.38385009765625,
			"y": -250.1821090698242,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 92.4609375,
			"height": 18.4,
			"seed": 62339215,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697423299211,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "mysql-record",
			"rawText": "mysql-record",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "mysql-record",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"id": "gqeSiXhj1IuXjtaWfeMnh",
			"type": "arrow",
			"x": 12.64276123046875,
			"y": -70.6964340209961,
			"width": 0.571533203125,
			"height": 34.857177734375,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 129869441,
			"version": 22,
			"versionNonce": 902121295,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423181497,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.571533203125,
					34.857177734375
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "4NWHRlPUGV4-ZQopnj3AQ",
			"type": "arrow",
			"x": 14.3570556640625,
			"y": 16.160743713378906,
			"width": 0.571533203125,
			"height": 42.85711669921875,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1771100449,
			"version": 27,
			"versionNonce": 1445114753,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423185301,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.571533203125,
					42.85711669921875
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "K3aoGzsN6GNmS4oD34aN6",
			"type": "rectangle",
			"x": -115.92864990234375,
			"y": -201.55358123779297,
			"width": 82.28582763671875,
			"height": 30.857177734375,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 381009967,
			"version": 137,
			"versionNonce": 535106625,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "MTr7ruAWCAYCazoxzX-dc",
					"type": "arrow"
				}
			],
			"updated": 1697423340063,
			"link": null,
			"locked": false
		},
		{
			"id": "7Vp0M6iy",
			"type": "text",
			"x": -109.64276123046875,
			"y": -196.98201751708984,
			"width": 65.67366027832031,
			"height": 21.593430688753227,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 23239521,
			"version": 381,
			"versionNonce": 351640047,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "i9DEmJW47bm6PGN61F_wO",
					"type": "arrow"
				},
				{
					"id": "JduBXMB2KiriJxqn9BAxM",
					"type": "arrow"
				}
			],
			"updated": 1697423339654,
			"link": null,
			"locked": false,
			"text": "读本地磁盘文件\n的source实现类",
			"rawText": "读本地磁盘文件\n的source实现类",
			"fontSize": 9.388448125544882,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 19,
			"containerId": null,
			"originalText": "读本地磁盘文件\n的source实现类",
			"lineHeight": 1.15
		},
		{
			"type": "rectangle",
			"version": 154,
			"versionNonce": 998695375,
			"isDeleted": false,
			"id": "MADIBPycpGjHC-I3P5yNc",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -9.785858154296875,
			"y": -194.69646453857422,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 82.28582763671875,
			"height": 30.857177734375,
			"seed": 904917505,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "MTr7ruAWCAYCazoxzX-dc",
					"type": "arrow"
				}
			],
			"updated": 1697423304998,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 423,
			"versionNonce": 1287168495,
			"isDeleted": false,
			"id": "Z8JVc1JB",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 0.37728118896484375,
			"y": -188.63606318373206,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 65.67366027832031,
			"height": 21.593430688753227,
			"seed": 553246767,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "i9DEmJW47bm6PGN61F_wO",
					"type": "arrow"
				},
				{
					"id": "MTr7ruAWCAYCazoxzX-dc",
					"type": "arrow"
				},
				{
					"id": "y9lEA_8t8a1NcpFf0sHeG",
					"type": "arrow"
				}
			],
			"updated": 1697423354238,
			"link": null,
			"locked": false,
			"fontSize": 9.388448125544882,
			"fontFamily": 2,
			"text": "读kafka数据的\n的source实现类",
			"rawText": "读kafka数据的\n的source实现类",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "读kafka数据的\n的source实现类",
			"lineHeight": 1.15,
			"baseline": 19
		},
		{
			"id": "i9DEmJW47bm6PGN61F_wO",
			"type": "arrow",
			"x": -19.92864990234375,
			"y": -122.12505340576172,
			"width": 43.99993896484375,
			"height": 44,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1638512513,
			"version": 109,
			"versionNonce": 237988257,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423344464,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-43.99993896484375,
					-44
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "7Vp0M6iy",
				"focus": 0.1646183252325592,
				"gap": 9.263533422574923
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "y9lEA_8t8a1NcpFf0sHeG",
			"type": "arrow",
			"x": -17.0714111328125,
			"y": -120.98213958740234,
			"width": 38.8570556640625,
			"height": 41.71429443359375,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 760453761,
			"version": 31,
			"versionNonce": 1060844705,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423354238,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					38.8570556640625,
					-41.71429443359375
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "Z8JVc1JB",
				"focus": -0.062414913722167904,
				"gap": 4.346198473982767
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"type": "rectangle",
			"version": 139,
			"versionNonce": 921770593,
			"isDeleted": false,
			"id": "m6h5velbEnkfdtU7RhK4y",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -118.50015258789062,
			"y": 166.16068267822266,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 82.28582763671875,
			"height": 30.857177734375,
			"seed": 1444680207,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1697423369425,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 441,
			"versionNonce": 396152033,
			"isDeleted": false,
			"id": "Nj2qdbwQ",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -115.1940689086914,
			"y": 172.79255620103356,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 72.43385314941406,
			"height": 21.593430688753227,
			"seed": 1373943329,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "0erNgODmWfJPGCXLl4tol",
					"type": "arrow"
				},
				{
					"id": "vp-MU_eZipz0xlVICHLJx",
					"type": "arrow"
				}
			],
			"updated": 1697423467653,
			"link": null,
			"locked": false,
			"fontSize": 9.388448125544882,
			"fontFamily": 2,
			"text": "写数据到HDFS的\nsink实现类",
			"rawText": "写数据到HDFS的\nsink实现类",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "写数据到HDFS的\nsink实现类",
			"lineHeight": 1.15,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 159,
			"versionNonce": 1008921825,
			"isDeleted": false,
			"id": "MkZPCyf5mhLOCDsh1rVwI",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 14.928314208984375,
			"y": 163.16068267822266,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 82.28582763671875,
			"height": 30.857177734375,
			"seed": 1667691041,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1697423400510,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 450,
			"versionNonce": 1380961967,
			"isDeleted": false,
			"id": "H0xSvgfu",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 20.42583465576172,
			"y": 172.79255620103356,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 74.01397705078125,
			"height": 21.593430688753227,
			"seed": 94628001,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "aEmsG1QHcdwwjD8BESjFm",
					"type": "arrow"
				},
				{
					"id": "-09UNJ6FkAVQeuFdb6-wb",
					"type": "arrow"
				}
			],
			"updated": 1697423474325,
			"link": null,
			"locked": false,
			"fontSize": 9.388448125544882,
			"fontFamily": 2,
			"text": "写数据到Hbase的\nsink实现类",
			"rawText": "写数据到Hbase的\nsink实现类",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "写数据到Hbase的\nsink实现类",
			"lineHeight": 1.15,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 97,
			"versionNonce": 856852673,
			"isDeleted": false,
			"id": "npnZ0kGY",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -123.35723876953125,
			"y": 231.38927154541017,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 43.5546875,
			"height": 18.4,
			"seed": 1542455503,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "vp-MU_eZipz0xlVICHLJx",
					"type": "arrow"
				}
			],
			"updated": 1697423467654,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "HDFS",
			"rawText": "HDFS",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "HDFS",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 103,
			"versionNonce": 1732116527,
			"isDeleted": false,
			"id": "rCTQ8d62",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 35.86541748046875,
			"y": 229.38927154541017,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 46.25,
			"height": 18.4,
			"seed": 1095684015,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1697423435056,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "Hbase",
			"rawText": "Hbase",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Hbase",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"id": "0erNgODmWfJPGCXLl4tol",
			"type": "arrow",
			"x": 3.49993896484375,
			"y": 109.44647979736328,
			"width": 78.28564453125,
			"height": 55.428619384765625,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 2101221487,
			"version": 25,
			"versionNonce": 1290096769,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423458854,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-78.28564453125,
					55.428619384765625
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "74bj9U-_-opDNruuaDM3h",
				"focus": -0.15739691958789442,
				"gap": 2.142913818359375
			},
			"endBinding": {
				"elementId": "Nj2qdbwQ",
				"focus": -0.43212859583400937,
				"gap": 7.917457018904642
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "aEmsG1QHcdwwjD8BESjFm",
			"type": "arrow",
			"x": 8.64276123046875,
			"y": 107.16071319580078,
			"width": 48.0001220703125,
			"height": 53.714385986328125,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 533305441,
			"version": 31,
			"versionNonce": 1551085185,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423462246,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					48.0001220703125,
					53.714385986328125
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "H0xSvgfu",
				"focus": 0.4181286719954958,
				"gap": 11.917457018904642
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "vp-MU_eZipz0xlVICHLJx",
			"type": "arrow",
			"x": -79.35723876953125,
			"y": 204.8750991821289,
			"width": 25.71417236328125,
			"height": 18.8570556640625,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 702297697,
			"version": 26,
			"versionNonce": 600317903,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423469918,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-25.71417236328125,
					18.8570556640625
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "Nj2qdbwQ",
				"focus": -0.5623516680025825,
				"gap": 10.489112292342142
			},
			"endBinding": {
				"elementId": "npnZ0kGY",
				"focus": -0.7714550566142286,
				"gap": 7.657116699218747
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "-09UNJ6FkAVQeuFdb6-wb",
			"type": "arrow",
			"x": 48.07135009765625,
			"y": 202.01786041259766,
			"width": 12,
			"height": 31.4285888671875,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1948633249,
			"version": 13,
			"versionNonce": 2001279969,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423474325,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					12,
					31.4285888671875
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "H0xSvgfu",
				"focus": 0.39868930858456886,
				"gap": 7.631873522810892
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "LcWDS1SZB3DFogJ5BJwh8",
			"type": "rectangle",
			"x": 333.2142333984375,
			"y": -123.12511444091797,
			"width": 261.142822265625,
			"height": 182.28567504882815,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 2118335265,
			"version": 127,
			"versionNonce": 1600147407,
			"isDeleted": false,
			"boundElements": [],
			"updated": 1697423677421,
			"link": null,
			"locked": false
		},
		{
			"id": "XEfMzDjj",
			"type": "text",
			"x": 351.7600578879035,
			"y": -107.69649505615234,
			"width": 235.16587829589844,
			"height": 175.4289087680819,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1071499119,
			"version": 528,
			"versionNonce": 2122347809,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423666400,
			"link": null,
			"locked": false,
			"text": "Flume工作机制：\n        \n可以启动Flume的agent程序进行数据采集\n每个agent程序中包含三大组件\n(内部的数据流转：event)：\nsource\nchannel\nsink\n\nagent程序可以根据需要在多台机器上启动\n\n",
			"rawText": "Flume工作机制：\n        \n可以启动Flume的agent程序进行数据采集\n每个agent程序中包含三大组件\n(内部的数据流转：event)：\nsource\nchannel\nsink\n\nagent程序可以根据需要在多台机器上启动\n\n",
			"fontSize": 12.712239765803035,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 172,
			"containerId": null,
			"originalText": "Flume工作机制：\n        \n可以启动Flume的agent程序进行数据采集\n每个agent程序中包含三大组件\n(内部的数据流转：event)：\nsource\nchannel\nsink\n\nagent程序可以根据需要在多台机器上启动\n\n",
			"lineHeight": 1.15
		},
		{
			"id": "7GD1x-xZ9gFdw-OVENBP0",
			"type": "rectangle",
			"x": 340.6427001953125,
			"y": -36.267845153808594,
			"width": 62.85723876953125,
			"height": 47.999969482421875,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1055002159,
			"version": 66,
			"versionNonce": 1802828833,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1697423762159,
			"link": null,
			"locked": false
		},
		{
			"id": "jZBXVISg",
			"type": "text",
			"x": 461.56298828125,
			"y": -28.610774230957034,
			"width": 4.4453125,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#ffec99",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 738859873,
			"version": 3,
			"versionNonce": 57767713,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1697423507433,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 14,
			"containerId": "LcWDS1SZB3DFogJ5BJwh8",
			"originalText": "",
			"lineHeight": 1.15
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#e03131",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 2,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 202.1429443359375,
		"scrollY": 346.8750915527344,
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