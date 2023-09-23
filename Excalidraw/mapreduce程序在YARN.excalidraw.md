---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
hadoop的mapreduce框架：分布式运算程序
核心角色：
1、MRAppMaster
2、yarnchild(map task)
3、yarnchild(reduce task)
4、提交job的客户端 ^0yVnRVjO

HDFS ^ykazoUFu

客户端 ^IMyD51QE

resource manager ^7Wd6KZSm

node manager 1 ^4U7YnxoP

node manager 2 ^sezPDZ4R

node manager 3 ^bWKYtmeF

YARN ^joFDqwM5

hadoop jar wc.har xx.yy.Jobsubmitter
//job 参数设置

job.submit() ^1RpmUTU7

/root/wc.jar ^qjAUIhFw

1.请求resourcemanager，
需要一个Container（带参数） ^tqjmZTUV

2.返回一个jobid
以及一个用于提交
程序资源的路径
/tmp/root/yarn/.../.staging/ ^4eSR9kLf

/wordcount/input/a.txt b.txt ^jqvsqm8e

/tmp/root/yarn/.../.staging/jobid ^R8GXuSCi

job.split
job.xml
job.jar ^Lf8YGP1t

3.扫描输入目录中的文件，
形成若干个FileSplit切片对象，
放入一个ArrayList中 ^VRNpoReS

4.将job对象中的
参数写入HDFS ^pN6A6F2M

5.上传程序jar包 ^Ak2bUjIx

6.告知程序
资源提交完毕 ^Eg0ir4CS

task ^qArow0u8

任务队列 ^wMDzOFPK

7.领取任务
创建容器 ^hquGjt3m

容器 1.5g 1core ^ZYbhvhfh

8.下载程序资源 ^MpuXqo7o

job.split、job.xml、job.jar ^vTyb6020

9.发送shell命令
启动MRAppMaster ^lHbU8cd1

10.MRAppMaster启动 ^9xrPf4yl

11.请求N个容器
（运行maptask） ^H5ONDSDm

12.领取任务创建容器 ^18aAgoX3

容器 1g 1core ^QElHFbVZ

容器 1g 1core ^2b3FsV7s

13.下载程序资源 ^8D3yDq4w

job.split、job.xml、job.jar ^NdAnnvKk

job.split、job.xml、job.jar ^6rrgCrvl

      yarn child
      (map task) ^HtlA3KOG

      yarn child
      (map task) ^M3U6F4CY

14.发送shell命令启动yarn child(map task) ^g7re5wUV

14步之后，一旦有maptask处理完成，MRAppmaster就会请求若干容器
启动若干个yarnchild(reduce task)，这些reduce task进程启动后就会去
已完成任务的maptask端下载属于自己的分区数据；

当所有maptask都完成任务时，各reduce task也就下载全了属于自己的
分区数据，接下来就是合并数据、分组聚合（调reduce方法）、输出结果；

当所有的reduce task进程也完成任务后，MRAppMaster也会向ResourceManager注销；

