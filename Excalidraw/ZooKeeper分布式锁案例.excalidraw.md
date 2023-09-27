---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
/locks ^AzfUBwND

/seq-00 ^8rKjVeJS

/seq-01 ^ZCNZQhJr

/seq-02 ^Gl40d01E

zookeeper ^mltfcsq9

1.客户端启动时，在/locks节点下创建一个临时顺序节点

2.判断自己是不是当前节点下最小的节点。
是，获取到锁；
不是，对前一个节点进行监听

3.获取到锁，处理完业务后，delete节点释放锁，
然后下面的节点将收到通知
 ^EVjL6v7d

watch ^q7J0xzWE

watch ^P3vO78Pb

客户端 ^vdhfccde

客户端 ^eEo2ZH0c

客户端 ^iz2hMLc1

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.9.14",
	"elements": [
		{
			"id": "WKYvSry3-r4TAh3wc0G3v",
			"type": "rectangle",
			"x": -70.7857666015625,
			"y": -126.9107437133789,
			"width": 100,
			"height": 43,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
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
			"seed": 1817917617,
			"version": 108,
			"versionNonce": 1887163999,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "AzfUBwND"
				}
			],
			"updated": 1695692669532,
			"link": null,
			"locked": false
		},
		{
			"id": "AzfUBwND",
			"type": "text",
			"x": -46.3472900390625,
			"y": -116.9107437133789,
			"width": 51.123046875,
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
			"seed": 1081573489,
			"version": 40,
			"versionNonce": 1184531217,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695692669532,
			"link": null,
			"locked": false,
			"text": "/locks",
			"rawText": "/locks",
			"fontSize": 20,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 18,
			"containerId": "WKYvSry3-r4TAh3wc0G3v",
			"originalText": "/locks",
			"lineHeight": 1.15
		},
		{
			"id": "ESzBZmgyj3IwvPexLgYwR",
			"type": "rectangle",
			"x": 19.2860107421875,
			"y": -58.767860412597656,
			"width": 109,
			"height": 35,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#b2f2bb",
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
			"seed": 2074273279,
			"version": 81,
			"versionNonce": 1751845905,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "8rKjVeJS"
				},
				{
					"id": "ZmUlxLXF8S5jvu91CI3t0",
					"type": "arrow"
				},
				{
					"id": "5lccEC9vMIt5EJT7jMPvw",
					"type": "arrow"
				}
			],
			"updated": 1695693143668,
			"link": null,
			"locked": false
		},
		{
			"id": "8rKjVeJS",
			"type": "text",
			"x": 40.4315185546875,
			"y": -52.767860412597656,
			"width": 66.708984375,
			"height": 23,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#b2f2bb",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1772269489,
			"version": 32,
			"versionNonce": 284510079,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695692695181,
			"link": null,
			"locked": false,
			"text": "/seq-00",
			"rawText": "/seq-00",
			"fontSize": 20,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 18,
			"containerId": "ESzBZmgyj3IwvPexLgYwR",
			"originalText": "/seq-00",
			"lineHeight": 1.15
		},
		{
			"type": "rectangle",
			"version": 127,
			"versionNonce": 269974065,
			"isDeleted": false,
			"id": "WA60FLbiVOHyKB5tv04DG",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 22.5714111328125,
			"y": 1.9464187622070312,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#b2f2bb",
			"width": 109,
			"height": 35,
			"seed": 176939999,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "ZCNZQhJr"
				},
				{
					"id": "-0_kc0X6YDPfe2XJ1-vhy",
					"type": "arrow"
				},
				{
					"id": "5lccEC9vMIt5EJT7jMPvw",
					"type": "arrow"
				}
			],
			"updated": 1695693143668,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 80,
			"versionNonce": 359041503,
			"isDeleted": false,
			"id": "ZCNZQhJr",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 43.7169189453125,
			"y": 7.946418762207031,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#b2f2bb",
			"width": 66.708984375,
			"height": 23,
			"seed": 809635839,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695692705263,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "/seq-01",
			"rawText": "/seq-01",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "WA60FLbiVOHyKB5tv04DG",
			"originalText": "/seq-01",
			"lineHeight": 1.15,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 171,
			"versionNonce": 806707409,
			"isDeleted": false,
			"id": "gqAcODstgxvBf8o5Al1We",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 24.285888671875,
			"y": 62.946510314941406,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#b2f2bb",
			"width": 109,
			"height": 35,
			"seed": 274973247,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "Gl40d01E"
				}
			],
			"updated": 1695692722174,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 128,
			"versionNonce": 252350655,
			"isDeleted": false,
			"id": "Gl40d01E",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 45.431396484375,
			"y": 68.9465103149414,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#b2f2bb",
			"width": 66.708984375,
			"height": 23,
			"seed": 1078927967,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695692722174,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "/seq-02",
			"rawText": "/seq-02",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "gqAcODstgxvBf8o5Al1We",
			"originalText": "/seq-02",
			"lineHeight": 1.15,
			"baseline": 18
		},
		{
			"id": "C1yYtTQTgcE564Q8VkEJ1",
			"type": "arrow",
			"x": -26.78570556640625,
			"y": -86.6964340209961,
			"width": 57.1429443359375,
			"height": 174.857177734375,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#b2f2bb",
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
			"seed": 1762518641,
			"version": 138,
			"versionNonce": 1594472401,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695692756910,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-4.5714111328125,
					163.4285888671875
				],
				[
					52.571533203125,
					174.857177734375
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "-0_kc0X6YDPfe2XJ1-vhy",
			"type": "arrow",
			"x": -33.0714111328125,
			"y": 22.44647979736328,
			"width": 54.28570556640625,
			"height": 0,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#b2f2bb",
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
			"seed": 1166316977,
			"version": 57,
			"versionNonce": 528666289,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695693224903,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					54.28570556640625,
					0
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "WA60FLbiVOHyKB5tv04DG",
				"gap": 1.35711669921875,
				"focus": -0.17143205915178572
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "ZmUlxLXF8S5jvu91CI3t0",
			"type": "arrow",
			"x": -29.64276123046875,
			"y": -42.696372985839844,
			"width": 46.28564453125,
			"height": 1.142852783203125,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#b2f2bb",
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
			"seed": 334267743,
			"version": 35,
			"versionNonce": 580587729,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695693224901,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					46.28564453125,
					1.142852783203125
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "ESzBZmgyj3IwvPexLgYwR",
				"gap": 2.64312744140625,
				"focus": -0.059710162577817236
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "tU_vqVjKiHlcQ9ibZZl-0",
			"type": "rectangle",
			"x": -99.928466796875,
			"y": -140.41069793701172,
			"width": 248,
			"height": 256.5714416503906,
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
				"type": 3
			},
			"seed": 1317345265,
			"version": 52,
			"versionNonce": 729450559,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "3QUbyMRE6ioFsqAdpYI2c",
					"type": "arrow"
				},
				{
					"id": "ZsucvFQgl7_ZjxPWlZnXD",
					"type": "arrow"
				},
				{
					"id": "6kYy4jVUxvFPGVTLZ3CVX",
					"type": "arrow"
				}
			],
			"updated": 1695693248523,
			"link": null,
			"locked": false
		},
		{
			"id": "mltfcsq9",
			"type": "text",
			"x": -22.21429443359375,
			"y": -168.41072845458984,
			"width": 93.3984375,
			"height": 23,
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
			"seed": 113184465,
			"version": 38,
			"versionNonce": 2018643263,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695692805125,
			"link": null,
			"locked": false,
			"text": "zookeeper",
			"rawText": "zookeeper",
			"fontSize": 20,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 18,
			"containerId": null,
			"originalText": "zookeeper",
			"lineHeight": 1.15
		},
		{
			"id": "EVjL6v7d",
			"type": "text",
			"x": 173.78570556640625,
			"y": -96.98717541288832,
			"width": 339.26641845703125,
			"height": 144.018609987788,
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
			"seed": 291322897,
			"version": 620,
			"versionNonce": 1326035327,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695693091759,
			"link": null,
			"locked": false,
			"text": "1.客户端启动时，在/locks节点下创建一个临时顺序节点\n\n2.判断自己是不是当前节点下最小的节点。\n是，获取到锁；\n不是，对前一个节点进行监听\n\n3.获取到锁，处理完业务后，delete节点释放锁，\n然后下面的节点将收到通知\n",
			"rawText": "1.客户端启动时，在/locks节点下创建一个临时顺序节点\n\n2.判断自己是不是当前节点下最小的节点。\n是，获取到锁；\n不是，对前一个节点进行监听\n\n3.获取到锁，处理完业务后，delete节点释放锁，\n然后下面的节点将收到通知\n",
			"fontSize": 13.91484154471382,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 141,
			"containerId": null,
			"originalText": "1.客户端启动时，在/locks节点下创建一个临时顺序节点\n\n2.判断自己是不是当前节点下最小的节点。\n是，获取到锁；\n不是，对前一个节点进行监听\n\n3.获取到锁，处理完业务后，delete节点释放锁，\n然后下面的节点将收到通知\n",
			"lineHeight": 1.15
		},
		{
			"id": "bXAARm0cCxgpxWw6pqTri",
			"type": "arrow",
			"x": 130.9285888671875,
			"y": 86.44647979736328,
			"width": 21.71435546875,
			"height": 61.142852783203125,
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
				"type": 2
			},
			"seed": 1190960575,
			"version": 58,
			"versionNonce": 960753055,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "q7J0xzWE"
				}
			],
			"updated": 1695693132774,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					21.71435546875,
					-25.714324951171875
				],
				[
					0.5714111328125,
					-61.142852783203125
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "q7J0xzWE",
			"type": "text",
			"x": 131.7445068359375,
			"y": 51.5321548461914,
			"width": 41.796875,
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
			"seed": 1643275761,
			"version": 6,
			"versionNonce": 998211473,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695693136375,
			"link": null,
			"locked": false,
			"text": "watch",
			"rawText": "watch",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 14,
			"containerId": "bXAARm0cCxgpxWw6pqTri",
			"originalText": "watch",
			"lineHeight": 1.15
		},
		{
			"id": "5lccEC9vMIt5EJT7jMPvw",
			"type": "arrow",
			"x": 132.5714111328125,
			"y": 19.22551116567972,
			"width": 20.49969482421875,
			"height": 63.234356641887885,
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
				"type": 2
			},
			"seed": 595371551,
			"version": 74,
			"versionNonce": 702247135,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "P3vO78Pb"
				}
			],
			"updated": 1695693224904,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					17.21429443359375,
					-32.20765075308206
				],
				[
					-3.285400390625,
					-63.234356641887885
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "WA60FLbiVOHyKB5tv04DG",
				"gap": 1,
				"focus": 0.8673297517147455
			},
			"endBinding": {
				"elementId": "ESzBZmgyj3IwvPexLgYwR",
				"gap": 1,
				"focus": -0.8675276484638369
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "P3vO78Pb",
			"type": "text",
			"x": 128.88726806640625,
			"y": -22.182139587402343,
			"width": 41.796875,
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
			"seed": 2113591473,
			"version": 6,
			"versionNonce": 1370745425,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695693161861,
			"link": null,
			"locked": false,
			"text": "watch",
			"rawText": "watch",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 14,
			"containerId": "5lccEC9vMIt5EJT7jMPvw",
			"originalText": "watch",
			"lineHeight": 1.15
		},
		{
			"id": "FFp4gMOWjXI3P7Uej7hY9",
			"type": "rectangle",
			"x": -237.6429443359375,
			"y": -125.55358123779297,
			"width": 64,
			"height": 29,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#e9ecef",
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
			"seed": 2034530353,
			"version": 36,
			"versionNonce": 402600337,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "vdhfccde"
				}
			],
			"updated": 1695693219923,
			"link": null,
			"locked": false
		},
		{
			"id": "vdhfccde",
			"type": "text",
			"x": -229.6429443359375,
			"y": -120.25358123779297,
			"width": 48,
			"height": 18.4,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#b2f2bb",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1273800785,
			"version": 33,
			"versionNonce": 1095681553,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695693197742,
			"link": null,
			"locked": false,
			"text": "客户端",
			"rawText": "客户端",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 14,
			"containerId": "FFp4gMOWjXI3P7Uej7hY9",
			"originalText": "客户端",
			"lineHeight": 1.15
		},
		{
			"type": "rectangle",
			"version": 60,
			"versionNonce": 1896922335,
			"isDeleted": false,
			"id": "pLZzylEJErp8VbJ7w936K",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -237.9287109375,
			"y": -75.05355072021484,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#e9ecef",
			"width": 64,
			"height": 29,
			"seed": 1190837215,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "eEo2ZH0c"
				},
				{
					"id": "ZsucvFQgl7_ZjxPWlZnXD",
					"type": "arrow"
				}
			],
			"updated": 1695693244428,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 59,
			"versionNonce": 379613233,
			"isDeleted": false,
			"id": "eEo2ZH0c",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -229.9287109375,
			"y": -69.75355072021485,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#b2f2bb",
			"width": 48,
			"height": 18.4,
			"seed": 689301503,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695693204163,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "客户端",
			"rawText": "客户端",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "pLZzylEJErp8VbJ7w936K",
			"originalText": "客户端",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "rectangle",
			"version": 106,
			"versionNonce": 259941407,
			"isDeleted": false,
			"id": "-izmg-5YKolMRQZh85eK7",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -239.9285888671875,
			"y": -24.767906188964844,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#e9ecef",
			"width": 64,
			"height": 29,
			"seed": 1684936799,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "iz2hMLc1"
				},
				{
					"id": "6kYy4jVUxvFPGVTLZ3CVX",
					"type": "arrow"
				}
			],
			"updated": 1695693248523,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 106,
			"versionNonce": 232231153,
			"isDeleted": false,
			"id": "iz2hMLc1",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -231.9285888671875,
			"y": -19.467906188964843,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#b2f2bb",
			"width": 48,
			"height": 18.4,
			"seed": 1760146559,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695693232759,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "客户端",
			"rawText": "客户端",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "-izmg-5YKolMRQZh85eK7",
			"originalText": "客户端",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"id": "3QUbyMRE6ioFsqAdpYI2c",
			"type": "arrow",
			"x": -174.78570556640625,
			"y": -112.41072845458984,
			"width": 71.428466796875,
			"height": 13.142913818359375,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#e9ecef",
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
			"seed": 1266305745,
			"version": 51,
			"versionNonce": 2007254449,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695693240204,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					71.428466796875,
					13.142913818359375
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "tU_vqVjKiHlcQ9ibZZl-0",
				"focus": 0.42154220424698724,
				"gap": 3.42877197265625
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "ZsucvFQgl7_ZjxPWlZnXD",
			"type": "arrow",
			"x": -172.50006103515625,
			"y": -55.83928680419922,
			"width": 70.85711669921875,
			"height": 40,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#e9ecef",
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
			"seed": 1997952511,
			"version": 47,
			"versionNonce": 1930249361,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695693244428,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					70.85711669921875,
					-40
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "pLZzylEJErp8VbJ7w936K",
				"focus": 0.7242631212687161,
				"gap": 1.42864990234375
			},
			"endBinding": {
				"elementId": "tU_vqVjKiHlcQ9ibZZl-0",
				"focus": 0.7800973530127439,
				"gap": 1.7144775390625
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "6kYy4jVUxvFPGVTLZ3CVX",
			"type": "arrow",
			"x": -173.071533203125,
			"y": -10.124961853027344,
			"width": 70.857177734375,
			"height": 75.42861938476562,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#e9ecef",
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
			"seed": 1585467679,
			"version": 49,
			"versionNonce": 2032185681,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1695693248523,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					70.857177734375,
					-75.42861938476562
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "-izmg-5YKolMRQZh85eK7",
				"focus": 0.7669971173615091,
				"gap": 2.8570556640625
			},
			"endBinding": {
				"elementId": "tU_vqVjKiHlcQ9ibZZl-0",
				"focus": 0.7985915362298178,
				"gap": 2.285888671875
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#1e1e1e",
		"currentItemBackgroundColor": "#e9ecef",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 0.5,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 2,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 457.571533203125,
		"scrollY": 322.4464416503906,
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