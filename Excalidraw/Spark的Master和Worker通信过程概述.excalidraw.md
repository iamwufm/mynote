---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
Master ^GSV2nhpH

Worker ^9TI76jB8

Worker ^LiIUUXI0

Worker ^JZIOwxKd

    1.Worker会向Master注册自己（内存，核数，...）
    
    2.Master收到Worker的注册信息之后，会告诉Worker已注册成功
    
    3. Worker收到Master的注册成功消息之后，会定期向master汇报自己的状态，
    也就向Master报活（心跳）
    
    4.Master收到Worker的心跳信息之后，定期的更新Worker的状态，如心跳发送时间
    
    5.Master会定期监测发送过来的心跳时间和当前时间的差，如果大于5分钟，则认为Worker已死。
    然后Master再分任务的时候就不会给该Worker下发任务

 ^3myygBXy

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.9.14",
	"elements": [
		{
			"id": "lYO7NNgdJjdHfX34v9Z8W",
			"type": "ellipse",
			"x": -66.92855834960938,
			"y": -204.76785278320312,
			"width": 131,
			"height": 80,
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
			"seed": 1719805861,
			"version": 57,
			"versionNonce": 1069100203,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "GSV2nhpH"
				},
				{
					"id": "yfwak6WQanDMCnOldCFmf",
					"type": "arrow"
				}
			],
			"updated": 1698899455023,
			"link": null,
			"locked": false
		},
		{
			"id": "GSV2nhpH",
			"type": "text",
			"x": -31.805575954828242,
			"y": -176.05212403066503,
			"width": 61.123046875,
			"height": 23,
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
			"seed": 2042511973,
			"version": 46,
			"versionNonce": 648052645,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1698899417818,
			"link": null,
			"locked": false,
			"text": "Master",
			"rawText": "Master",
			"fontSize": 20,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 18,
			"containerId": "lYO7NNgdJjdHfX34v9Z8W",
			"originalText": "Master",
			"lineHeight": 1.15
		},
		{
			"type": "ellipse",
			"version": 80,
			"versionNonce": 863006693,
			"isDeleted": false,
			"id": "lTLrsOazwNfKldaOakS1r",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -254.21429443359375,
			"y": -14.267814636230469,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 131,
			"height": 80,
			"seed": 1469056453,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "9TI76jB8"
				},
				{
					"id": "yfwak6WQanDMCnOldCFmf",
					"type": "arrow"
				},
				{
					"id": "xRyH9VzDp9750PIsAm-I2",
					"type": "arrow"
				}
			],
			"updated": 1698899461903,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 74,
			"versionNonce": 1848924549,
			"isDeleted": false,
			"id": "9TI76jB8",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -220.57080422631262,
			"y": 14.44791411630763,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 64.08203125,
			"height": 23,
			"seed": 829752613,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1698899438769,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "Worker",
			"rawText": "Worker",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "lTLrsOazwNfKldaOakS1r",
			"originalText": "Worker",
			"lineHeight": 1.15,
			"baseline": 18
		},
		{
			"type": "ellipse",
			"version": 104,
			"versionNonce": 898131813,
			"isDeleted": false,
			"id": "TZuUk_onXImE06StrNqZi",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -76.64288330078125,
			"y": -9.696434020996094,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 131,
			"height": 80,
			"seed": 1898425253,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "LiIUUXI0"
				}
			],
			"updated": 1698899443679,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 101,
			"versionNonce": 320859499,
			"isDeleted": false,
			"id": "LiIUUXI0",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -42.99939309350012,
			"y": 19.019294731542004,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 64.08203125,
			"height": 23,
			"seed": 1759309573,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1698899443679,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "Worker",
			"rawText": "Worker",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "TZuUk_onXImE06StrNqZi",
			"originalText": "Worker",
			"lineHeight": 1.15,
			"baseline": 18
		},
		{
			"type": "ellipse",
			"version": 132,
			"versionNonce": 1036030821,
			"isDeleted": false,
			"id": "z7Gh8bHdfFDbjaoVhaS1N",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 99.64276123046875,
			"y": -16.124961853027344,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 131,
			"height": 80,
			"seed": 1230291275,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "JZIOwxKd"
				}
			],
			"updated": 1698899451376,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 129,
			"versionNonce": 1792049515,
			"isDeleted": false,
			"id": "JZIOwxKd",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 133.28625143774988,
			"y": 12.590766899510754,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 64.08203125,
			"height": 23,
			"seed": 1399087083,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1698899451376,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "Worker",
			"rawText": "Worker",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "z7Gh8bHdfFDbjaoVhaS1N",
			"originalText": "Worker",
			"lineHeight": 1.15,
			"baseline": 18
		},
		{
			"id": "yfwak6WQanDMCnOldCFmf",
			"type": "arrow",
			"x": -186,
			"y": -21.55352020263672,
			"width": 121.71429443359375,
			"height": 119.42861938476562,
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
			"seed": 1233313477,
			"version": 35,
			"versionNonce": 356353925,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1698899456736,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					121.71429443359375,
					-119.42861938476562
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "lTLrsOazwNfKldaOakS1r",
				"focus": -0.5894562674305242,
				"gap": 7.317868401303166
			},
			"endBinding": {
				"elementId": "lYO7NNgdJjdHfX34v9Z8W",
				"focus": 0.500536023347439,
				"gap": 7.079339519226558
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "xRyH9VzDp9750PIsAm-I2",
			"type": "arrow",
			"x": -23.14276123046875,
			"y": -127.83928680419922,
			"width": 124.00006103515625,
			"height": 116.57144165039062,
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
			"seed": 216293195,
			"version": 49,
			"versionNonce": 1139846731,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1698899461903,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-124.00006103515625,
					116.57144165039062
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "lTLrsOazwNfKldaOakS1r",
				"focus": 0.028340450907603,
				"gap": 5.49177789840914
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "3myygBXy",
			"type": "text",
			"x": 67.76442205750152,
			"y": -192.98206329345703,
			"width": 543.957763671875,
			"height": 185.85722351074224,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 637741893,
			"version": 1087,
			"versionNonce": 194775685,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1698900262880,
			"link": null,
			"locked": false,
			"text": "    1.Worker会向Master注册自己（内存，核数，...）\n    \n    2.Master收到Worker的注册信息之后，会告诉Worker已注册成功\n    \n    3. Worker收到Master的注册成功消息之后，会定期向master汇报自己的状态，\n    也就向Master报活（心跳）\n    \n    4.Master收到Worker的心跳信息之后，定期的更新Worker的状态，如心跳发送时间\n    \n    5.Master会定期监测发送过来的心跳时间和当前时间的差，如果大于5分钟，则认为Worker已死。\n    然后Master再分任务的时候就不会给该Worker下发任务\n\n",
			"rawText": "    1.Worker会向Master注册自己（内存，核数，...）\n    \n    2.Master收到Worker的注册信息之后，会告诉Worker已注册成功\n    \n    3. Worker收到Master的注册成功消息之后，会定期向master汇报自己的状态，\n    也就向Master报活（心跳）\n    \n    4.Master收到Worker的心跳信息之后，定期的更新Worker的状态，如心跳发送时间\n    \n    5.Master会定期监测发送过来的心跳时间和当前时间的差，如果大于5分钟，则认为Worker已死。\n    然后Master再分任务的时候就不会给该Worker下发任务\n\n",
			"fontSize": 12.431921305066371,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 183,
			"containerId": null,
			"originalText": "    1.Worker会向Master注册自己（内存，核数，...）\n    \n    2.Master收到Worker的注册信息之后，会告诉Worker已注册成功\n    \n    3. Worker收到Master的注册成功消息之后，会定期向master汇报自己的状态，\n    也就向Master报活（心跳）\n    \n    4.Master收到Worker的心跳信息之后，定期的更新Worker的状态，如心跳发送时间\n    \n    5.Master会定期监测发送过来的心跳时间和当前时间的差，如果大于5分钟，则认为Worker已死。\n    然后Master再分任务的时候就不会给该Worker下发任务\n\n",
			"lineHeight": 1.15
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#1971c2",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 0.5,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 2,
		"currentItemFontSize": 20,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 278.5,
		"scrollY": 321.3035888671875,
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