当MRAppMaster退出后，客户端也就运行完毕、退出； ^7mDhJ7g6

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.9.14",
	"elements": [
		{
			"type": "text",
			"version": 414,
			"versionNonce": 1567421666,
			"isDeleted": false,
			"id": "0yVnRVjO",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -484.53919534122247,
			"y": -180.42816848667812,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 386.796875,
			"height": 138,
			"seed": 1234129150,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694171738007,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "hadoop的mapreduce框架：分布式运算程序\n核心角色：\n1、MRAppMaster\n2、yarnchild(map task)\n3、yarnchild(reduce task)\n4、提交job的客户端",
			"rawText": "hadoop的mapreduce框架：分布式运算程序\n核心角色：\n1、MRAppMaster\n2、yarnchild(map task)\n3、yarnchild(reduce task)\n4、提交job的客户端",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "hadoop的mapreduce框架：分布式运算程序\n核心角色：\n1、MRAppMaster\n2、yarnchild(map task)\n3、yarnchild(reduce task)\n4、提交job的客户端",
			"lineHeight": 1.15,
			"baseline": 133
		},
		{
			"type": "rectangle",
			"version": 195,
			"versionNonce": 1452573950,
			"isDeleted": false,
			"id": "ystbSXa64tiBut4nfN9SI",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -51.166664123535156,
			"y": -156.1197738647461,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 149.5,
			"height": 187.50000762939462,
			"seed": 1297175486,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694169614180,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 201,
			"versionNonce": 1836756670,
			"isDeleted": false,
			"id": "uXsfYjkCT_EDQRHpEDb4_",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 294.8334503173828,
			"y": -183.95314025878906,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 736.666625976563,
			"height": 222.66668701171875,
			"seed": 2068243490,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694170487710,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 54,
			"versionNonce": 1710699838,
			"isDeleted": false,
			"id": "DK43rJogELzfRqUt4SC2X",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -37.12500000000006,
			"y": 240.9752960205078,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 159.16664123535162,
			"height": 160.0000000000001,
			"seed": 343280510,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "ov6Fl-n8cwTr7dc_PKKBV",
					"type": "arrow"
				},
				{
					"id": "Xmjqwai20ehxG9EOF2dsU",
					"type": "arrow"
				},
				{
					"id": "BGCbFbJS9Inb_e3OrmXIA",
					"type": "arrow"
				}
			],
			"updated": 1694169614180,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 69,
			"versionNonce": 3163554,
			"isDeleted": false,
			"id": "ykazoUFu",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -12.125038146972685,
			"y": -196.52474212646493,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 43.5546875,
			"height": 18.4,
			"seed": 1030386046,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614180,
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
			"version": 21,
			"versionNonce": 236576126,
			"isDeleted": false,
			"id": "IMyD51QE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 18.708358764648438,
			"y": 212.64193725585938,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 48,
			"height": 18.4,
			"seed": 422913634,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614180,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "客户端",
			"rawText": "客户端",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "客户端",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "rectangle",
			"version": 82,
			"versionNonce": 1595621730,
			"isDeleted": false,
			"id": "WtnYftnHF2_kqwHWmjPKf",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 324.5417175292969,
			"y": -135.77470397949222,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 145,
			"height": 141,
			"seed": 307535870,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "NGxe4eO8UYpEvOLFp-LMO",
					"type": "arrow"
				},
				{
					"id": "Xmjqwai20ehxG9EOF2dsU",
					"type": "arrow"
				}
			],
			"updated": 1694169614180,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 39,
			"versionNonce": 608009662,
			"isDeleted": false,
			"id": "7Wd6KZSm",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 334.54164123535156,
			"y": -163.19138336181646,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 129.84375,
			"height": 18.4,
			"seed": 1189989730,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614180,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "resource manager",
			"rawText": "resource manager",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "resource manager",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "rectangle",
			"version": 131,
			"versionNonce": 1242254050,
			"isDeleted": false,
			"id": "nxBT4Uh4sowBfD1g-YbG8",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 499.54164123535156,
			"y": -137.7747421264649,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 145,
			"height": 140.8333587646485,
			"seed": 800558910,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "dLlpbfEKxzSZiugStlp7X",
					"type": "arrow"
				}
			],
			"updated": 1694170516782,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 122,
			"versionNonce": 627778978,
			"isDeleted": false,
			"id": "hS_2rDcToNuSxtVNX2-4W",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 669.9582824707031,
			"y": -137.7747421264649,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 145,
			"height": 140.8333587646485,
			"seed": 908963454,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "lx-XqP4jD241x3twBqNRu",
					"type": "arrow"
				}
			],
			"updated": 1694170549454,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 177,
			"versionNonce": 486973886,
			"isDeleted": false,
			"id": "cbs3SWN66m2yVzh7sXAQF",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 867.0415649414062,
			"y": -140.69134521484384,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 145,
			"height": 140.8333587646485,
			"seed": 2026698338,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "XZiqv-hljh7HozLSKkteq",
					"type": "arrow"
				},
				{
					"id": "xsTdoHa500x3CQT8C4I8f",
					"type": "arrow"
				}
			],
			"updated": 1694170631638,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 103,
			"versionNonce": 1659610686,
			"isDeleted": false,
			"id": "4U7YnxoP",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 518.7864074707031,
			"y": -169.8913452148438,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 116.53125,
			"height": 18.4,
			"seed": 1537064674,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "node manager 1",
			"rawText": "node manager 1",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "node manager 1",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 121,
			"versionNonce": 133932450,
			"isDeleted": false,
			"id": "sezPDZ4R",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 680.4426574707031,
			"y": -170.72466583251958,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 116.53125,
			"height": 18.4,
			"seed": 480604670,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "8u0rS1ZyI22Iy5XK8fW-c",
					"type": "arrow"
				}
			],
			"updated": 1694170021487,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "node manager 2",
			"rawText": "node manager 2",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "node manager 2",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 136,
			"versionNonce": 89824610,
			"isDeleted": false,
			"id": "bWKYtmeF",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 873.7759399414062,
			"y": -171.1413452148438,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 116.53125,
			"height": 18.4,
			"seed": 53339198,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "yWnpHR9kkjRJLvZke8mC4",
					"type": "arrow"
				}
			],
			"updated": 1694170026503,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "node manager 3",
			"rawText": "node manager 3",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "node manager 3",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 151,
			"versionNonce": 241598562,
			"isDeleted": false,
			"id": "joFDqwM5",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 641.0976562500001,
			"y": -224.05800552368163,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 43.265625,
			"height": 18.4,
			"seed": 1959276606,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "YARN",
			"rawText": "YARN",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "YARN",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 152,
			"versionNonce": 1367977662,
			"isDeleted": false,
			"id": "1RpmUTU7",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -31.291641235351733,
			"y": 260.1420135498047,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 262.9296875,
			"height": 73.6,
			"seed": 1942031166,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "NGxe4eO8UYpEvOLFp-LMO",
					"type": "arrow"
				},
				{
					"id": "fI16n8mhYse-iGxCj9ba7",
					"type": "arrow"
				},
				{
					"id": "BGCbFbJS9Inb_e3OrmXIA",
					"type": "arrow"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "hadoop jar wc.har xx.yy.Jobsubmitter\n//job 参数设置\n\njob.submit()",
			"rawText": "hadoop jar wc.har xx.yy.Jobsubmitter\n//job 参数设置\n\njob.submit()",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "hadoop jar wc.har xx.yy.Jobsubmitter\n//job 参数设置\n\njob.submit()",
			"lineHeight": 1.15,
			"baseline": 69
		},
		{
			"type": "text",
			"version": 41,
			"versionNonce": 675030050,
			"isDeleted": false,
			"id": "qjAUIhFw",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -27.125000000000114,
			"y": 365.14208984375,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 78.2421875,
			"height": 18.4,
			"seed": 1578036798,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "5zWmqM1nJ0hvdhzF2smLC",
					"type": "arrow"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "/root/wc.jar",
			"rawText": "/root/wc.jar",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "/root/wc.jar",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "arrow",
			"version": 535,
			"versionNonce": 606152446,
			"isDeleted": false,
			"id": "NGxe4eO8UYpEvOLFp-LMO",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 108.70828247070307,
			"y": 247.6420135498047,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 214.16474538384426,
			"height": 313.3302999059783,
			"seed": 947857150,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "1RpmUTU7",
				"focus": -0.17209309891128746,
				"gap": 12.5
			},
			"endBinding": {
				"elementId": "WtnYftnHF2_kqwHWmjPKf",
				"gap": 1.6686896747495459,
				"focus": 0.6252425720659993
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
					79.16671752929688,
					-109.16667938232422
				],
				[
					214.16474538384426,
					-313.3302999059783
				]
			]
		},
		{
			"type": "text",
			"version": 194,
			"versionNonce": 120016866,
			"isDeleted": false,
			"id": "tqjmZTUV",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 23.708435058593693,
			"y": 156.8086929321289,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 199.10540771484375,
			"height": 34.35673158776361,
			"seed": 1180758782,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "1.请求resourcemanager，\n需要一个Container（带参数）",
			"rawText": "1.请求resourcemanager，\n需要一个Container（带参数）",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1.请求resourcemanager，\n需要一个Container（带参数）",
			"lineHeight": 1.15,
			"baseline": 31
		},
		{
			"type": "arrow",
			"version": 219,
			"versionNonce": 1570740030,
			"isDeleted": false,
			"id": "ov6Fl-n8cwTr7dc_PKKBV",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 327.8987150854057,
			"y": -8.121830236717301,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 202.42839807005862,
			"height": 276.6208841789725,
			"seed": 619840546,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "OYCTC9fyw_dHDwr9dIFYF",
				"gap": 9.142926149945737,
				"focus": 0.7579334736796125
			},
			"endBinding": {
				"elementId": "DK43rJogELzfRqUt4SC2X",
				"gap": 3.428675779995509,
				"focus": 0.3229677158130782
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
					-202.42839807005862,
					276.6208841789725
				]
			]
		},
		{
			"type": "text",
			"version": 344,
			"versionNonce": 2134311842,
			"isDeleted": false,
			"id": "4eSR9kLf",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 209.9374618530273,
			"y": 102.13036528521974,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 169.2817840576172,
			"height": 68.71346317552722,
			"seed": 1724775294,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "2.返回一个jobid\n以及一个用于提交\n程序资源的路径\n/tmp/root/yarn/.../.staging/",
			"rawText": "2.返回一个jobid\n以及一个用于提交\n程序资源的路径\n/tmp/root/yarn/.../.staging/",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "2.返回一个jobid\n以及一个用于提交\n程序资源的路径\n/tmp/root/yarn/.../.staging/",
			"lineHeight": 1.15,
			"baseline": 65
		},
		{
			"type": "text",
			"version": 51,
			"versionNonce": 1986396030,
			"isDeleted": false,
			"id": "jqvsqm8e",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -37.125000000000114,
			"y": -129.8579483032227,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 186.765625,
			"height": 18.4,
			"seed": 1356405822,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "fI16n8mhYse-iGxCj9ba7",
					"type": "arrow"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "/wordcount/input/a.txt b.txt",
			"rawText": "/wordcount/input/a.txt b.txt",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "/wordcount/input/a.txt b.txt",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 31,
			"versionNonce": 332337470,
			"isDeleted": false,
			"id": "R8GXuSCi",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -43.79160308837902,
			"y": -64.02458953857428,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 215.21875,
			"height": 18.4,
			"seed": 1272015038,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "qf5j6GsOT20h2s8KLH05j",
					"type": "arrow"
				},
				{
					"id": "JMmLW8_FjOzFq12Z_P1Q9",
					"type": "arrow"
				}
			],
			"updated": 1694170008455,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "/tmp/root/yarn/.../.staging/jobid",
			"rawText": "/tmp/root/yarn/.../.staging/jobid",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "/tmp/root/yarn/.../.staging/jobid",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "rectangle",
			"version": 171,
			"versionNonce": 850747326,
			"isDeleted": false,
			"id": "RUP1a9PIiVxbQ1DugtTsi",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 3.5417175292968466,
			"y": -43.191268920898494,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 75,
			"height": 66,
			"seed": 487649250,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "Lf8YGP1t"
				},
				{
					"id": "wHcjuohk_ORoX0Scoj5v5",
					"type": "arrow"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 65,
			"versionNonce": 94372642,
			"isDeleted": false,
			"id": "Lf8YGP1t",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 13.916717529296847,
			"y": -37.79126892089849,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 54.25,
			"height": 55.199999999999996,
			"seed": 98612414,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "job.split\njob.xml\njob.jar",
			"rawText": "job.split\njob.xml\njob.jar",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "RUP1a9PIiVxbQ1DugtTsi",
			"originalText": "job.split\njob.xml\njob.jar",
			"lineHeight": 1.15,
			"baseline": 51
		},
		{
			"type": "arrow",
			"version": 154,
			"versionNonce": 648984574,
			"isDeleted": false,
			"id": "fI16n8mhYse-iGxCj9ba7",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -32.1249237060548,
			"y": 334.3088073730469,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 265.00000000000006,
			"height": 457.50000000000006,
			"seed": 1241629118,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "1RpmUTU7",
				"focus": -1.0086238872625999,
				"gap": 1
			},
			"endBinding": {
				"elementId": "jqvsqm8e",
				"focus": 0.9459819573478985,
				"gap": 1.6666412353515625
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
					-265.00000000000006,
					-220
				],
				[
					-6.666717529296875,
					-457.50000000000006
				]
			]
		},
		{
			"type": "text",
			"version": 323,
			"versionNonce": 1790398178,
			"isDeleted": false,
			"id": "VRNpoReS",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -362.0943450927736,
			"y": 106.29708281451661,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 202.40048217773438,
			"height": 51.535097381645414,
			"seed": 645737762,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "qf5j6GsOT20h2s8KLH05j",
					"type": "arrow"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "3.扫描输入目录中的文件，\n形成若干个FileSplit切片对象，\n放入一个ArrayList中",
			"rawText": "3.扫描输入目录中的文件，\n形成若干个FileSplit切片对象，\n放入一个ArrayList中",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "3.扫描输入目录中的文件，\n形成若干个FileSplit切片对象，\n放入一个ArrayList中",
			"lineHeight": 1.15,
			"baseline": 48
		},
		{
			"type": "arrow",
			"version": 80,
			"versionNonce": 978520126,
			"isDeleted": false,
			"id": "qf5j6GsOT20h2s8KLH05j",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -252.9582824707033,
			"y": 102.64212799072266,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 263.3333587646485,
			"height": 135.00000000000006,
			"seed": 1101092478,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "VRNpoReS",
				"focus": -0.32652376994769694,
				"gap": 3.6549548237939575
			},
			"endBinding": {
				"elementId": "R8GXuSCi",
				"focus": 0.07660961003921479,
				"gap": 13.266717529296873
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
					263.3333587646485,
					-135.00000000000006
				]
			]
		},
		{
			"type": "arrow",
			"version": 166,
			"versionNonce": 242164386,
			"isDeleted": false,
			"id": "31ghpInagemxceao4_req",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -14.624923706054801,
			"y": 278.47544860839844,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 147.4999237060547,
			"height": 290.00000000000006,
			"seed": 2053390526,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"startBinding": null,
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
					-119.16664123535156,
					-124.16664123535156
				],
				[
					28.333282470703125,
					-290.00000000000006
				]
			]
		},
		{
			"type": "text",
			"version": 443,
			"versionNonce": 763580542,
			"isDeleted": false,
			"id": "pN6A6F2M",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -164.15852355957037,
			"y": 173.54125868222417,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 107.02497863769531,
			"height": 34.35673158776361,
			"seed": 1219206306,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "4.将job对象中的\n参数写入HDFS",
			"rawText": "4.将job对象中的\n参数写入HDFS",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "4.将job对象中的\n参数写入HDFS",
			"lineHeight": 1.15,
			"baseline": 31
		},
		{
			"type": "arrow",
			"version": 47,
			"versionNonce": 1534492258,
			"isDeleted": false,
			"id": "5zWmqM1nJ0hvdhzF2smLC",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 13.708358764648324,
			"y": 362.6421661376953,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 11.666717529296875,
			"height": 341.6667175292969,
			"seed": 976156898,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "qjAUIhFw",
				"focus": 0.03328884762350712,
				"gap": 2.4999237060546875
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
					11.666717529296875,
					-341.6667175292969
				]
			]
		},
		{
			"type": "text",
			"version": 540,
			"versionNonce": 469799102,
			"isDeleted": false,
			"id": "Ak2bUjIx",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -21.470771789550895,
			"y": 95.46376219684083,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 103.69343566894531,
			"height": 17.178365793881806,
			"seed": 1963044030,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "5.上传程序jar包",
			"rawText": "5.上传程序jar包",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "5.上传程序jar包",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "arrow",
			"version": 136,
			"versionNonce": 245129762,
			"isDeleted": false,
			"id": "Xmjqwai20ehxG9EOF2dsU",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 132.0535501311877,
			"y": 306.79244437940395,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 218.42751280821415,
			"height": 300.64928572541027,
			"seed": 271559102,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "DK43rJogELzfRqUt4SC2X",
				"gap": 10.011908895836127,
				"focus": 0.5758047360652134
			},
			"endBinding": {
				"elementId": "WtnYftnHF2_kqwHWmjPKf",
				"gap": 1.0011832511616086,
				"focus": -0.043068427152294454
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
					218.42751280821415,
					-300.64928572541027
				]
			]
		},
		{
			"type": "text",
			"version": 294,
			"versionNonce": 23841022,
			"isDeleted": false,
			"id": "Eg0ir4CS",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 201.65557861328114,
			"y": 207.13036528521974,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 89.5799560546875,
			"height": 34.35673158776361,
			"seed": 1148894078,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "6.告知程序\n资源提交完毕",
			"rawText": "6.告知程序\n资源提交完毕",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "6.告知程序\n资源提交完毕",
			"lineHeight": 1.15,
			"baseline": 31
		},
		{
			"type": "rectangle",
			"version": 104,
			"versionNonce": 1787283746,
			"isDeleted": false,
			"id": "OYCTC9fyw_dHDwr9dIFYF",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 337.04164123535145,
			"y": -46.52451324462896,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 127.5,
			"height": 39.999961853027344,
			"seed": 403097058,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "ov6Fl-n8cwTr7dc_PKKBV",
					"type": "arrow"
				},
				{
					"id": "CVGJRfYbm0bTlB7we9rwD",
					"type": "arrow"
				},
				{
					"id": "kNCP2B_lNpyK_Ec71l-RU",
					"type": "arrow"
				}
			],
			"updated": 1694169957825,
			"link": null,
			"locked": false
		},
		{
			"type": "ellipse",
			"version": 242,
			"versionNonce": 116359063,
			"isDeleted": false,
			"id": "drEeIyTJ0fF7NPU62ZiEF",
			"fillStyle": "solid",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 368.45843505859364,
			"y": -46.52455139160162,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 61,
			"height": 41,
			"seed": 46330658,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "qArow0u8"
				}
			],
			"updated": 1695348316007,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 200,
			"versionNonce": 2069194359,
			"isDeleted": false,
			"id": "qArow0u8",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 384.21980323240393,
			"y": -35.22024040592585,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 29.34375,
			"height": 18.4,
			"seed": 1328626402,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1695348311990,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "task",
			"rawText": "task",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "drEeIyTJ0fF7NPU62ZiEF",
			"originalText": "task",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 408,
			"versionNonce": 384835966,
			"isDeleted": false,
			"id": "wMDzOFPK",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 390.1683807373045,
			"y": -65.14705490621832,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 52.39996337890625,
			"height": 15.066679382324217,
			"seed": 1511285154,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "I8-t6VDvIkNBzzu4cvXId",
					"type": "arrow"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 13.101460332455842,
			"fontFamily": 2,
			"text": "任务队列",
			"rawText": "任务队列",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "任务队列",
			"lineHeight": 1.15,
			"baseline": 12
		},
		{
			"type": "rectangle",
			"version": 80,
			"versionNonce": 683359586,
			"isDeleted": false,
			"id": "4NzzAGz4cPP0B5Nl4vjIy",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 515.3749999999999,
			"y": -97.3578720092774,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 120,
			"height": 78.33335876464842,
			"seed": 611257954,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 93,
			"versionNonce": 1101742526,
			"isDeleted": false,
			"id": "I8-t6VDvIkNBzzu4cvXId",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 395.3749999999998,
			"y": -47.35794830322271,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 127.5,
			"height": 47.499961853027344,
			"seed": 39405986,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "wMDzOFPK",
				"focus": -0.37042531176876464,
				"gap": 2.7224272206713813
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
					37.50000000000006,
					-43.33332061767578
				],
				[
					127.5,
					-47.499961853027344
				]
			]
		},
		{
			"type": "text",
			"version": 374,
			"versionNonce": 1928413474,
			"isDeleted": false,
			"id": "hquGjt3m",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 394.75166320800764,
			"y": -122.0363522440772,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 72.17134094238281,
			"height": 34.35673158776361,
			"seed": 1449438782,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "7.领取任务\n创建容器",
			"rawText": "7.领取任务\n创建容器",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "7.领取任务\n创建容器",
			"lineHeight": 1.15,
			"baseline": 31
		},
		{
			"type": "text",
			"version": 422,
			"versionNonce": 1620597246,
			"isDeleted": false,
			"id": "ZYbhvhfh",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 520.1226119995115,
			"y": -128.7029553324561,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 104.56092834472656,
			"height": 17.178365793881806,
			"seed": 240171746,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "wHcjuohk_ORoX0Scoj5v5",
					"type": "arrow"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "容器 1.5g 1core",
			"rawText": "容器 1.5g 1core",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "容器 1.5g 1core",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "rectangle",
			"version": 124,
			"versionNonce": 1698709054,
			"isDeleted": false,
			"id": "4VBU3myoVBlWIiDd1ssG5",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 522.0417175292966,
			"y": -78.19123077392584,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 103.33335876464841,
			"height": 37.5,
			"seed": 2094762686,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694169646412,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 231,
			"versionNonce": 346241305,
			"isDeleted": false,
			"id": "wHcjuohk_ORoX0Scoj5v5",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 42.04164123535128,
			"y": -47.35794830322271,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 500.8333587646486,
			"height": 202.50000000000003,
			"seed": 1807124414,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1695348290147,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "RUP1a9PIiVxbQ1DugtTsi",
				"gap": 4.166679382324219,
				"focus": -0.4984925145197734
			},
			"endBinding": {
				"elementId": "ZYbhvhfh",
				"focus": 0.51916390946901,
				"gap": 15.32167235309084
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
					192.50000000000006,
					-202.50000000000003
				],
				[
					500.8333587646486,
					-96.66667938232422
				]
			]
		},
		{
			"type": "text",
			"version": 424,
			"versionNonce": 1795743906,
			"isDeleted": false,
			"id": "MpuXqo7o",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 185.95597076415987,
			"y": -257.03631409710454,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 102.03132629394531,
			"height": 17.178365793881806,
			"seed": 1563169982,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "8.下载程序资源",
			"rawText": "8.下载程序资源",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "8.下载程序资源",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 506,
			"versionNonce": 1324118654,
			"isDeleted": false,
			"id": "vTyb6020",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 526.7892532348633,
			"y": -34.119634714780304,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 127.14498901367188,
			"height": 12.96419146325904,
			"seed": 1574765026,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 11.27320996805134,
			"fontFamily": 2,
			"text": "job.split、job.xml、job.jar",
			"rawText": "job.split、job.xml、job.jar",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "job.split、job.xml、job.jar",
			"lineHeight": 1.15,
			"baseline": 10
		},
		{
			"type": "arrow",
			"version": 101,
			"versionNonce": 975731810,
			"isDeleted": false,
			"id": "BGCbFbJS9Inb_e3OrmXIA",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 125.37496185302706,
			"y": 373.4754104614258,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 404.1666793823243,
			"height": 370.83335876464844,
			"seed": 372339838,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "DK43rJogELzfRqUt4SC2X",
				"focus": 0.8402723975846983,
				"gap": 3.333320617675497
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
					404.1666793823243,
					-370.83335876464844
				]
			]
		},
		{
			"type": "text",
			"version": 341,
			"versionNonce": 938832574,
			"isDeleted": false,
			"id": "lHbU8cd1",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 238.9183044433591,
			"y": 262.9636859028955,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 125.27194213867188,
			"height": 34.35673158776361,
			"seed": 907233790,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "9.发送shell命令\n启动MRAppMaster",
			"rawText": "9.发送shell命令\n启动MRAppMaster",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "9.发送shell命令\n启动MRAppMaster",
			"lineHeight": 1.15,
			"baseline": 31
		},
		{
			"type": "text",
			"version": 476,
			"versionNonce": 1465036222,
			"isDeleted": false,
			"id": "9xrPf4yl",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 525.585021972656,
			"y": -67.0363522440772,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 126.07388305664062,
			"height": 14.82559457600895,
			"seed": 620922466,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "CVGJRfYbm0bTlB7we9rwD",
					"type": "arrow"
				}
			],
			"updated": 1694169753849,
			"link": null,
			"locked": false,
			"fontSize": 12.891821370442566,
			"fontFamily": 2,
			"text": "10.MRAppMaster启动",
			"rawText": "10.MRAppMaster启动",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "10.MRAppMaster启动",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "arrow",
			"version": 162,
			"versionNonce": 2463970,
			"isDeleted": false,
			"id": "CVGJRfYbm0bTlB7we9rwD",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 576.2083587646482,
			"y": -39.02462768554693,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 113.33335876464844,
			"height": 105.00000000000006,
			"seed": 282795362,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169760353,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "9xrPf4yl",
				"focus": 0.17456574484977916,
				"gap": 13.186129982521308
			},
			"endBinding": {
				"elementId": "OYCTC9fyw_dHDwr9dIFYF",
				"focus": -0.21671125340838732,
				"gap": 5.833282470703125
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
					-6.6666412353515625,
					105.00000000000006
				],
				[
					-113.33335876464844,
					38.33335876464844
				]
			]
		},
		{
			"type": "text",
			"version": 431,
			"versionNonce": 716592354,
			"isDeleted": false,
			"id": "H5ONDSDm",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 511.78932952880837,
			"y": 71.29700652057132,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 116.14482116699219,
			"height": 34.35673158776361,
			"seed": 1654023870,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "kNCP2B_lNpyK_Ec71l-RU",
					"type": "arrow"
				}
			],
			"updated": 1694169957825,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "11.请求N个容器\n（运行maptask）",
			"rawText": "11.请求N个容器\n（运行maptask）",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "11.请求N个容器\n（运行maptask）",
			"lineHeight": 1.15,
			"baseline": 31
		},
		{
			"type": "line",
			"version": 180,
			"versionNonce": 2073225570,
			"isDeleted": false,
			"id": "4Ga8Py4WeIDQHY8vDYSFh",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 420.3749999999998,
			"y": 122.64205169677734,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 598.3333587646486,
			"height": 0.8333206176757812,
			"seed": 1838507774,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169871465,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					598.3333587646486,
					0.8333206176757812
				]
			]
		},
		{
			"type": "arrow",
			"version": 166,
			"versionNonce": 562783486,
			"isDeleted": false,
			"id": "186LRxW0X18DJ_ifcTf_M",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 750.3749999999998,
			"y": 118.47537231445312,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 0.8409290842660084,
			"height": 113.00003051757818,
			"seed": 1710452002,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694170178400,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "EIMN4dmSgd1xE56t68-Hi",
				"focus": -0.11718636132599182,
				"gap": 18.666648864746094
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
					0.8409290842660084,
					-113.00003051757818
				]
			]
		},
		{
			"type": "arrow",
			"version": 54,
			"versionNonce": 1813913506,
			"isDeleted": false,
			"id": "XZiqv-hljh7HozLSKkteq",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 944.5417175292968,
			"y": 121.80873107910156,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 0,
			"height": 116.66667938232428,
			"seed": 678112446,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169889304,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "cbs3SWN66m2yVzh7sXAQF",
				"focus": -0.0689676219019381,
				"gap": 5.000038146972628
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
					0,
					-116.66667938232428
				]
			]
		},
		{
			"type": "text",
			"version": 466,
			"versionNonce": 1605171746,
			"isDeleted": false,
			"id": "18aAgoX3",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 652.3025894165037,
			"y": 138.79700652057136,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 140.1946563720703,
			"height": 17.178365793881806,
			"seed": 220457698,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694169903991,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "12.领取任务创建容器",
			"rawText": "12.领取任务创建容器",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "12.领取任务创建容器",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "arrow",
			"version": 48,
			"versionNonce": 965628414,
			"isDeleted": false,
			"id": "kNCP2B_lNpyK_Ec71l-RU",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 426.2083587646482,
			"y": 6.808731079101506,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 84.16664123535156,
			"height": 112.50000000000006,
			"seed": 712207358,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169957825,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "OYCTC9fyw_dHDwr9dIFYF",
				"focus": -0.006079124433172304,
				"gap": 13.333282470703125
			},
			"endBinding": {
				"elementId": "H5ONDSDm",
				"focus": -1.1639814956112604,
				"gap": 13.654992970766635
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
					84.16664123535156,
					112.50000000000006
				]
			]
		},
		{
			"type": "line",
			"version": 179,
			"versionNonce": 1404424738,
			"isDeleted": false,
			"id": "sO2KJqPdkDrBeyTEWNf8O",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 386.2084350585935,
			"y": -264.85800552368175,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 621.6665649414065,
			"height": 2.5,
			"seed": 1151166626,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694169997095,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					621.6665649414065,
					2.5
				]
			]
		},
		{
			"type": "arrow",
			"version": 64,
			"versionNonce": 559163810,
			"isDeleted": false,
			"id": "JMmLW8_FjOzFq12Z_P1Q9",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 65.37507629394503,
			"y": -33.191268920898494,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 391.6666412353516,
			"height": 234.16666030883795,
			"seed": 513618338,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694170009584,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "R8GXuSCi",
				"focus": 0.3068454074081149,
				"gap": 12.43332061767578
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
					391.6666412353516,
					-234.16666030883795
				]
			]
		},
		{
			"type": "arrow",
			"version": 30,
			"versionNonce": 1374016830,
			"isDeleted": false,
			"id": "8u0rS1ZyI22Iy5XK8fW-c",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 746.2083969116209,
			"y": -263.19126892089855,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 0,
			"height": 117.50000000000006,
			"seed": 2049090914,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694170021486,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "sezPDZ4R",
				"focus": 0.1287228008095295,
				"gap": 6.633396911621091
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
					0,
					117.50000000000006
				]
			]
		},
		{
			"type": "arrow",
			"version": 32,
			"versionNonce": 1925890430,
			"isDeleted": false,
			"id": "yWnpHR9kkjRJLvZke8mC4",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 947.8750381469724,
			"y": -259.02458953857433,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 2.5,
			"height": 118.33332061767584,
			"seed": 377135458,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694170026503,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "bWKYtmeF",
				"focus": 0.3059276525854258,
				"gap": 12.05007629394531
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
					2.5,
					118.33332061767584
				]
			]
		},
		{
			"type": "rectangle",
			"version": 117,
			"versionNonce": 1509564770,
			"isDeleted": false,
			"id": "EIMN4dmSgd1xE56t68-Hi",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 687.8750381469724,
			"y": -102.35794830322271,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 114.16664123535153,
			"height": 89.16664123535156,
			"seed": 1045101054,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "186LRxW0X18DJ_ifcTf_M",
					"type": "arrow"
				},
				{
					"id": "lx-XqP4jD241x3twBqNRu",
					"type": "arrow"
				}
			],
			"updated": 1694170549454,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 450,
			"versionNonce": 4604094,
			"isDeleted": false,
			"id": "QElHFbVZ",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 699.3444976806638,
			"y": -129.28045181783943,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 92.10955810546875,
			"height": 17.178365793881806,
			"seed": 1896266878,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694170070679,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "容器 1g 1core",
			"rawText": "容器 1g 1core",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "容器 1g 1core",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "rectangle",
			"version": 137,
			"versionNonce": 1525779326,
			"isDeleted": false,
			"id": "kM-KhjGKq-Sgox6fhYEvk",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 878.2916412353513,
			"y": -106.9412689208985,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 116.66664123535158,
			"height": 82.5,
			"seed": 2117669282,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694170125008,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 484,
			"versionNonce": 1784792674,
			"isDeleted": false,
			"id": "2b3FsV7s",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 900.1536178588865,
			"y": -130.94713120016365,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 92.10955810546875,
			"height": 17.178365793881806,
			"seed": 363691070,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694170096711,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "容器 1g 1core",
			"rawText": "容器 1g 1core",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "容器 1g 1core",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "rectangle",
			"version": 141,
			"versionNonce": 578577022,
			"isDeleted": false,
			"id": "6OoWBtQMrt0OIkzwoNvEb",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 694.9583587646482,
			"y": -80.6912689208985,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 103.33335876464841,
			"height": 37.5,
			"seed": 598028606,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694170281785,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 161,
			"versionNonce": 972914366,
			"isDeleted": false,
			"id": "iU53XysGs5bbef7Kv3Bku",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 887.8749999999998,
			"y": -83.60791015625006,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 103.33335876464841,
			"height": 37.5,
			"seed": 784811006,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694170130261,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 496,
			"versionNonce": 358486910,
			"isDeleted": false,
			"id": "8D3yDq4w",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 628.1943511962888,
			"y": -284.28045181783943,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 110.33467102050781,
			"height": 17.178365793881806,
			"seed": 1042035618,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694170165185,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "13.下载程序资源",
			"rawText": "13.下载程序资源",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "13.下载程序资源",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 568,
			"versionNonce": 1939704766,
			"isDeleted": false,
			"id": "NdAnnvKk",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 691.8025436401365,
			"y": -34.673364652527994,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 127.14498901367188,
			"height": 12.96419146325904,
			"seed": 183231778,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694170213883,
			"link": null,
			"locked": false,
			"fontSize": 11.27320996805134,
			"fontFamily": 2,
			"text": "job.split、job.xml、job.jar",
			"rawText": "job.split、job.xml、job.jar",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "job.split、job.xml、job.jar",
			"lineHeight": 1.15,
			"baseline": 10
		},
		{
			"type": "text",
			"version": 549,
			"versionNonce": 791075838,
			"isDeleted": false,
			"id": "6rrgCrvl",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 885.5525436401365,
			"y": -36.340005887879556,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 127.14498901367188,
			"height": 12.96419146325904,
			"seed": 1369902718,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694170205924,
			"link": null,
			"locked": false,
			"fontSize": 11.27320996805134,
			"fontFamily": 2,
			"text": "job.split、job.xml、job.jar",
			"rawText": "job.split、job.xml、job.jar",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "job.split、job.xml、job.jar",
			"lineHeight": 1.15,
			"baseline": 10
		},
		{
			"type": "text",
			"version": 597,
			"versionNonce": 996748926,
			"isDeleted": false,
			"id": "HtlA3KOG",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 703.1713790893552,
			"y": -72.68738682657875,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 75.65910339355469,
			"height": 27.25132534823762,
			"seed": 1348122274,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694170486558,
			"link": null,
			"locked": false,
			"fontSize": 11.848402325320706,
			"fontFamily": 2,
			"text": "      yarn child\n      (map task)",
			"rawText": "      yarn child\n      (map task)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "      yarn child\n      (map task)",
			"lineHeight": 1.15,
			"baseline": 24
		},
		{
			"type": "text",
			"version": 620,
			"versionNonce": 1021930046,
			"isDeleted": false,
			"id": "M3U6F4CY",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 891.2739334106445,
			"y": -75.98361097734153,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 75.65910339355469,
			"height": 27.25132534823762,
			"seed": 1901107006,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694170492845,
			"link": null,
			"locked": false,
			"fontSize": 11.848402325320706,
			"fontFamily": 2,
			"text": "      yarn child\n      (map task)",
			"rawText": "      yarn child\n      (map task)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "      yarn child\n      (map task)",
			"lineHeight": 1.15,
			"baseline": 24
		},
		{
			"type": "arrow",
			"version": 27,
			"versionNonce": 1302356222,
			"isDeleted": false,
			"id": "dLlpbfEKxzSZiugStlp7X",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 592.8749999999998,
			"y": 5.14208984375,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 23.333358764648438,
			"height": 20.83332061767578,
			"seed": 318599806,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694170540590,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "nxBT4Uh4sowBfD1g-YbG8",
				"focus": 0.39881231691815433,
				"gap": 2.0834732055664062
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
					23.333358764648438,
					20.83332061767578
				]
			]
		},
		{
			"type": "line",
			"version": 148,
			"versionNonce": 329387042,
			"isDeleted": false,
			"id": "qARcuY4eBmiR_RFvfo-xm",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 606.2083587646482,
			"y": 29.30876922607422,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 388.33335876464866,
			"height": 0.8333587646484375,
			"seed": 1508089506,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694170532591,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					388.33335876464866,
					-0.8333587646484375
				]
			]
		},
		{
			"type": "arrow",
			"version": 33,
			"versionNonce": 433724770,
			"isDeleted": false,
			"id": "lx-XqP4jD241x3twBqNRu",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 701.2083587646482,
			"y": 26.808731079101562,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 1.6666412353515625,
			"height": 25.000000000000057,
			"seed": 551467390,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694170561991,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "hS_2rDcToNuSxtVNX2-4W",
				"focus": -0.4530412202882626,
				"gap": 23.75011444091797
			},
			"endBinding": {
				"elementId": "EIMN4dmSgd1xE56t68-Hi",
				"focus": 0.6345996218628518,
				"gap": 15.000038146972656
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
					1.6666412353515625,
					-25.000000000000057
				]
			]
		},
		{
			"type": "text",
			"version": 656,
			"versionNonce": 305627518,
			"isDeleted": false,
			"id": "g7re5wUV",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 754.4442367553711,
			"y": 39.469586329133335,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 275.453857421875,
			"height": 17.178365793881806,
			"seed": 886529122,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "xsTdoHa500x3CQT8C4I8f",
					"type": "arrow"
				}
			],
			"updated": 1694170631638,
			"link": null,
			"locked": false,
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"text": "14.发送shell命令启动yarn child(map task)",
			"rawText": "14.发送shell命令启动yarn child(map task)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "14.发送shell命令启动yarn child(map task)",
			"lineHeight": 1.15,
			"baseline": 14
		},
		{
			"type": "arrow",
			"version": 30,
			"versionNonce": 1880743266,
			"isDeleted": false,
			"id": "xsTdoHa500x3CQT8C4I8f",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 898.7083587646482,
			"y": 27.64208984375,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 0.8333587646484375,
			"height": 27.5,
			"seed": 1115383074,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694170631638,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "g7re5wUV",
				"focus": 0.05185914823447863,
				"gap": 11.827496485383335
			},
			"endBinding": {
				"elementId": "cbs3SWN66m2yVzh7sXAQF",
				"focus": 0.5868709642665253,
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
					-0.8333587646484375,
					-27.5
				]
			]
		},
		{
			"type": "text",
			"version": 1340,
			"versionNonce": 863367394,
			"isDeleted": false,
			"id": "7mDhJ7g6",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 454.7528898269736,
			"y": 201.72432210239356,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 637.2637939453125,
			"height": 189.03851202889155,
			"seed": 594247842,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694171753060,
			"link": null,
			"locked": false,
			"fontSize": 16.43813148077318,
			"fontFamily": 2,
			"text": "14步之后，一旦有maptask处理完成，MRAppmaster就会请求若干容器\n启动若干个yarnchild(reduce task)，这些reduce task进程启动后就会去\n已完成任务的maptask端下载属于自己的分区数据；\n\n当所有maptask都完成任务时，各reduce task也就下载全了属于自己的\n分区数据，接下来就是合并数据、分组聚合（调reduce方法）、输出结果；\n\n当所有的reduce task进程也完成任务后，MRAppMaster也会向ResourceManager注销；\n\n当MRAppMaster退出后，客户端也就运行完毕、退出；",
			"rawText": "14步之后，一旦有maptask处理完成，MRAppmaster就会请求若干容器\n启动若干个yarnchild(reduce task)，这些reduce task进程启动后就会去\n已完成任务的maptask端下载属于自己的分区数据；\n\n当所有maptask都完成任务时，各reduce task也就下载全了属于自己的\n分区数据，接下来就是合并数据、分组聚合（调reduce方法）、输出结果；\n\n当所有的reduce task进程也完成任务后，MRAppMaster也会向ResourceManager注销；\n\n当MRAppMaster退出后，客户端也就运行完毕、退出；",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "14步之后，一旦有maptask处理完成，MRAppmaster就会请求若干容器\n启动若干个yarnchild(reduce task)，这些reduce task进程启动后就会去\n已完成任务的maptask端下载属于自己的分区数据；\n\n当所有maptask都完成任务时，各reduce task也就下载全了属于自己的\n分区数据，接下来就是合并数据、分组聚合（调reduce方法）、输出结果；\n\n当所有的reduce task进程也完成任务后，MRAppMaster也会向ResourceManager注销；\n\n当MRAppMaster退出后，客户端也就运行完毕、退出；",
			"lineHeight": 1.15,
			"baseline": 185
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#f08c00",
		"currentItemBackgroundColor": "#ffec99",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 0.5,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 2,
		"currentItemFontSize": 20,
		"currentItemTextAlign": "center",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 278.6080575422805,
		"scrollY": 387.5963122039112,
		"zoom": {
			"value": 0.7000000000000002
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