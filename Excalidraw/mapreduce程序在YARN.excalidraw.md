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
			"id": "0yVnRVjO",
			"type": "text",
			"x": -484.53919534122247,
			"y": -180.42816848667812,
			"width": 386.796875,
			"height": 138,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1234129150,
			"version": 414,
			"versionNonce": 1567421666,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694171738007,
			"link": null,
			"locked": false,
			"text": "hadoop的mapreduce框架：分布式运算程序\n核心角色：\n1、MRAppMaster\n2、yarnchild(map task)\n3、yarnchild(reduce task)\n4、提交job的客户端",
			"rawText": "hadoop的mapreduce框架：分布式运算程序\n核心角色：\n1、MRAppMaster\n2、yarnchild(map task)\n3、yarnchild(reduce task)\n4、提交job的客户端",
			"fontSize": 20,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 133,
			"containerId": null,
			"originalText": "hadoop的mapreduce框架：分布式运算程序\n核心角色：\n1、MRAppMaster\n2、yarnchild(map task)\n3、yarnchild(reduce task)\n4、提交job的客户端",
			"lineHeight": 1.15
		},
		{
			"id": "ystbSXa64tiBut4nfN9SI",
			"type": "rectangle",
			"x": -51.166664123535156,
			"y": -156.1197738647461,
			"width": 149.5,
			"height": 187.50000762939462,
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
			"roundness": {
				"type": 3
			},
			"seed": 1297175486,
			"version": 195,
			"versionNonce": 1452573950,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614180,
			"link": null,
			"locked": false
		},
		{
			"id": "uXsfYjkCT_EDQRHpEDb4_",
			"type": "rectangle",
			"x": 294.8334503173828,
			"y": -183.95314025878906,
			"width": 736.666625976563,
			"height": 222.66668701171875,
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
			"roundness": {
				"type": 3
			},
			"seed": 2068243490,
			"version": 201,
			"versionNonce": 1836756670,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170487710,
			"link": null,
			"locked": false
		},
		{
			"id": "DK43rJogELzfRqUt4SC2X",
			"type": "rectangle",
			"x": -37.12500000000006,
			"y": 240.9752960205078,
			"width": 159.16664123535162,
			"height": 160.0000000000001,
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
			"roundness": {
				"type": 3
			},
			"seed": 343280510,
			"version": 54,
			"versionNonce": 1710699838,
			"isDeleted": false,
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
			"id": "ykazoUFu",
			"type": "text",
			"x": -12.125038146972685,
			"y": -196.52474212646493,
			"width": 43.5546875,
			"height": 18.4,
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
			"seed": 1030386046,
			"version": 69,
			"versionNonce": 3163554,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614180,
			"link": null,
			"locked": false,
			"text": "HDFS",
			"rawText": "HDFS",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "HDFS",
			"lineHeight": 1.15
		},
		{
			"id": "IMyD51QE",
			"type": "text",
			"x": 18.708358764648438,
			"y": 212.64193725585938,
			"width": 48,
			"height": 18.4,
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
			"seed": 422913634,
			"version": 21,
			"versionNonce": 236576126,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614180,
			"link": null,
			"locked": false,
			"text": "客户端",
			"rawText": "客户端",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "客户端",
			"lineHeight": 1.15
		},
		{
			"id": "WtnYftnHF2_kqwHWmjPKf",
			"type": "rectangle",
			"x": 324.5417175292969,
			"y": -135.77470397949222,
			"width": 145,
			"height": 141,
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
			"roundness": {
				"type": 3
			},
			"seed": 307535870,
			"version": 82,
			"versionNonce": 1595621730,
			"isDeleted": false,
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
			"id": "7Wd6KZSm",
			"type": "text",
			"x": 334.54164123535156,
			"y": -163.19138336181646,
			"width": 129.84375,
			"height": 18.4,
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
			"seed": 1189989730,
			"version": 39,
			"versionNonce": 608009662,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614180,
			"link": null,
			"locked": false,
			"text": "resource manager",
			"rawText": "resource manager",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "resource manager",
			"lineHeight": 1.15
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
			"baseline": 15
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
			"baseline": 15
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
			"baseline": 15
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
			"baseline": 15
		},
		{
			"id": "1RpmUTU7",
			"type": "text",
			"x": -31.291641235351733,
			"y": 260.1420135498047,
			"width": 262.9296875,
			"height": 73.6,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1942031166,
			"version": 152,
			"versionNonce": 1367977662,
			"isDeleted": false,
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
			"text": "hadoop jar wc.har xx.yy.Jobsubmitter\n//job 参数设置\n\njob.submit()",
			"rawText": "hadoop jar wc.har xx.yy.Jobsubmitter\n//job 参数设置\n\njob.submit()",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 70,
			"containerId": null,
			"originalText": "hadoop jar wc.har xx.yy.Jobsubmitter\n//job 参数设置\n\njob.submit()",
			"lineHeight": 1.15
		},
		{
			"id": "qjAUIhFw",
			"type": "text",
			"x": -27.125000000000114,
			"y": 365.14208984375,
			"width": 78.2421875,
			"height": 18.4,
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
			"seed": 1578036798,
			"version": 41,
			"versionNonce": 675030050,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "5zWmqM1nJ0hvdhzF2smLC",
					"type": "arrow"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"text": "/root/wc.jar",
			"rawText": "/root/wc.jar",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "/root/wc.jar",
			"lineHeight": 1.15
		},
		{
			"id": "NGxe4eO8UYpEvOLFp-LMO",
			"type": "arrow",
			"x": 108.70828247070307,
			"y": 247.6420135498047,
			"width": 214.16474538384426,
			"height": 313.3302999059783,
			"angle": 0,
			"strokeColor": "#e03131",
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
			"seed": 947857150,
			"version": 535,
			"versionNonce": 606152446,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
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
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "tqjmZTUV",
			"type": "text",
			"x": 23.708435058593693,
			"y": 156.8086929321289,
			"width": 199.10540771484375,
			"height": 34.35673158776361,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1180758782,
			"version": 194,
			"versionNonce": 120016866,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"text": "1.请求resourcemanager，\n需要一个Container（带参数）",
			"rawText": "1.请求resourcemanager，\n需要一个Container（带参数）",
			"fontSize": 14.937709385984181,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 31,
			"containerId": null,
			"originalText": "1.请求resourcemanager，\n需要一个Container（带参数）",
			"lineHeight": 1.15
		},
		{
			"id": "ov6Fl-n8cwTr7dc_PKKBV",
			"type": "arrow",
			"x": 327.8987150854057,
			"y": -8.121830236717301,
			"width": 202.42839807005862,
			"height": 276.6208841789725,
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
			"roundness": {
				"type": 2
			},
			"seed": 619840546,
			"version": 219,
			"versionNonce": 1570740030,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-202.42839807005862,
					276.6208841789725
				]
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"id": "jqvsqm8e",
			"type": "text",
			"x": -37.125000000000114,
			"y": -129.8579483032227,
			"width": 186.765625,
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
			"seed": 1356405822,
			"version": 51,
			"versionNonce": 1986396030,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "fI16n8mhYse-iGxCj9ba7",
					"type": "arrow"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"text": "/wordcount/input/a.txt b.txt",
			"rawText": "/wordcount/input/a.txt b.txt",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "/wordcount/input/a.txt b.txt",
			"lineHeight": 1.15
		},
		{
			"id": "R8GXuSCi",
			"type": "text",
			"x": -43.79160308837902,
			"y": -64.02458953857428,
			"width": 215.21875,
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
			"seed": 1272015038,
			"version": 31,
			"versionNonce": 332337470,
			"isDeleted": false,
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
			"text": "/tmp/root/yarn/.../.staging/jobid",
			"rawText": "/tmp/root/yarn/.../.staging/jobid",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "/tmp/root/yarn/.../.staging/jobid",
			"lineHeight": 1.15
		},
		{
			"id": "RUP1a9PIiVxbQ1DugtTsi",
			"type": "rectangle",
			"x": 3.5417175292968466,
			"y": -43.191268920898494,
			"width": 75,
			"height": 66,
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
			"roundness": {
				"type": 3
			},
			"seed": 487649250,
			"version": 171,
			"versionNonce": 850747326,
			"isDeleted": false,
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
			"id": "Lf8YGP1t",
			"type": "text",
			"x": 13.916717529296847,
			"y": -37.79126892089849,
			"width": 54.25,
			"height": 55.199999999999996,
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
			"seed": 98612414,
			"version": 65,
			"versionNonce": 94372642,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"text": "job.split\njob.xml\njob.jar",
			"rawText": "job.split\njob.xml\njob.jar",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 51,
			"containerId": "RUP1a9PIiVxbQ1DugtTsi",
			"originalText": "job.split\njob.xml\njob.jar",
			"lineHeight": 1.15
		},
		{
			"id": "fI16n8mhYse-iGxCj9ba7",
			"type": "arrow",
			"x": -32.1249237060548,
			"y": 334.3088073730469,
			"width": 265.00000000000006,
			"height": 457.50000000000006,
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
			"roundness": {
				"type": 2
			},
			"seed": 1241629118,
			"version": 154,
			"versionNonce": 648984574,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
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
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"id": "qf5j6GsOT20h2s8KLH05j",
			"type": "arrow",
			"x": -252.9582824707033,
			"y": 102.64212799072266,
			"width": 263.3333587646485,
			"height": 135.00000000000006,
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
			"roundness": {
				"type": 2
			},
			"seed": 1101092478,
			"version": 80,
			"versionNonce": 978520126,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					263.3333587646485,
					-135.00000000000006
				]
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "31ghpInagemxceao4_req",
			"type": "arrow",
			"x": -14.624923706054801,
			"y": 278.47544860839844,
			"width": 147.4999237060547,
			"height": 290.00000000000006,
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
			"roundness": {
				"type": 2
			},
			"seed": 2053390526,
			"version": 166,
			"versionNonce": 242164386,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
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
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"id": "5zWmqM1nJ0hvdhzF2smLC",
			"type": "arrow",
			"x": 13.708358764648324,
			"y": 362.6421661376953,
			"width": 11.666717529296875,
			"height": 341.6667175292969,
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
			"roundness": {
				"type": 2
			},
			"seed": 976156898,
			"version": 47,
			"versionNonce": 1534492258,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					11.666717529296875,
					-341.6667175292969
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "qjAUIhFw",
				"focus": 0.03328884762350712,
				"gap": 2.4999237060546875
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"baseline": 13
		},
		{
			"id": "Xmjqwai20ehxG9EOF2dsU",
			"type": "arrow",
			"x": 132.0535501311877,
			"y": 306.79244437940395,
			"width": 218.42751280821415,
			"height": 300.64928572541027,
			"angle": 0,
			"strokeColor": "#e03131",
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
			"seed": 271559102,
			"version": 136,
			"versionNonce": 245129762,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					218.42751280821415,
					-300.64928572541027
				]
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"id": "OYCTC9fyw_dHDwr9dIFYF",
			"type": "rectangle",
			"x": 337.04164123535145,
			"y": -46.52451324462896,
			"width": 127.5,
			"height": 39.999961853027344,
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
			"seed": 403097058,
			"version": 104,
			"versionNonce": 1787283746,
			"isDeleted": false,
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
			"id": "drEeIyTJ0fF7NPU62ZiEF",
			"type": "ellipse",
			"x": 368.45843505859364,
			"y": -46.52455139160162,
			"width": 61,
			"height": 41,
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
			"seed": 46330658,
			"version": 239,
			"versionNonce": 1027995966,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "qArow0u8"
				}
			],
			"updated": 1694169614181,
			"link": null,
			"locked": false
		},
		{
			"id": "qArow0u8",
			"type": "text",
			"x": 384.21980323240393,
			"y": -35.22024040592585,
			"width": 29.34375,
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
			"seed": 1328626402,
			"version": 199,
			"versionNonce": 488620450,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"text": "task",
			"rawText": "task",
			"fontSize": 16,
			"fontFamily": 2,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "drEeIyTJ0fF7NPU62ZiEF",
			"originalText": "task",
			"lineHeight": 1.15
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
			"id": "4NzzAGz4cPP0B5Nl4vjIy",
			"type": "rectangle",
			"x": 515.3749999999999,
			"y": -97.3578720092774,
			"width": 120,
			"height": 78.33335876464842,
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
			"roundness": {
				"type": 3
			},
			"seed": 611257954,
			"version": 80,
			"versionNonce": 683359586,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false
		},
		{
			"id": "I8-t6VDvIkNBzzu4cvXId",
			"type": "arrow",
			"x": 395.3749999999998,
			"y": -47.35794830322271,
			"width": 127.5,
			"height": 47.499961853027344,
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
			"roundness": {
				"type": 2
			},
			"seed": 39405986,
			"version": 93,
			"versionNonce": 1101742526,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
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
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "wMDzOFPK",
				"focus": -0.37042531176876464,
				"gap": 2.7224272206713813
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"baseline": 13
		},
		{
			"id": "4VBU3myoVBlWIiDd1ssG5",
			"type": "rectangle",
			"x": 522.0417175292966,
			"y": -78.19123077392584,
			"width": 103.33335876464841,
			"height": 37.5,
			"angle": 0,
			"strokeColor": "#e03131",
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
			"seed": 2094762686,
			"version": 124,
			"versionNonce": 1698709054,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169646412,
			"link": null,
			"locked": false
		},
		{
			"id": "wHcjuohk_ORoX0Scoj5v5",
			"type": "arrow",
			"x": 42.04164123535128,
			"y": -47.35794830322271,
			"width": 500.8333587646486,
			"height": 202.50000000000003,
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
			"roundness": {
				"type": 2
			},
			"seed": 1807124414,
			"version": 230,
			"versionNonce": 221715554,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170670438,
			"link": null,
			"locked": false,
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
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"baseline": 13
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
			"id": "BGCbFbJS9Inb_e3OrmXIA",
			"type": "arrow",
			"x": 125.37496185302706,
			"y": 373.4754104614258,
			"width": 404.1666793823243,
			"height": 370.83335876464844,
			"angle": 0,
			"strokeColor": "#e03131",
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
			"seed": 372339838,
			"version": 101,
			"versionNonce": 975731810,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169614181,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					404.1666793823243,
					-370.83335876464844
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "DK43rJogELzfRqUt4SC2X",
				"focus": 0.8402723975846983,
				"gap": 3.333320617675497
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"id": "CVGJRfYbm0bTlB7we9rwD",
			"type": "arrow",
			"x": 576.2083587646482,
			"y": -39.02462768554693,
			"width": 113.33335876464844,
			"height": 105.00000000000006,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 282795362,
			"version": 162,
			"versionNonce": 2463970,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169760353,
			"link": null,
			"locked": false,
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
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"id": "4Ga8Py4WeIDQHY8vDYSFh",
			"type": "line",
			"x": 420.3749999999998,
			"y": 122.64205169677734,
			"width": 598.3333587646486,
			"height": 0.8333206176757812,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1838507774,
			"version": 180,
			"versionNonce": 2073225570,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169871465,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					598.3333587646486,
					0.8333206176757812
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": null
		},
		{
			"id": "186LRxW0X18DJ_ifcTf_M",
			"type": "arrow",
			"x": 750.3749999999998,
			"y": 118.47537231445312,
			"width": 0.8409290842660084,
			"height": 113.00003051757818,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 1710452002,
			"version": 166,
			"versionNonce": 562783486,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170178400,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.8409290842660084,
					-113.00003051757818
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "EIMN4dmSgd1xE56t68-Hi",
				"focus": -0.11718636132599182,
				"gap": 18.666648864746094
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "XZiqv-hljh7HozLSKkteq",
			"type": "arrow",
			"x": 944.5417175292968,
			"y": 121.80873107910156,
			"width": 0,
			"height": 116.66667938232428,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 678112446,
			"version": 54,
			"versionNonce": 1813913506,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169889304,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					-116.66667938232428
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "cbs3SWN66m2yVzh7sXAQF",
				"focus": -0.0689676219019381,
				"gap": 5.000038146972628
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"baseline": 13
		},
		{
			"id": "kNCP2B_lNpyK_Ec71l-RU",
			"type": "arrow",
			"x": 426.2083587646482,
			"y": 6.808731079101506,
			"width": 84.16664123535156,
			"height": 112.50000000000006,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 712207358,
			"version": 48,
			"versionNonce": 965628414,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169957825,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					84.16664123535156,
					112.50000000000006
				]
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "sO2KJqPdkDrBeyTEWNf8O",
			"type": "line",
			"x": 386.2084350585935,
			"y": -264.85800552368175,
			"width": 621.6665649414065,
			"height": 2.5,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1151166626,
			"version": 179,
			"versionNonce": 1404424738,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694169997095,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					621.6665649414065,
					2.5
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": null
		},
		{
			"id": "JMmLW8_FjOzFq12Z_P1Q9",
			"type": "arrow",
			"x": 65.37507629394503,
			"y": -33.191268920898494,
			"width": 391.6666412353516,
			"height": 234.16666030883795,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 513618338,
			"version": 64,
			"versionNonce": 559163810,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170009584,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					391.6666412353516,
					-234.16666030883795
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "R8GXuSCi",
				"focus": 0.3068454074081149,
				"gap": 12.43332061767578
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "8u0rS1ZyI22Iy5XK8fW-c",
			"type": "arrow",
			"x": 746.2083969116209,
			"y": -263.19126892089855,
			"width": 0,
			"height": 117.50000000000006,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 2049090914,
			"version": 30,
			"versionNonce": 1374016830,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170021486,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					117.50000000000006
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "sezPDZ4R",
				"focus": 0.1287228008095295,
				"gap": 6.633396911621091
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "yWnpHR9kkjRJLvZke8mC4",
			"type": "arrow",
			"x": 947.8750381469724,
			"y": -259.02458953857433,
			"width": 2.5,
			"height": 118.33332061767584,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 377135458,
			"version": 32,
			"versionNonce": 1925890430,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170026503,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					2.5,
					118.33332061767584
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "bWKYtmeF",
				"focus": 0.3059276525854258,
				"gap": 12.05007629394531
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "EIMN4dmSgd1xE56t68-Hi",
			"type": "rectangle",
			"x": 687.8750381469724,
			"y": -102.35794830322271,
			"width": 114.16664123535153,
			"height": 89.16664123535156,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 1045101054,
			"version": 117,
			"versionNonce": 1509564770,
			"isDeleted": false,
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
			"baseline": 13
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
			"baseline": 13
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
			"baseline": 13
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
			"id": "dLlpbfEKxzSZiugStlp7X",
			"type": "arrow",
			"x": 592.8749999999998,
			"y": 5.14208984375,
			"width": 23.333358764648438,
			"height": 20.83332061767578,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 318599806,
			"version": 27,
			"versionNonce": 1302356222,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170540590,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					23.333358764648438,
					20.83332061767578
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "nxBT4Uh4sowBfD1g-YbG8",
				"focus": 0.39881231691815433,
				"gap": 2.0834732055664062
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "qARcuY4eBmiR_RFvfo-xm",
			"type": "line",
			"x": 606.2083587646482,
			"y": 29.30876922607422,
			"width": 388.33335876464866,
			"height": 0.8333587646484375,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1508089506,
			"version": 148,
			"versionNonce": 329387042,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170532591,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					388.33335876464866,
					-0.8333587646484375
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": null
		},
		{
			"id": "lx-XqP4jD241x3twBqNRu",
			"type": "arrow",
			"x": 701.2083587646482,
			"y": 26.808731079101562,
			"width": 1.6666412353515625,
			"height": 25.000000000000057,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 551467390,
			"version": 33,
			"versionNonce": 433724770,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170561991,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					1.6666412353515625,
					-25.000000000000057
				]
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
			"baseline": 13
		},
		{
			"id": "xsTdoHa500x3CQT8C4I8f",
			"type": "arrow",
			"x": 898.7083587646482,
			"y": 27.64208984375,
			"width": 0.8333587646484375,
			"height": 27.5,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 1115383074,
			"version": 30,
			"versionNonce": 1880743266,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1694170631638,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-0.8333587646484375,
					-27.5
				]
			],
			"lastCommittedPoint": null,
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
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
		},
		{
			"id": "nTUio3-7iTPTYmK-BiKgp",
			"type": "freedraw",
			"x": 469.6067925927473,
			"y": 555.9798671294026,
			"width": 230,
			"height": 225.83335876464844,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1539802686,
			"version": 115,
			"versionNonce": 1933535742,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182924220,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					1.6666412353515625,
					-0.8333206176757812
				],
				[
					5,
					-2.5
				],
				[
					8.333358764648438,
					-5
				],
				[
					14.166641235351562,
					-7.5
				],
				[
					21.666641235351562,
					-9.166679382324219
				],
				[
					29.166641235351562,
					-11.666679382324219
				],
				[
					48.33335876464844,
					-13.333320617675781
				],
				[
					60.83335876464844,
					-13.333320617675781
				],
				[
					70.83335876464844,
					-13.333320617675781
				],
				[
					78.33335876464844,
					-12.5
				],
				[
					84.16664123535156,
					-10.833320617675781
				],
				[
					87.5,
					-8.333320617675781
				],
				[
					91.66664123535156,
					-5
				],
				[
					96.66664123535156,
					-1.6666793823242188
				],
				[
					100,
					2.5
				],
				[
					104.16664123535156,
					8.333320617675781
				],
				[
					109.16664123535156,
					16.66667938232422
				],
				[
					115,
					26.66667938232422
				],
				[
					122.5,
					42.5
				],
				[
					125.83335876464844,
					51.66667938232422
				],
				[
					131.66664123535156,
					66.66667938232422
				],
				[
					135,
					77.5
				],
				[
					138.33335876464844,
					88.33332061767578
				],
				[
					140.83335876464844,
					98.33332061767578
				],
				[
					142.5,
					107.50003814697266
				],
				[
					143.33335876464844,
					113.33332061767578
				],
				[
					143.33335876464844,
					120.00003814697266
				],
				[
					143.33335876464844,
					127.50003814697266
				],
				[
					143.33335876464844,
					134.16667938232422
				],
				[
					143.33335876464844,
					140.83332061767578
				],
				[
					143.33335876464844,
					150.83332061767578
				],
				[
					140.83335876464844,
					158.33332061767578
				],
				[
					137.5,
					166.66667938232422
				],
				[
					135,
					173.33332061767578
				],
				[
					131.66664123535156,
					177.50003814697266
				],
				[
					128.33335876464844,
					183.33332061767578
				],
				[
					122.5,
					190.83332061767578
				],
				[
					120.83335876464844,
					192.50003814697266
				],
				[
					117.5,
					195.83332061767578
				],
				[
					112.5,
					200.00003814697266
				],
				[
					107.5,
					202.50003814697266
				],
				[
					99.16664123535156,
					206.66667938232422
				],
				[
					90,
					210.83332061767578
				],
				[
					85,
					211.66667938232422
				],
				[
					79.16664123535156,
					212.50003814697266
				],
				[
					72.5,
					212.50003814697266
				],
				[
					65,
					212.50003814697266
				],
				[
					57.5,
					212.50003814697266
				],
				[
					50,
					212.50003814697266
				],
				[
					41.66664123535156,
					210.00003814697266
				],
				[
					35,
					208.33332061767578
				],
				[
					25.833358764648438,
					205.00003814697266
				],
				[
					10.833358764648438,
					200.00003814697266
				],
				[
					-1.6666412353515625,
					196.66667938232422
				],
				[
					-10,
					192.50003814697266
				],
				[
					-18.333358764648438,
					189.16667938232422
				],
				[
					-25.833358764648438,
					184.16667938232422
				],
				[
					-33.33335876464844,
					180.00003814697266
				],
				[
					-39.16664123535156,
					175.83332061767578
				],
				[
					-42.5,
					173.33332061767578
				],
				[
					-47.5,
					169.16667938232422
				],
				[
					-51.66664123535156,
					164.16667938232422
				],
				[
					-56.66664123535156,
					160.00003814697266
				],
				[
					-60.83335876464844,
					152.50003814697266
				],
				[
					-69.16664123535156,
					140.00003814697266
				],
				[
					-73.33335876464844,
					130.00003814697266
				],
				[
					-78.33335876464844,
					117.50003814697266
				],
				[
					-82.5,
					104.16667938232422
				],
				[
					-85,
					95
				],
				[
					-85.83335876464844,
					87.5
				],
				[
					-86.66664123535156,
					77.5
				],
				[
					-86.66664123535156,
					68.33332061767578
				],
				[
					-86.66664123535156,
					60
				],
				[
					-85.83335876464844,
					51.66667938232422
				],
				[
					-83.33335876464844,
					44.16667938232422
				],
				[
					-77.5,
					30
				],
				[
					-73.33335876464844,
					21.66667938232422
				],
				[
					-69.16664123535156,
					17.5
				],
				[
					-65.83335876464844,
					11.666679382324219
				],
				[
					-61.66664123535156,
					7.5
				],
				[
					-57.5,
					3.3333206176757812
				],
				[
					-52.5,
					0
				],
				[
					-48.33335876464844,
					-2.5
				],
				[
					-45,
					-4.166679382324219
				],
				[
					-42.5,
					-5
				],
				[
					-40,
					-5.833320617675781
				],
				[
					-38.33335876464844,
					-6.666679382324219
				],
				[
					-37.5,
					-6.666679382324219
				],
				[
					-35,
					-7.5
				],
				[
					-34.16664123535156,
					-7.5
				],
				[
					-33.33335876464844,
					-7.5
				],
				[
					-32.5,
					-7.5
				],
				[
					-30.833358764648438,
					-7.5
				],
				[
					-29.166641235351562,
					-7.5
				],
				[
					-28.333358764648438,
					-7.5
				],
				[
					-26.666641235351562,
					-7.5
				],
				[
					-20.833358764648438,
					-6.666679382324219
				],
				[
					-16.666641235351562,
					-6.666679382324219
				],
				[
					-10.833358764648438,
					-6.666679382324219
				],
				[
					-3.3333587646484375,
					-6.666679382324219
				],
				[
					5.8333587646484375,
					-6.666679382324219
				],
				[
					15,
					-6.666679382324219
				],
				[
					21.666641235351562,
					-6.666679382324219
				],
				[
					23.333358764648438,
					-6.666679382324219
				],
				[
					25.833358764648438,
					-6.666679382324219
				],
				[
					28.333358764648438,
					-5.833320617675781
				],
				[
					30,
					-5.833320617675781
				],
				[
					30.833358764648438,
					-5.833320617675781
				],
				[
					32.5,
					-5.833320617675781
				],
				[
					33.33335876464844,
					-5.833320617675781
				],
				[
					33.33335876464844,
					-5.833320617675781
				]
			],
			"pressures": [],
			"simulatePressure": true,
			"lastCommittedPoint": [
				33.33335876464844,
				-5.833320617675781
			]
		},
		{
			"id": "2C1AhNDPJKt46bIq397Vb",
			"type": "ellipse",
			"x": 689.6067925927473,
			"y": 516.8131877470784,
			"width": 265.0000762939454,
			"height": 255,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 2058991266,
			"version": 62,
			"versionNonce": 1596418878,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182928082,
			"link": null,
			"locked": false
		},
		{
			"id": "DbPRn6YA3qSpHEYuR7hbK",
			"type": "ellipse",
			"x": 748.773433828099,
			"y": 570.1465083647541,
			"width": 149.99999999999997,
			"height": 160.8333206176758,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 2113965858,
			"version": 110,
			"versionNonce": 432435106,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182928082,
			"link": null,
			"locked": false
		},
		{
			"id": "Pynj8_AdFqwsNq4r8GJry",
			"type": "freedraw",
			"x": 846.273433828099,
			"y": 573.4798671294026,
			"width": 27.5,
			"height": 44.16667938232422,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 448347490,
			"version": 33,
			"versionNonce": 533943166,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182928082,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					-0.8333206176757812
				],
				[
					0.8333587646484375,
					-1.6666793823242188
				],
				[
					1.666717529296875,
					-3.3333206176757812
				],
				[
					2.5,
					-5.833320617675781
				],
				[
					5,
					-9.166679382324219
				],
				[
					6.666717529296875,
					-12.5
				],
				[
					8.333358764648438,
					-15
				],
				[
					10.833358764648438,
					-18.33332061767578
				],
				[
					13.333358764648438,
					-21.66667938232422
				],
				[
					16.666717529296875,
					-26.66667938232422
				],
				[
					16.666717529296875,
					-28.33332061767578
				],
				[
					18.333358764648438,
					-29.16667938232422
				],
				[
					19.166717529296875,
					-30.83332061767578
				],
				[
					20,
					-31.66667938232422
				],
				[
					20.833358764648438,
					-32.5
				],
				[
					20.833358764648438,
					-33.33332061767578
				],
				[
					20.833358764648438,
					-34.16667938232422
				],
				[
					21.666717529296875,
					-35
				],
				[
					21.666717529296875,
					-35.83332061767578
				],
				[
					22.5,
					-37.5
				],
				[
					23.333358764648438,
					-39.16667938232422
				],
				[
					25,
					-40
				],
				[
					25,
					-40.83332061767578
				],
				[
					25,
					-41.66667938232422
				],
				[
					25.833358764648438,
					-42.5
				],
				[
					26.666717529296875,
					-43.33332061767578
				],
				[
					26.666717529296875,
					-44.16667938232422
				],
				[
					27.5,
					-44.16667938232422
				],
				[
					27.5,
					-44.16667938232422
				]
			],
			"pressures": [],
			"simulatePressure": true,
			"lastCommittedPoint": [
				27.5,
				-44.16667938232422
			]
		},
		{
			"id": "7Av4apbR6Vdmg13VFQRBs",
			"type": "freedraw",
			"x": 893.773433828099,
			"y": 544.3131877470784,
			"width": 5.833282470703125,
			"height": 38.33335876464844,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 769680766,
			"version": 16,
			"versionNonce": 2019306338,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182928082,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					0.8333587646484375
				],
				[
					0,
					2.5
				],
				[
					0,
					7.5
				],
				[
					0,
					13.333358764648438
				],
				[
					-1.6666412353515625,
					22.5
				],
				[
					-4.1666412353515625,
					29.16667938232422
				],
				[
					-5,
					33.33335876464844
				],
				[
					-5.833282470703125,
					36.66667938232422
				],
				[
					-5.833282470703125,
					37.5
				],
				[
					-5.833282470703125,
					38.33335876464844
				],
				[
					-5.833282470703125,
					37.5
				],
				[
					-5.833282470703125,
					37.5
				]
			],
			"pressures": [],
			"simulatePressure": true,
			"lastCommittedPoint": [
				-5.833282470703125,
				37.5
			]
		},
		{
			"id": "0MjtRC90VUlMM6DE-SLaJ",
			"type": "freedraw",
			"x": 907.9401513573958,
			"y": 574.3131877470784,
			"width": 10,
			"height": 25.833358764648438,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1066344254,
			"version": 16,
			"versionNonce": 1800942526,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182928082,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					2.5
				],
				[
					0,
					4.166679382324219
				],
				[
					0,
					10
				],
				[
					0,
					15.833358764648438
				],
				[
					0,
					20.833358764648438
				],
				[
					0,
					23.333358764648438
				],
				[
					0,
					25.833358764648438
				],
				[
					0,
					25
				],
				[
					0.833282470703125,
					24.16667938232422
				],
				[
					5,
					21.66667938232422
				],
				[
					10,
					17.5
				],
				[
					10,
					17.5
				]
			],
			"pressures": [],
			"simulatePressure": true,
			"lastCommittedPoint": [
				10,
				17.5
			]
		},
		{
			"id": "Sjy1NOxLvhzWVi1Zc-6hF",
			"type": "freedraw",
			"x": 927.1068688866927,
			"y": 587.6465465117268,
			"width": 8.333282470703125,
			"height": 44.16664123535156,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1571049726,
			"version": 21,
			"versionNonce": 1011117858,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182928082,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					2.5
				],
				[
					0.833282470703125,
					5
				],
				[
					0.833282470703125,
					12.5
				],
				[
					0.833282470703125,
					19.166641235351562
				],
				[
					0.833282470703125,
					24.166641235351562
				],
				[
					0.833282470703125,
					28.33332061767578
				],
				[
					0.833282470703125,
					30.83332061767578
				],
				[
					0.833282470703125,
					32.5
				],
				[
					0.833282470703125,
					35
				],
				[
					0.833282470703125,
					38.33332061767578
				],
				[
					0.833282470703125,
					41.66664123535156
				],
				[
					0.833282470703125,
					43.33332061767578
				],
				[
					1.66656494140625,
					44.16664123535156
				],
				[
					2.5,
					44.16664123535156
				],
				[
					6.66656494140625,
					41.66664123535156
				],
				[
					8.333282470703125,
					40.83332061767578
				],
				[
					8.333282470703125,
					40.83332061767578
				]
			],
			"pressures": [],
			"simulatePressure": true,
			"lastCommittedPoint": [
				8.333282470703125,
				40.83332061767578
			]
		},
		{
			"id": "x0YeJPGsWrXo3Phc5eP6Y",
			"type": "freedraw",
			"x": 949.6068688866927,
			"y": 628.4798671294026,
			"width": 27.5,
			"height": 45.83332061767578,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 2141681570,
			"version": 17,
			"versionNonce": 1809255422,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182928082,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-1.666717529296875,
					7.5
				],
				[
					-5.83343505859375,
					17.5
				],
				[
					-11.666717529296875,
					27.500038146972656
				],
				[
					-16.666717529296875,
					35.83332061767578
				],
				[
					-21.666717529296875,
					41.66667938232422
				],
				[
					-24.166717529296875,
					43.33332061767578
				],
				[
					-25,
					45.83332061767578
				],
				[
					-25.83343505859375,
					45.83332061767578
				],
				[
					-27.5,
					45.000038146972656
				],
				[
					-27.5,
					43.33332061767578
				],
				[
					-27.5,
					41.66667938232422
				],
				[
					-27.5,
					40.000038146972656
				],
				[
					-27.5,
					40.000038146972656
				]
			],
			"pressures": [],
			"simulatePressure": true,
			"lastCommittedPoint": [
				-27.5,
				40.000038146972656
			]
		},
		{
			"id": "bbreaTCp165tb5Zp4-W_8",
			"type": "freedraw",
			"x": 914.6068688866927,
			"y": 673.4799052763752,
			"width": 11.666717529296875,
			"height": 17.5,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 207897918,
			"version": 11,
			"versionNonce": 703478498,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182928082,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-3.33343505859375,
					3.333282470703125
				],
				[
					-5.83343505859375,
					8.333282470703125
				],
				[
					-8.33343505859375,
					11.666641235351562
				],
				[
					-10.000076293945312,
					15
				],
				[
					-10.83343505859375,
					16.666641235351562
				],
				[
					-11.666717529296875,
					17.5
				],
				[
					-11.666717529296875,
					17.5
				]
			],
			"pressures": [],
			"simulatePressure": true,
			"lastCommittedPoint": [
				-11.666717529296875,
				17.5
			]
		},
		{
			"id": "XNLzF41jljeUXCJka8cV0",
			"type": "freedraw",
			"x": 868.773433828099,
			"y": 716.8131877470784,
			"width": 21.666717529296875,
			"height": 33.33335876464844,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 20,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1894470818,
			"version": 30,
			"versionNonce": 1599134782,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1694182928082,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.8333587646484375,
					0
				],
				[
					0.8333587646484375,
					2.5
				],
				[
					2.5,
					5
				],
				[
					4.166717529296875,
					8.333358764648438
				],
				[
					7.5,
					13.333358764648438
				],
				[
					8.333358764648438,
					14.166717529296875
				],
				[
					10,
					16.666717529296875
				],
				[
					10.833358764648438,
					18.333358764648438
				],
				[
					11.666717529296875,
					19.166717529296875
				],
				[
					12.5,
					19.166717529296875
				],
				[
					13.333358764648438,
					20
				],
				[
					14.166717529296875,
					20.833358764648438
				],
				[
					15,
					21.666717529296875
				],
				[
					15,
					22.5
				],
				[
					15.833358764648438,
					23.333358764648438
				],
				[
					16.666717529296875,
					24.166717529296875
				],
				[
					17.5,
					25
				],
				[
					18.333358764648438,
					27.5
				],
				[
					19.166717529296875,
					28.333358764648438
				],
				[
					19.166717529296875,
					29.166717529296875
				],
				[
					20,
					30
				],
				[
					20.833358764648438,
					30.833358764648438
				],
				[
					20.833358764648438,
					31.666717529296875
				],
				[
					20.833358764648438,
					32.5
				],
				[
					21.666717529296875,
					33.33335876464844
				],
				[
					21.666717529296875,
					33.33335876464844
				]
			],
			"pressures": [],
			"simulatePressure": true,
			"lastCommittedPoint": [
				21.666717529296875,
				33.33335876464844
			]
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
		"currentItemFontSize": 20,
		"currentItemTextAlign": "center",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 34.35156617190128,
		"scrollY": -109.19600024707825,
		"zoom": {
			"value": 0.7999999999999998
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