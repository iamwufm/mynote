---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
mapreduce框架的工作机制

划分输入切片：
job客户端负责划分：
    扫描输入目录中的所有文件
    遍历每一个文件：
        按照128M规则划分范围 ^fqsigiMZ

FileSplit0:/wordcount/input/a.txt 0-128M
FileSplit1:/wordcount/input/a.txt 128-200M
FileSplit2:/wordcount/input/b.txt 0-128M
FileSplit3:/wordcount/input/b.txt 128-180M
..... ^meRO2zwu

job.split文件 ^sEZkbY98

arrayList ^fBh9pn74

TextInputFormat ^7y9jGIIo

map task（yarn child）0 ^IDEGFSdK

LineRecordReader
                    next()
产生一对kv
k：行起始偏移量 v：行内容 ^lpPB4QNI

反复调用 ^onQ2Vhhd

    /wordcount/input/a.txt     b.txt     c.txt
                               200M   180M  100M
 ^j0qMoWUx

0-128M ^JlXRj8KA

split0 切片范围 ^ELsZZeO1

WcMapper类
        map(k,v,context)
                context.write(k,v)
 ^vCAp3B3t

环形缓冲区：100M ^v1AfhcOg

MapOutputCollector ^jNy7B1Zj

kv ^rkwCvjqL

kv ^G5HheJ2U

kv ^0oCHACyb

kv ^7J8jnrO4

kv ^ONaHK26L

kv ^7b7qzrLk

kv ^UufpOxj7

kv ^qqKUtcfZ

kv ^9aXb5lAB

kv ^uAgXvaTy

分区：按Partitioner的getPartition()
排序：按key的compareTo ^rs6gGZGT

spiller ^PxyvinCz

a,1 a,1 d,1
 kv kv   kv   kv kv kv kv ^pfZk2ner

kv kv   kv   kv kv kv kv ^XCDT7cHE

溢出文件，本地磁盘 ^CinskEHp

区号小的在前面，同区中的按key有序 ^6PidAHLk

0号区 ^7DcEXl2R

kv kv kv kv  ^yKhCetOs

kv kv kv kv  ^GEbYcxIi

kv kv  ^FgCqCyfQ

1号区 ^KLjpsL9d

0号区 ^AuODT0gm

1号区 ^YjY8A8Nc

2号区 ^I7HiG0Mo

分区且有序 ^egwNsdPY

Merge合并 ^iP4BLjED

Combiner局部聚合 ^LJlciteX

分区索引文件 ^oT6v8B4N

纳入nodeManager 的web程序document目录中 ^Pbz2Z1lv

a,1 a,1 a,1 d,1 g,1
kv   kv   kv  kv  kv ^f3NwfHeW

a,1 a,1 g,1 g,1 g,1
kv   kv   kv  kv  kv ^UsamHWyk

0号区 ^qiFdVFTq

0号区 ^vP6Xmdhc

a,1 a,1 a,1 a,1 a,1 d,1 d,1 g,1 g,1 g,1 g,1 ^Lep36iK2

合并排序 ^IDQaPuWF

文件 ^kXkiey3m

WcReduce类
        reduce(k,迭代器,context)         
                    v.values
            context.write(k,v)
 ^GAtWD4i1

分组比较器GroupingComparator ^TUN9u1MD

每迭代一下 ^PzzOZLNU

TextOutputFormat ^NFZ7Ijxo

LineRecordWriter
                    write(k,v) ^JDQqEXoZ

/wordcount/output

    part-r-0000
        key \t value
        key \t value
        ....
    
    part-r-0001
        key \t value
        key \t value
        .... ^kCrqKbN9

reduce task（yarn child）0 ^rC6gjDZS

HDFS ^UUqNZP1A

http下载 ^iZqQmEgd

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
			"version": 374,
			"versionNonce": 774455548,
			"isDeleted": false,
			"id": "fqsigiMZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -213.58329264322919,
			"y": -117.40868479410805,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 173.72509765625,
			"height": 106.69314026012991,
			"seed": 535334562,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 13.2538062434944,
			"fontFamily": 2,
			"text": "mapreduce框架的工作机制\n\n划分输入切片：\njob客户端负责划分：\n    扫描输入目录中的所有文件\n    遍历每一个文件：\n        按照128M规则划分范围",
			"rawText": "mapreduce框架的工作机制\n\n划分输入切片：\njob客户端负责划分：\n    扫描输入目录中的所有文件\n    遍历每一个文件：\n        按照128M规则划分范围",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "mapreduce框架的工作机制\n\n划分输入切片：\njob客户端负责划分：\n    扫描输入目录中的所有文件\n    遍历每一个文件：\n        按照128M规则划分范围",
			"lineHeight": 1.15,
			"baseline": 103
		},
		{
			"type": "rectangle",
			"version": 146,
			"versionNonce": 348413124,
			"isDeleted": false,
			"id": "k92wDabQWyIjkb82zt5ZN",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -221.6944580078125,
			"y": 16.60246785481769,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 266,
			"height": 91.3333740234375,
			"seed": 473976638,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 546,
			"versionNonce": 1305461116,
			"isDeleted": false,
			"id": "meRO2zwu",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -202.3270263671875,
			"y": 28.026377671884475,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 227.85610961914062,
			"height": 70.18797693660288,
			"seed": 1672319202,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "2HdVgBH2mHl_sBl0i4ekr",
					"type": "arrow"
				},
				{
					"id": "Q77PK_Fpw-2WHTMuYjm2U",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "FileSplit0:/wordcount/input/a.txt 0-128M\nFileSplit1:/wordcount/input/a.txt 128-200M\nFileSplit2:/wordcount/input/b.txt 0-128M\nFileSplit3:/wordcount/input/b.txt 128-180M\n.....",
			"rawText": "FileSplit0:/wordcount/input/a.txt 0-128M\nFileSplit1:/wordcount/input/a.txt 128-200M\nFileSplit2:/wordcount/input/b.txt 0-128M\nFileSplit3:/wordcount/input/b.txt 128-180M\n.....",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "FileSplit0:/wordcount/input/a.txt 0-128M\nFileSplit1:/wordcount/input/a.txt 128-200M\nFileSplit2:/wordcount/input/b.txt 0-128M\nFileSplit3:/wordcount/input/b.txt 128-180M\n.....",
			"lineHeight": 1.15,
			"baseline": 67
		},
		{
			"type": "text",
			"version": 730,
			"versionNonce": 153363524,
			"isDeleted": false,
			"id": "sEZkbY98",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -220.95579020182302,
			"y": 177.73064735526634,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 65.76556396484375,
			"height": 14.037595387320575,
			"seed": 1404386,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "Q77PK_Fpw-2WHTMuYjm2U",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "job.split文件",
			"rawText": "job.split文件",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "job.split文件",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "text",
			"version": 698,
			"versionNonce": 634532348,
			"isDeleted": false,
			"id": "fBh9pn74",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -117.80513254801429,
			"y": 115.21968111894108,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 46.780487060546875,
			"height": 14.037595387320575,
			"seed": 836654716,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "arrayList",
			"rawText": "arrayList",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "arrayList",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "arrow",
			"version": 84,
			"versionNonce": 2099182532,
			"isDeleted": false,
			"id": "2HdVgBH2mHl_sBl0i4ekr",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -206.58901723225915,
			"y": -49.42816242275029,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 0,
			"height": 60.00000000000003,
			"seed": 425362756,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "meRO2zwu",
				"focus": -1.037409493844124,
				"gap": 17.454540094634737
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
					60.00000000000003
				]
			]
		},
		{
			"type": "arrow",
			"version": 319,
			"versionNonce": 624071292,
			"isDeleted": false,
			"id": "Q77PK_Fpw-2WHTMuYjm2U",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -207.10384755743468,
			"y": 108.49107189010024,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 0.1626607281110637,
			"height": 64.52039188860388,
			"seed": 261566020,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "meRO2zwu",
				"focus": 1.042284780011151,
				"gap": 11.332649238805345
			},
			"endBinding": {
				"elementId": "sEZkbY98",
				"focus": -0.5842800532875296,
				"gap": 4.71918357656223
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
					-0.1626607281110637,
					64.52039188860388
				]
			]
		},
		{
			"type": "text",
			"version": 816,
			"versionNonce": 1792205636,
			"isDeleted": false,
			"id": "7y9jGIIo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 101.91710662841825,
			"y": -72.55805427331148,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 88.14605712890625,
			"height": 14.037595387320575,
			"seed": 726286276,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "RItBoB69ugzC8_Cklj3SM",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "TextInputFormat",
			"rawText": "TextInputFormat",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "TextInputFormat",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "text",
			"version": 979,
			"versionNonce": 228514628,
			"isDeleted": false,
			"id": "IDEGFSdK",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 514.0350304167736,
			"y": -125.80884352538203,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 217.861328125,
			"height": 23,
			"seed": 358124740,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694315177045,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "map task（yarn child）0",
			"rawText": "map task（yarn child）0",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "map task（yarn child）0",
			"lineHeight": 1.15,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 69,
			"versionNonce": 1486740164,
			"isDeleted": false,
			"id": "lvxw5TUaxghcpBSymbPCe",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 97.29988861084007,
			"y": -75.5392820109663,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 95.55557250976568,
			"height": 21.111119588216155,
			"seed": 887554044,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "iJO30FjrQqtwuQlqslBim",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1154,
			"versionNonce": 1825907580,
			"isDeleted": false,
			"id": "lpPB4QNI",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 118.13364410400419,
			"y": -45.89141303795991,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 108.16766357421875,
			"height": 40.53113326166265,
			"seed": 967245052,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "iJO30FjrQqtwuQlqslBim",
					"type": "arrow"
				},
				{
					"id": "IrZW7rz3_E_6doiTWiLiW",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 8.811115926448403,
			"fontFamily": 2,
			"text": "LineRecordReader\n                    next()\n产生一对kv\nk：行起始偏移量 v：行内容",
			"rawText": "LineRecordReader\n                    next()\n产生一对kv\nk：行起始偏移量 v：行内容",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "LineRecordReader\n                    next()\n产生一对kv\nk：行起始偏移量 v：行内容",
			"lineHeight": 1.15,
			"baseline": 38
		},
		{
			"type": "text",
			"version": 1007,
			"versionNonce": 1360912964,
			"isDeleted": false,
			"id": "onQ2Vhhd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 199.2447891235355,
			"y": -35.613626783077066,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 32.1199951171875,
			"height": 9.645456657661024,
			"seed": 249559364,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "Hugup8uaqSRZdwh1F_X4V",
					"type": "arrow"
				},
				{
					"id": "VvHiKUhtyGtHEOGw1L9om",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 8.037880548050854,
			"fontFamily": 3,
			"text": "反复调用",
			"rawText": "反复调用",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "反复调用",
			"lineHeight": 1.2,
			"baseline": 7
		},
		{
			"type": "arrow",
			"version": 433,
			"versionNonce": 1104892924,
			"isDeleted": false,
			"id": "RItBoB69ugzC8_Cklj3SM",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 375.63320922851585,
			"y": -62.425880574308046,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 183.14436481198706,
			"height": 1.6957790453024941,
			"seed": 1908107004,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "7y9jGIIo",
				"gap": 2.425680659204289,
				"focus": 0.13290288624213106
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
					-183.14436481198706,
					-1.6957790453024941
				]
			]
		},
		{
			"type": "arrow",
			"version": 227,
			"versionNonce": 1529936324,
			"isDeleted": false,
			"id": "iJO30FjrQqtwuQlqslBim",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 183.1055045544087,
			"y": -56.65040159918242,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 0,
			"height": 7.105427357601002e-15,
			"seed": 792870396,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "lpPB4QNI",
				"focus": -0.2813590073832762,
				"gap": 10.758988561222523
			},
			"endBinding": {
				"elementId": "lvxw5TUaxghcpBSymbPCe",
				"focus": -0.03544383737332045,
				"gap": 10
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
					-7.105427357601002e-15
				]
			]
		},
		{
			"type": "text",
			"version": 737,
			"versionNonce": 1741124732,
			"isDeleted": false,
			"id": "j0qMoWUx",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 100.59402211507188,
			"y": -175.07772340081723,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 201.322998046875,
			"height": 39.7028238622449,
			"seed": 302564860,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 11.508064887607217,
			"fontFamily": 2,
			"text": "    /wordcount/input/a.txt     b.txt     c.txt\n                               200M   180M  100M\n",
			"rawText": "    /wordcount/input/a.txt     b.txt     c.txt\n                               200M   180M  100M\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "    /wordcount/input/a.txt     b.txt     c.txt\n                               200M   180M  100M\n",
			"lineHeight": 1.15,
			"baseline": 36
		},
		{
			"type": "line",
			"version": 112,
			"versionNonce": 1911890244,
			"isDeleted": false,
			"id": "Ta2ZojHstTVbqHxUWlu3E",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 106.18879445393901,
			"y": -143.87261534429967,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 206.6666666666668,
			"height": 1.1111195882161269,
			"seed": 190567620,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
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
					206.6666666666668,
					-1.1111195882161269
				]
			]
		},
		{
			"type": "text",
			"version": 961,
			"versionNonce": 744195324,
			"isDeleted": false,
			"id": "JlXRj8KA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 103.13369496663444,
			"y": -142.55809242028423,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 32.48211669921875,
			"height": 11.021104536228542,
			"seed": 1258807548,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 9.583569161937863,
			"fontFamily": 2,
			"text": "0-128M",
			"rawText": "0-128M",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0-128M",
			"lineHeight": 1.15,
			"baseline": 9
		},
		{
			"type": "text",
			"version": 948,
			"versionNonce": 2093504708,
			"isDeleted": false,
			"id": "ELsZZeO1",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 193.9670282999678,
			"y": -134.50253262617605,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 67.31317138671875,
			"height": 11.71742926442164,
			"seed": 2021149124,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 10.189068925584035,
			"fontFamily": 2,
			"text": "split0 切片范围",
			"rawText": "split0 切片范围",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "split0 切片范围",
			"lineHeight": 1.15,
			"baseline": 9
		},
		{
			"type": "arrow",
			"version": 323,
			"versionNonce": 1427735932,
			"isDeleted": false,
			"id": "Hugup8uaqSRZdwh1F_X4V",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 205.73288992746402,
			"y": -38.872615344299646,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 6.9559721733066056,
			"height": 79.44444020589191,
			"seed": 324920188,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "onQ2Vhhd",
				"focus": -0.6239877351064507,
				"gap": 3.25898856122258
			},
			"endBinding": {
				"elementId": "h02JGpCEXBSXsfzfTAhhG",
				"focus": 0.3761560675691191,
				"gap": 2.222226460774756
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
					6.9559721733066056,
					-79.44444020589191
				]
			]
		},
		{
			"type": "rectangle",
			"version": 64,
			"versionNonce": 1195941956,
			"isDeleted": false,
			"id": "h02JGpCEXBSXsfzfTAhhG",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 190.63322194417344,
			"y": -136.6504015991825,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 74.44442749023438,
			"height": 16.11111958821617,
			"seed": 230222532,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "Hugup8uaqSRZdwh1F_X4V",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false
		},
		{
			"type": "line",
			"version": 280,
			"versionNonce": 658732540,
			"isDeleted": false,
			"id": "FzxXfUHs5sbLqrtqSisYM",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -127.70016225179046,
			"y": 14.460717989033753,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 361.1110941569012,
			"height": 153.3333333333334,
			"seed": 1777926084,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
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
					-34.444427490234375,
					13.333333333333343
				],
				[
					326.6666666666668,
					-140.00000000000006
				]
			]
		},
		{
			"type": "line",
			"version": 100,
			"versionNonce": 2035546052,
			"isDeleted": false,
			"id": "wXAEHNU_4kEGeWB72-Rmi",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 162.29988861083984,
			"y": -24.42816242275012,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 35,
			"height": 0,
			"seed": 115125700,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
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
					35,
					0
				]
			]
		},
		{
			"type": "text",
			"version": 997,
			"versionNonce": 110291780,
			"isDeleted": false,
			"id": "vCAp3B3t",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 102.02249908447266,
			"y": 18.553014452274496,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 146.42947387695312,
			"height": 56.1503815492823,
			"seed": 1381247996,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "VvHiKUhtyGtHEOGw1L9om",
					"type": "arrow"
				}
			],
			"updated": 1694314107589,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "WcMapper类\n        map(k,v,context)\n                context.write(k,v)\n",
			"rawText": "WcMapper类\n        map(k,v,context)\n                context.write(k,v)\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "WcMapper类\n        map(k,v,context)\n                context.write(k,v)\n",
			"lineHeight": 1.15,
			"baseline": 53
		},
		{
			"type": "rectangle",
			"version": 72,
			"versionNonce": 252206916,
			"isDeleted": false,
			"id": "tOgomCUUktzQeM_x2XGww",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 96.1887435913086,
			"y": 13.905170910583223,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 160.55557250976574,
			"height": 53.8888804117839,
			"seed": 1989019332,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "9cJsHoruYhzk3LOcA-oAE",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 118,
			"versionNonce": 253551300,
			"isDeleted": false,
			"id": "VvHiKUhtyGtHEOGw1L9om",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 185.0776494344076,
			"y": -27.205948677632946,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 30.555572509765625,
			"height": 41.6666666666667,
			"seed": 1637030340,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694314107590,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "onQ2Vhhd",
				"focus": 1.4083095187996528,
				"gap": 14.167139689127907
			},
			"endBinding": {
				"elementId": "vCAp3B3t",
				"focus": -0.472314938379513,
				"gap": 4.092296463240743
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
					-30.555572509765625,
					41.6666666666667
				]
			]
		},
		{
			"type": "arrow",
			"version": 29,
			"versionNonce": 1355011780,
			"isDeleted": false,
			"id": "IrZW7rz3_E_6doiTWiLiW",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 152.85541025797534,
			"y": -52.76149575608346,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 11.111094156901004,
			"height": 6.666666666666686,
			"seed": 69659204,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "lpPB4QNI",
				"focus": -0.29470813151444364,
				"gap": 6.870082718123555
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
					11.111094156901004,
					6.666666666666686
				]
			]
		},
		{
			"type": "ellipse",
			"version": 172,
			"versionNonce": 1864165244,
			"isDeleted": false,
			"id": "IHde9w4x2fT9YD6xtOZZL",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 301.18879445393895,
			"y": -59.42816242275018,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 131.11109415690112,
			"height": 127.22221374511722,
			"seed": 29744836,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "dfszHKBT1fZbNRtD6angZ",
					"type": "arrow"
				},
				{
					"id": "9cJsHoruYhzk3LOcA-oAE",
					"type": "arrow"
				},
				{
					"id": "xC3lI5UzIytl2x454nhz0",
					"type": "arrow"
				},
				{
					"id": "8EumC7qzvC_yEFLer0mjd",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false
		},
		{
			"type": "ellipse",
			"version": 116,
			"versionNonce": 1260674628,
			"isDeleted": false,
			"id": "RQC47WJSthcKrAWBGYxXz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 321.74436696370446,
			"y": -36.09485452073193,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 81.66666666666674,
			"height": 82.22221374511722,
			"seed": 1635910724,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "9cJsHoruYhzk3LOcA-oAE",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 997,
			"versionNonce": 780834812,
			"isDeleted": false,
			"id": "v1AfhcOg",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 312.85593414306663,
			"y": -80.33586595950943,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 103.71781921386719,
			"height": 14.037595387320575,
			"seed": 1245747964,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "环形缓冲区：100M",
			"rawText": "环形缓冲区：100M",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "环形缓冲区：100M",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "text",
			"version": 1091,
			"versionNonce": 1191704004,
			"isDeleted": false,
			"id": "jNy7B1Zj",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 224.3299102783205,
			"y": -5.891438469275002,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 74.6126708984375,
			"height": 9.652906217550814,
			"seed": 551069308,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "BD4BtqLYvNF4NljPkkmEY",
					"type": "arrow"
				},
				{
					"id": "9cJsHoruYhzk3LOcA-oAE",
					"type": "arrow"
				}
			],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "MapOutputCollector",
			"rawText": "MapOutputCollector",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "MapOutputCollector",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "line",
			"version": 35,
			"versionNonce": 99498108,
			"isDeleted": false,
			"id": "1C04RpfQ7BOu-g2h20Tzs",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 320.07770029703784,
			"y": -38.872615344299646,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 12.777760823567746,
			"height": 13.888880411783873,
			"seed": 1801352004,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
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
					12.777760823567746,
					13.888880411783873
				]
			]
		},
		{
			"type": "line",
			"version": 37,
			"versionNonce": 1099734340,
			"isDeleted": false,
			"id": "0fVva67_3Ed31_AHa-Uad",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 305.07770029703784,
			"y": 22.238504243916537,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 20,
			"height": 4.444452921549498,
			"seed": 520351556,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
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
					20,
					-4.444452921549498
				]
			]
		},
		{
			"type": "arrow",
			"version": 285,
			"versionNonce": 1628031228,
			"isDeleted": false,
			"id": "dfszHKBT1fZbNRtD6angZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 302.8554611206056,
			"y": 51.12738465570038,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 118.33338419596373,
			"height": 22.222188313802008,
			"seed": 1592086212,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "IHde9w4x2fT9YD6xtOZZL",
				"focus": 1.058134897368083,
				"gap": 14.422977043953097
			},
			"endBinding": {
				"elementId": "IHde9w4x2fT9YD6xtOZZL",
				"focus": -1.0428415251490206,
				"gap": 12.631567091790984
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
					54.44447835286462,
					22.222188313802008
				],
				[
					118.33338419596373,
					7.777786254882869
				]
			]
		},
		{
			"type": "freedraw",
			"version": 12,
			"versionNonce": 1678791876,
			"isDeleted": false,
			"id": "CaSNtjGocYrXA2xK5JkFG",
			"fillStyle": "hachure",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 390.63327280680346,
			"y": -39.42816242275015,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 0.0001,
			"height": 0.0001,
			"seed": 379595644,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.0001,
					0.0001
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 11,
			"versionNonce": 1667189116,
			"isDeleted": false,
			"id": "k85NgzjWXF2OK-aBaCeNW",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 347.8554611206056,
			"y": -42.20594867763299,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 0.0001,
			"height": 0.0001,
			"seed": 1493263300,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.0001,
					0.0001
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 85,
			"versionNonce": 2083593284,
			"isDeleted": false,
			"id": "CVo-c6eN9SagRFYpN4Dvu",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 328.96660614013683,
			"y": -37.20594867763296,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 54.444427490234375,
			"height": 11.666666666666686,
			"seed": 533199612,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-0.555572509765625,
					0
				],
				[
					0,
					0
				],
				[
					0.5555216471353788,
					-1.1111195882161553
				],
				[
					1.1110941569010038,
					-1.6666666666666856
				],
				[
					2.2221883138021212,
					-1.6666666666666856
				],
				[
					3.333333333333371,
					-2.777786254882812
				],
				[
					3.88885498046875,
					-3.333333333333343
				],
				[
					5.55552164713538,
					-3.888880411783873
				],
				[
					6.666666666666629,
					-4.444452921549498
				],
				[
					7.777760823567746,
					-5.000000000000028
				],
				[
					8.333333333333371,
					-5.55554707845053
				],
				[
					10,
					-6.111119588216155
				],
				[
					10.555521647135379,
					-6.111119588216155
				],
				[
					11.666666666666629,
					-6.666666666666686
				],
				[
					12.777760823567746,
					-7.222213745117216
				],
				[
					14.444427490234375,
					-7.777786254882841
				],
				[
					15.555521647135379,
					-8.333333333333343
				],
				[
					16.66666666666663,
					-8.333333333333343
				],
				[
					17.22218831380212,
					-8.333333333333343
				],
				[
					18.33333333333337,
					-8.888880411783873
				],
				[
					18.88885498046875,
					-8.888880411783873
				],
				[
					20,
					-8.888880411783873
				],
				[
					20,
					-9.444452921549498
				],
				[
					20.55552164713538,
					-9.444452921549498
				],
				[
					21.111094156901004,
					-9.444452921549498
				],
				[
					22.22218831380212,
					-9.444452921549498
				],
				[
					22.777760823567746,
					-9.444452921549498
				],
				[
					23.33333333333337,
					-9.444452921549498
				],
				[
					24.444427490234375,
					-9.444452921549498
				],
				[
					25.55552164713538,
					-10.55554707845053
				],
				[
					26.111094156901004,
					-10.55554707845053
				],
				[
					26.66666666666663,
					-10.55554707845053
				],
				[
					27.22218831380212,
					-10.55554707845053
				],
				[
					28.33333333333337,
					-10.55554707845053
				],
				[
					29.444427490234375,
					-10.55554707845053
				],
				[
					30,
					-11.111119588216155
				],
				[
					31.66666666666663,
					-11.111119588216155
				],
				[
					32.22218831380212,
					-11.111119588216155
				],
				[
					32.777760823567746,
					-11.666666666666686
				],
				[
					33.33333333333337,
					-11.666666666666686
				],
				[
					33.88885498046875,
					-11.666666666666686
				],
				[
					34.444427490234375,
					-11.666666666666686
				],
				[
					35,
					-11.666666666666686
				],
				[
					36.111094156901004,
					-11.666666666666686
				],
				[
					37.22218831380212,
					-11.666666666666686
				],
				[
					38.33333333333337,
					-11.666666666666686
				],
				[
					38.88885498046875,
					-11.666666666666686
				],
				[
					39.444427490234375,
					-11.666666666666686
				],
				[
					40,
					-11.666666666666686
				],
				[
					40.55552164713538,
					-11.666666666666686
				],
				[
					41.111094156901004,
					-11.666666666666686
				],
				[
					41.66666666666663,
					-11.666666666666686
				],
				[
					42.777760823567746,
					-11.666666666666686
				],
				[
					43.33333333333337,
					-11.666666666666686
				],
				[
					43.88885498046875,
					-11.666666666666686
				],
				[
					45,
					-11.666666666666686
				],
				[
					45.55552164713538,
					-11.666666666666686
				],
				[
					46.111094156901004,
					-11.666666666666686
				],
				[
					47.777760823567746,
					-11.666666666666686
				],
				[
					49.444427490234375,
					-11.666666666666686
				],
				[
					50.55552164713538,
					-11.666666666666686
				],
				[
					52.22218831380212,
					-11.666666666666686
				],
				[
					53.33333333333337,
					-11.666666666666686
				],
				[
					53.88885498046875,
					-11.666666666666686
				],
				[
					53.33333333333337,
					-11.666666666666686
				],
				[
					50.55552164713538,
					-11.111119588216155
				],
				[
					49.444427490234375,
					-11.111119588216155
				],
				[
					48.88885498046875,
					-11.111119588216155
				],
				[
					48.33333333333337,
					-11.111119588216155
				],
				[
					45,
					-10.55554707845053
				],
				[
					43.88885498046875,
					-10.55554707845053
				],
				[
					43.33333333333337,
					-10.55554707845053
				],
				[
					42.777760823567746,
					-10.55554707845053
				],
				[
					41.66666666666663,
					-10.000000000000028
				],
				[
					41.66666666666663,
					-10.000000000000028
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 277,
			"versionNonce": 1882241532,
			"isDeleted": false,
			"id": "1HSehayY482lQcOCA5bcZ",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 332.2999394734702,
			"y": -31.65040159918246,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 103.888905843099,
			"height": 105.00000000000006,
			"seed": 1781157828,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					-0.5555470784505019
				],
				[
					0.5555216471353787,
					-1.6666666666666572
				],
				[
					1.6666666666666288,
					-2.7777608235676894
				],
				[
					3.88885498046875,
					-3.3333333333333144
				],
				[
					6.111094156901004,
					-4.444427490234374
				],
				[
					8.333333333333258,
					-5.555547078450502
				],
				[
					10.555521647135379,
					-6.666666666666657
				],
				[
					13.88885498046875,
					-7.777760823567689
				],
				[
					15,
					-8.333333333333314
				],
				[
					16.66666666666663,
					-8.888880411783845
				],
				[
					18.333333333333258,
					-10
				],
				[
					19.444427490234375,
					-10
				],
				[
					21.111094156901004,
					-10.55554707845053
				],
				[
					21.66666666666663,
					-10.55554707845053
				],
				[
					22.222188313802008,
					-10.55554707845053
				],
				[
					23.333333333333258,
					-10.55554707845053
				],
				[
					23.88885498046875,
					-11.111094156901032
				],
				[
					24.444427490234375,
					-11.111094156901032
				],
				[
					25,
					-11.111094156901032
				],
				[
					26.111094156901004,
					-11.666666666666657
				],
				[
					26.66666666666663,
					-11.666666666666657
				],
				[
					27.777760823567633,
					-11.666666666666657
				],
				[
					28.88885498046875,
					-11.666666666666657
				],
				[
					31.111094156901004,
					-12.222213745117188
				],
				[
					32.77776082356763,
					-12.777760823567718
				],
				[
					36.66666666666663,
					-12.777760823567718
				],
				[
					39.444427490234375,
					-13.333333333333343
				],
				[
					41.66666666666663,
					-13.888880411783845
				],
				[
					43.88885498046875,
					-14.444427490234375
				],
				[
					44.444427490234375,
					-14.444427490234375
				],
				[
					45.55552164713538,
					-15
				],
				[
					46.66666666666663,
					-15
				],
				[
					47.22218831380201,
					-15
				],
				[
					48.33333333333326,
					-15
				],
				[
					48.88885498046875,
					-15.55554707845053
				],
				[
					49.444427490234375,
					-15.55554707845053
				],
				[
					50,
					-15.55554707845053
				],
				[
					50.55552164713538,
					-15.55554707845053
				],
				[
					51.111094156901004,
					-15.55554707845053
				],
				[
					51.66666666666663,
					-15.55554707845053
				],
				[
					52.22218831380201,
					-15.55554707845053
				],
				[
					53.33333333333326,
					-15.55554707845053
				],
				[
					53.88885498046875,
					-15.55554707845053
				],
				[
					55,
					-15.55554707845053
				],
				[
					56.111094156901004,
					-15.55554707845053
				],
				[
					57.22218831380201,
					-15
				],
				[
					57.77776082356763,
					-15
				],
				[
					60,
					-13.888880411783845
				],
				[
					61.111094156901004,
					-13.333333333333343
				],
				[
					62.22218831380201,
					-13.333333333333343
				],
				[
					62.77776082356763,
					-12.777760823567718
				],
				[
					63.33333333333326,
					-12.222213745117188
				],
				[
					64.44442749023438,
					-12.222213745117188
				],
				[
					65,
					-11.666666666666657
				],
				[
					66.111094156901,
					-11.111094156901032
				],
				[
					67.22218831380201,
					-10
				],
				[
					67.77776082356763,
					-10
				],
				[
					68.33333333333326,
					-9.444427490234375
				],
				[
					69.44442749023438,
					-8.333333333333314
				],
				[
					70,
					-7.777760823567689
				],
				[
					70.55552164713538,
					-7.2222137451171875
				],
				[
					70.55552164713538,
					-6.666666666666657
				],
				[
					71.111094156901,
					-6.666666666666657
				],
				[
					71.111094156901,
					-6.111094156901032
				],
				[
					71.66666666666663,
					-5.555547078450502
				],
				[
					71.66666666666663,
					-5
				],
				[
					72.22218831380201,
					-3.8888804117838447
				],
				[
					72.22218831380201,
					-3.3333333333333144
				],
				[
					72.22218831380201,
					-2.2222137451171875
				],
				[
					72.77776082356763,
					-1.6666666666666572
				],
				[
					72.77776082356763,
					-1.1110941569010322
				],
				[
					72.77776082356763,
					0
				],
				[
					72.77776082356763,
					0.555572509765625
				],
				[
					73.33333333333326,
					2.2222391764323106
				],
				[
					73.33333333333326,
					2.7777862548828125
				],
				[
					73.88885498046875,
					4.444452921549498
				],
				[
					74.44442749023438,
					5
				],
				[
					74.44442749023438,
					5.555572509765625
				],
				[
					74.44442749023438,
					6.666666666666686
				],
				[
					74.44442749023438,
					7.7777862548828125
				],
				[
					75.55552164713538,
					8.888905843098968
				],
				[
					75.55552164713538,
					10
				],
				[
					76.111094156901,
					11.666666666666686
				],
				[
					76.66666666666663,
					12.22223917643231
				],
				[
					76.66666666666663,
					13.333333333333343
				],
				[
					77.22218831380201,
					14.444452921549498
				],
				[
					77.77776082356763,
					15.555572509765653
				],
				[
					78.33333333333326,
					16.111119588216155
				],
				[
					78.33333333333326,
					17.77778625488284
				],
				[
					78.88885498046875,
					18.888905843098968
				],
				[
					79.44442749023438,
					19.444452921549498
				],
				[
					79.44442749023438,
					20.555572509765653
				],
				[
					79.44442749023438,
					22.22223917643231
				],
				[
					79.44442749023438,
					23.333333333333343
				],
				[
					80,
					25.00000000000003
				],
				[
					80.55552164713538,
					26.111119588216155
				],
				[
					80.55552164713538,
					26.666666666666686
				],
				[
					81.111094156901,
					27.77778625488284
				],
				[
					81.111094156901,
					28.333333333333343
				],
				[
					81.111094156901,
					29.444452921549498
				],
				[
					81.111094156901,
					30.00000000000003
				],
				[
					81.111094156901,
					30.555572509765653
				],
				[
					81.111094156901,
					31.666666666666686
				],
				[
					81.66666666666663,
					33.33333333333334
				],
				[
					81.66666666666663,
					33.88890584309897
				],
				[
					81.66666666666663,
					35.00000000000003
				],
				[
					81.66666666666663,
					36.111119588216155
				],
				[
					81.66666666666663,
					37.22223917643231
				],
				[
					81.66666666666663,
					38.888905843098996
				],
				[
					82.77776082356763,
					40.55557250976565
				],
				[
					82.77776082356763,
					41.666666666666686
				],
				[
					83.33333333333326,
					42.77778625488284
				],
				[
					83.33333333333326,
					43.33333333333337
				],
				[
					83.33333333333326,
					43.888905843098996
				],
				[
					83.33333333333326,
					45.00000000000003
				],
				[
					83.88885498046875,
					47.22223917643231
				],
				[
					83.88885498046875,
					47.77778625488284
				],
				[
					84.44442749023438,
					50.00000000000003
				],
				[
					84.44442749023438,
					51.111119588216184
				],
				[
					85,
					51.666666666666686
				],
				[
					85,
					52.22223917643231
				],
				[
					85,
					52.77778625488284
				],
				[
					85,
					53.33333333333337
				],
				[
					85,
					53.888905843098996
				],
				[
					85,
					55.00000000000003
				],
				[
					85,
					55.55557250976565
				],
				[
					85,
					56.111119588216184
				],
				[
					85,
					57.22223917643231
				],
				[
					85,
					57.77778625488284
				],
				[
					85,
					58.888905843098996
				],
				[
					84.44442749023438,
					59.4444529215495
				],
				[
					83.88885498046875,
					60.55557250976565
				],
				[
					83.33333333333326,
					61.111119588216184
				],
				[
					82.77776082356763,
					62.77778625488284
				],
				[
					81.66666666666663,
					63.33333333333337
				],
				[
					81.111094156901,
					64.44445292154953
				],
				[
					80,
					66.11111958821618
				],
				[
					79.44442749023438,
					67.22223917643234
				],
				[
					77.77776082356763,
					67.77778625488284
				],
				[
					76.66666666666663,
					68.888905843099
				],
				[
					76.111094156901,
					69.44445292154953
				],
				[
					75.55552164713538,
					70.00000000000003
				],
				[
					74.44442749023438,
					70.55557250976565
				],
				[
					73.33333333333326,
					70.55557250976565
				],
				[
					73.33333333333326,
					71.11111958821618
				],
				[
					72.22218831380201,
					71.66666666666671
				],
				[
					71.111094156901,
					72.22223917643234
				],
				[
					69.44442749023438,
					72.77778625488284
				],
				[
					68.33333333333326,
					73.88890584309902
				],
				[
					67.22218831380201,
					74.44445292154953
				],
				[
					66.111094156901,
					74.44445292154953
				],
				[
					65.55552164713538,
					75.00000000000003
				],
				[
					65,
					76.11111958821621
				],
				[
					64.44442749023438,
					76.66666666666671
				],
				[
					63.88885498046875,
					76.66666666666671
				],
				[
					62.77776082356763,
					77.77778625488284
				],
				[
					62.22218831380201,
					77.77778625488284
				],
				[
					61.66666666666663,
					77.77778625488284
				],
				[
					60,
					78.3333333333334
				],
				[
					59.444427490234375,
					78.88890584309902
				],
				[
					58.88885498046875,
					78.88890584309902
				],
				[
					58.33333333333326,
					79.44445292154953
				],
				[
					57.77776082356763,
					79.44445292154953
				],
				[
					56.66666666666663,
					80.55557250976565
				],
				[
					56.111094156901004,
					81.11111958821621
				],
				[
					55.55552164713538,
					81.11111958821621
				],
				[
					55,
					81.66666666666671
				],
				[
					53.88885498046875,
					81.66666666666671
				],
				[
					52.22218831380201,
					82.77778625488284
				],
				[
					51.66666666666663,
					82.77778625488284
				],
				[
					50.55552164713538,
					83.88890584309902
				],
				[
					48.88885498046875,
					84.44445292154953
				],
				[
					47.77776082356763,
					85.00000000000003
				],
				[
					46.66666666666663,
					85.00000000000003
				],
				[
					45,
					86.11111958821621
				],
				[
					43.88885498046875,
					86.66666666666671
				],
				[
					41.66666666666663,
					87.22223917643234
				],
				[
					40.55552164713538,
					87.77778625488284
				],
				[
					38.33333333333326,
					88.3333333333334
				],
				[
					37.22218831380201,
					88.88890584309902
				],
				[
					36.66666666666663,
					88.88890584309902
				],
				[
					36.111094156901004,
					88.88890584309902
				],
				[
					35,
					89.44445292154953
				],
				[
					34.444427490234375,
					89.44445292154953
				],
				[
					33.33333333333326,
					89.44445292154953
				],
				[
					32.22218831380201,
					89.44445292154953
				],
				[
					31.66666666666663,
					89.44445292154953
				],
				[
					31.111094156901004,
					89.44445292154953
				],
				[
					30,
					89.44445292154953
				],
				[
					28.88885498046875,
					89.44445292154953
				],
				[
					28.333333333333258,
					89.44445292154953
				],
				[
					27.777760823567633,
					89.44445292154953
				],
				[
					26.66666666666663,
					88.88890584309902
				],
				[
					26.111094156901004,
					88.88890584309902
				],
				[
					25.55552164713538,
					88.88890584309902
				],
				[
					24.444427490234375,
					88.3333333333334
				],
				[
					24.444427490234375,
					87.77778625488284
				],
				[
					23.333333333333258,
					87.22223917643234
				],
				[
					22.222188313802008,
					86.66666666666671
				],
				[
					21.66666666666663,
					86.66666666666671
				],
				[
					21.111094156901004,
					86.11111958821621
				],
				[
					20.55552164713538,
					86.11111958821621
				],
				[
					18.333333333333258,
					85.00000000000003
				],
				[
					17.777760823567633,
					85.00000000000003
				],
				[
					16.66666666666663,
					84.44445292154953
				],
				[
					16.111094156901004,
					84.44445292154953
				],
				[
					16.111094156901004,
					83.88890584309902
				],
				[
					15.555521647135379,
					83.3333333333334
				],
				[
					15,
					83.3333333333334
				],
				[
					13.88885498046875,
					82.77778625488284
				],
				[
					13.333333333333258,
					82.77778625488284
				],
				[
					12.777760823567633,
					82.22223917643234
				],
				[
					12.222188313802008,
					81.66666666666671
				],
				[
					11.666666666666629,
					81.66666666666671
				],
				[
					10.555521647135379,
					81.66666666666671
				],
				[
					10,
					81.66666666666671
				],
				[
					9.444427490234375,
					81.11111958821621
				],
				[
					8.88885498046875,
					80.55557250976565
				],
				[
					8.333333333333258,
					80.00000000000003
				],
				[
					7.2221883138020075,
					79.44445292154953
				],
				[
					6.111094156901004,
					78.88890584309902
				],
				[
					5,
					78.3333333333334
				],
				[
					4.444427490234375,
					78.3333333333334
				],
				[
					3.88885498046875,
					77.77778625488284
				],
				[
					2.2221883138020075,
					77.77778625488284
				],
				[
					1.6666666666666288,
					77.22223917643234
				],
				[
					0.5555216471353788,
					76.66666666666671
				],
				[
					-0.555572509765625,
					76.11111958821621
				],
				[
					-1.11114501953125,
					75.55557250976565
				],
				[
					-2.2222391764323675,
					75.00000000000003
				],
				[
					-2.2222391764323675,
					74.44445292154953
				],
				[
					-3.3333333333333712,
					74.44445292154953
				],
				[
					-3.8889058430989962,
					73.88890584309902
				],
				[
					-4.444478352864621,
					73.3333333333334
				],
				[
					-5.555572509765625,
					72.77778625488284
				],
				[
					-5.555572509765625,
					72.22223917643234
				],
				[
					-6.111145019531364,
					71.66666666666671
				],
				[
					-6.6666666666667425,
					70.55557250976565
				],
				[
					-7.2222391764323675,
					69.44445292154953
				],
				[
					-8.333333333333371,
					68.33333333333337
				],
				[
					-8.333333333333371,
					67.77778625488284
				],
				[
					-8.888905843098996,
					66.66666666666671
				],
				[
					-8.888905843098996,
					66.11111958821618
				],
				[
					-9.444478352864621,
					66.11111958821618
				],
				[
					-9.444478352864621,
					65.00000000000003
				],
				[
					-9.444478352864621,
					64.44445292154953
				],
				[
					-9.444478352864621,
					63.888905843098996
				],
				[
					-9.444478352864621,
					63.33333333333337
				],
				[
					-10.555572509765739,
					62.77778625488284
				],
				[
					-10.555572509765739,
					62.22223917643234
				],
				[
					-10.555572509765739,
					61.666666666666714
				],
				[
					-11.111145019531364,
					61.111119588216184
				],
				[
					-11.111145019531364,
					60.55557250976565
				],
				[
					-11.111145019531364,
					60.00000000000003
				],
				[
					-11.666666666666742,
					59.4444529215495
				],
				[
					-12.222239176432367,
					59.4444529215495
				],
				[
					-12.777811686197992,
					59.4444529215495
				],
				[
					-13.333333333333371,
					59.4444529215495
				],
				[
					-14.444478352864621,
					59.4444529215495
				],
				[
					-15.000000000000114,
					59.4444529215495
				],
				[
					-16.111145019531364,
					59.4444529215495
				],
				[
					-16.666666666666742,
					59.4444529215495
				],
				[
					-17.222239176432367,
					59.4444529215495
				],
				[
					-17.777811686197992,
					59.4444529215495
				],
				[
					-18.33333333333337,
					59.4444529215495
				],
				[
					-18.888905843098996,
					60.00000000000003
				],
				[
					-18.888905843098996,
					60.00000000000003
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 271,
			"versionNonce": 1942811588,
			"isDeleted": false,
			"id": "Qu-Eory7TfG1R0bpV1qJr",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 407.2999394734702,
			"y": -35.5392820109663,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 108.88890584309911,
			"height": 93.88888041178387,
			"seed": 1861792124,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.5555216471353788,
					1.1102230246251565e-16
				],
				[
					1.1110941569010038,
					0
				],
				[
					2.2221883138020075,
					0.5555470784505304
				],
				[
					2.7777608235676325,
					1.1111195882161553
				],
				[
					3.3333333333332575,
					2.2222137451171875
				],
				[
					3.3333333333332575,
					2.7777862548828125
				],
				[
					3.88885498046875,
					4.44445292154947
				],
				[
					3.888854980468749,
					5.55554707845053
				],
				[
					4.444427490234375,
					6.666666666666657
				],
				[
					5,
					8.333333333333343
				],
				[
					6.111094156901004,
					10
				],
				[
					6.111094156901004,
					10.55554707845053
				],
				[
					6.666666666666629,
					11.666666666666657
				],
				[
					6.666666666666629,
					12.777786254882812
				],
				[
					6.666666666666629,
					13.333333333333343
				],
				[
					6.666666666666629,
					13.888880411783845
				],
				[
					6.666666666666629,
					15
				],
				[
					7.2221883138020075,
					16.111119588216155
				],
				[
					7.2221883138020075,
					16.666666666666657
				],
				[
					7.2221883138020075,
					17.777786254882812
				],
				[
					7.2221883138020075,
					18.888880411783873
				],
				[
					7.7777608235676325,
					20.55554707845053
				],
				[
					8.333333333333258,
					21.666666666666686
				],
				[
					8.88885498046875,
					22.777786254882812
				],
				[
					8.88885498046875,
					23.333333333333343
				],
				[
					8.88885498046875,
					23.888880411783873
				],
				[
					9.444427490234375,
					24.444452921549498
				],
				[
					10,
					25
				],
				[
					10.555521647135379,
					26.111119588216155
				],
				[
					10.555521647135379,
					26.666666666666686
				],
				[
					10.555521647135379,
					27.222213745117188
				],
				[
					10.555521647135379,
					27.777786254882812
				],
				[
					10.555521647135379,
					28.333333333333343
				],
				[
					10.555521647135379,
					28.888880411783873
				],
				[
					11.111094156901004,
					29.444452921549498
				],
				[
					11.111094156901004,
					30
				],
				[
					12.222188313802008,
					30.55554707845053
				],
				[
					12.222188313802008,
					31.111119588216155
				],
				[
					12.777760823567746,
					31.666666666666686
				],
				[
					13.333333333333371,
					32.22221374511719
				],
				[
					13.88885498046875,
					33.33333333333334
				],
				[
					13.88885498046875,
					33.88888041178387
				],
				[
					13.88885498046875,
					35
				],
				[
					14.444427490234375,
					36.111119588216155
				],
				[
					15,
					37.22221374511719
				],
				[
					15.555521647135379,
					37.77778625488281
				],
				[
					15.555521647135379,
					38.33333333333334
				],
				[
					15.555521647135379,
					38.88888041178387
				],
				[
					16.111094156901004,
					40
				],
				[
					16.111094156901004,
					40.55554707845053
				],
				[
					16.66666666666663,
					41.111119588216155
				],
				[
					17.22218831380212,
					41.666666666666686
				],
				[
					17.22218831380212,
					42.222213745117216
				],
				[
					17.777760823567746,
					42.77778625488284
				],
				[
					17.777760823567746,
					43.88888041178387
				],
				[
					17.777760823567746,
					44.4444529215495
				],
				[
					17.777760823567746,
					45.00000000000003
				],
				[
					17.777760823567746,
					45.55554707845053
				],
				[
					17.777760823567746,
					46.666666666666686
				],
				[
					17.777760823567746,
					47.77778625488284
				],
				[
					17.777760823567746,
					48.33333333333334
				],
				[
					17.22218831380212,
					49.4444529215495
				],
				[
					17.22218831380212,
					50.00000000000003
				],
				[
					16.66666666666663,
					51.111119588216155
				],
				[
					15.555521647135379,
					51.666666666666686
				],
				[
					15,
					52.222213745117216
				],
				[
					14.444427490234375,
					52.77778625488284
				],
				[
					13.88885498046875,
					53.88888041178387
				],
				[
					13.88885498046875,
					54.4444529215495
				],
				[
					13.333333333333371,
					54.4444529215495
				],
				[
					12.777760823567746,
					55.55554707845053
				],
				[
					12.222188313802008,
					56.666666666666686
				],
				[
					11.666666666666629,
					57.222213745117216
				],
				[
					11.666666666666629,
					57.77778625488284
				],
				[
					11.111094156901004,
					57.77778625488284
				],
				[
					11.111094156901004,
					58.33333333333334
				],
				[
					10.555521647135379,
					58.88888041178387
				],
				[
					10.555521647135379,
					59.4444529215495
				],
				[
					10,
					60.00000000000003
				],
				[
					10,
					60.55554707845053
				],
				[
					9.444427490234375,
					60.55554707845053
				],
				[
					9.444427490234375,
					61.111119588216155
				],
				[
					8.88885498046875,
					61.666666666666686
				],
				[
					8.333333333333258,
					62.77778625488284
				],
				[
					8.333333333333258,
					63.88888041178387
				],
				[
					7.7777608235676325,
					64.4444529215495
				],
				[
					7.7777608235676325,
					65.00000000000003
				],
				[
					7.7777608235676325,
					66.11111958821618
				],
				[
					7.7777608235676325,
					66.66666666666669
				],
				[
					7.2221883138020075,
					67.22221374511722
				],
				[
					6.666666666666629,
					67.77778625488284
				],
				[
					6.111094156901004,
					68.88888041178387
				],
				[
					6.111094156901004,
					70.00000000000003
				],
				[
					5.555521647135379,
					70.55554707845056
				],
				[
					5,
					71.11111958821618
				],
				[
					4.444427490234375,
					71.66666666666669
				],
				[
					4.444427490234375,
					72.77778625488284
				],
				[
					4.444427490234375,
					73.88888041178387
				],
				[
					3.3333333333332575,
					74.4444529215495
				],
				[
					3.3333333333332575,
					75.55554707845056
				],
				[
					2.7777608235676325,
					76.11111958821618
				],
				[
					2.7777608235676325,
					76.66666666666669
				],
				[
					2.2221883138020075,
					77.22221374511724
				],
				[
					1.6666666666666288,
					78.33333333333337
				],
				[
					1.6666666666666288,
					78.88888041178387
				],
				[
					1.1110941569010038,
					79.4444529215495
				],
				[
					1.1110941569010038,
					80.00000000000006
				],
				[
					0.5555216471353788,
					80.55554707845056
				],
				[
					0,
					81.11111958821618
				],
				[
					0,
					81.66666666666669
				],
				[
					-0.555572509765625,
					82.22221374511724
				],
				[
					-1.6666666666667425,
					83.33333333333337
				],
				[
					-2.7778116861979925,
					83.33333333333337
				],
				[
					-3.3333333333333712,
					83.88888041178387
				],
				[
					-4.444478352864621,
					85.00000000000006
				],
				[
					-5,
					85.55554707845056
				],
				[
					-5.555572509765625,
					86.11111958821618
				],
				[
					-6.6666666666667425,
					86.66666666666669
				],
				[
					-7.7778116861979925,
					86.66666666666669
				],
				[
					-8.333333333333371,
					87.22221374511724
				],
				[
					-9.444478352864621,
					87.77778625488287
				],
				[
					-10,
					87.77778625488287
				],
				[
					-11.11114501953125,
					88.33333333333337
				],
				[
					-11.666666666666742,
					88.33333333333337
				],
				[
					-12.222239176432367,
					88.33333333333337
				],
				[
					-12.777811686197992,
					88.88888041178387
				],
				[
					-13.333333333333371,
					89.4444529215495
				],
				[
					-14.444478352864621,
					89.4444529215495
				],
				[
					-15,
					90.00000000000006
				],
				[
					-15.555572509765625,
					90.00000000000006
				],
				[
					-16.666666666666742,
					90.00000000000006
				],
				[
					-17.777811686197992,
					90.55554707845056
				],
				[
					-18.33333333333337,
					90.55554707845056
				],
				[
					-18.33333333333337,
					91.11111958821618
				],
				[
					-19.44447835286462,
					91.11111958821618
				],
				[
					-20,
					91.66666666666669
				],
				[
					-20.555572509765625,
					91.66666666666669
				],
				[
					-21.11114501953125,
					91.66666666666669
				],
				[
					-21.666666666666742,
					92.22221374511724
				],
				[
					-22.222239176432367,
					92.22221374511724
				],
				[
					-23.33333333333337,
					92.22221374511724
				],
				[
					-23.888905843098996,
					92.22221374511724
				],
				[
					-24.44447835286462,
					92.77778625488287
				],
				[
					-24.44447835286462,
					93.33333333333337
				],
				[
					-26.11114501953125,
					93.33333333333337
				],
				[
					-26.666666666666742,
					93.33333333333337
				],
				[
					-27.222239176432367,
					93.33333333333337
				],
				[
					-27.777811686197992,
					93.33333333333337
				],
				[
					-28.33333333333337,
					93.33333333333337
				],
				[
					-28.888905843098996,
					93.33333333333337
				],
				[
					-30,
					93.33333333333337
				],
				[
					-31.11114501953125,
					93.33333333333337
				],
				[
					-31.666666666666742,
					93.33333333333337
				],
				[
					-32.22223917643237,
					93.88888041178387
				],
				[
					-32.77781168619799,
					93.88888041178387
				],
				[
					-33.888905843098996,
					93.88888041178387
				],
				[
					-34.44447835286462,
					93.88888041178387
				],
				[
					-35,
					93.88888041178387
				],
				[
					-36.66666666666674,
					93.88888041178387
				],
				[
					-37.77781168619799,
					93.88888041178387
				],
				[
					-38.33333333333337,
					93.88888041178387
				],
				[
					-38.888905843098996,
					93.88888041178387
				],
				[
					-40,
					93.88888041178387
				],
				[
					-40.555572509765625,
					93.88888041178387
				],
				[
					-41.66666666666674,
					93.88888041178387
				],
				[
					-42.22223917643237,
					93.88888041178387
				],
				[
					-42.77781168619799,
					93.88888041178387
				],
				[
					-43.888905843098996,
					93.88888041178387
				],
				[
					-44.44447835286462,
					93.88888041178387
				],
				[
					-45,
					93.88888041178387
				],
				[
					-45.555572509765625,
					93.88888041178387
				],
				[
					-46.66666666666674,
					93.88888041178387
				],
				[
					-47.22223917643237,
					93.88888041178387
				],
				[
					-47.77781168619799,
					93.88888041178387
				],
				[
					-48.33333333333337,
					93.88888041178387
				],
				[
					-48.888905843098996,
					93.88888041178387
				],
				[
					-49.44447835286462,
					93.88888041178387
				],
				[
					-50,
					93.88888041178387
				],
				[
					-50.555572509765625,
					93.33333333333337
				],
				[
					-51.11114501953125,
					93.33333333333337
				],
				[
					-51.66666666666674,
					92.77778625488287
				],
				[
					-52.22223917643237,
					92.77778625488287
				],
				[
					-52.77781168619799,
					92.77778625488287
				],
				[
					-53.888905843098996,
					92.77778625488287
				],
				[
					-54.44447835286462,
					92.77778625488287
				],
				[
					-55,
					92.77778625488287
				],
				[
					-55.555572509765625,
					92.22221374511724
				],
				[
					-56.66666666666674,
					92.22221374511724
				],
				[
					-57.22223917643237,
					92.22221374511724
				],
				[
					-58.33333333333337,
					92.22221374511724
				],
				[
					-59.44447835286462,
					91.66666666666669
				],
				[
					-60,
					91.66666666666669
				],
				[
					-60.555572509765625,
					91.66666666666669
				],
				[
					-61.11114501953125,
					91.11111958821618
				],
				[
					-62.22223917643237,
					91.11111958821618
				],
				[
					-62.77781168619799,
					91.11111958821618
				],
				[
					-63.33333333333337,
					90.55554707845056
				],
				[
					-63.888905843098996,
					90.55554707845056
				],
				[
					-64.44447835286462,
					90.55554707845056
				],
				[
					-64.44447835286462,
					90.00000000000006
				],
				[
					-65,
					90.00000000000006
				],
				[
					-65.55557250976562,
					89.4444529215495
				],
				[
					-66.66666666666674,
					88.88888041178387
				],
				[
					-67.77781168619799,
					88.33333333333337
				],
				[
					-68.33333333333337,
					87.77778625488287
				],
				[
					-69.44447835286462,
					87.22221374511724
				],
				[
					-70,
					87.22221374511724
				],
				[
					-70.55557250976562,
					86.66666666666669
				],
				[
					-71.11114501953125,
					86.66666666666669
				],
				[
					-71.66666666666674,
					86.11111958821618
				],
				[
					-72.22223917643237,
					86.11111958821618
				],
				[
					-72.77781168619799,
					85.55554707845056
				],
				[
					-73.888905843099,
					85.00000000000006
				],
				[
					-74.44447835286462,
					84.4444529215495
				],
				[
					-75,
					84.4444529215495
				],
				[
					-75.55557250976562,
					83.33333333333337
				],
				[
					-76.11114501953125,
					82.77778625488287
				],
				[
					-76.66666666666674,
					82.77778625488287
				],
				[
					-77.22223917643237,
					82.22221374511724
				],
				[
					-77.77781168619799,
					82.22221374511724
				],
				[
					-77.77781168619799,
					81.66666666666669
				],
				[
					-78.33333333333337,
					81.11111958821618
				],
				[
					-78.888905843099,
					80.55554707845056
				],
				[
					-79.44447835286462,
					80.55554707845056
				],
				[
					-80,
					80.00000000000006
				],
				[
					-80.55557250976562,
					80.00000000000006
				],
				[
					-81.11114501953136,
					80.00000000000006
				],
				[
					-81.66666666666674,
					79.4444529215495
				],
				[
					-82.22223917643237,
					78.88888041178387
				],
				[
					-82.77781168619799,
					78.33333333333337
				],
				[
					-82.77781168619799,
					77.77778625488287
				],
				[
					-83.33333333333337,
					77.22221374511724
				],
				[
					-83.888905843099,
					76.66666666666669
				],
				[
					-83.888905843099,
					76.11111958821618
				],
				[
					-84.44447835286462,
					76.11111958821618
				],
				[
					-84.44447835286462,
					75.55554707845056
				],
				[
					-85.00000000000011,
					75.00000000000003
				],
				[
					-85.00000000000011,
					73.88888041178387
				],
				[
					-85.00000000000011,
					73.33333333333337
				],
				[
					-85.55557250976574,
					72.77778625488284
				],
				[
					-85.55557250976574,
					72.22221374511722
				],
				[
					-85.55557250976574,
					71.66666666666669
				],
				[
					-86.11114501953136,
					71.11111958821618
				],
				[
					-86.11114501953136,
					70.55554707845056
				],
				[
					-86.66666666666674,
					70.00000000000003
				],
				[
					-87.22223917643237,
					69.4444529215495
				],
				[
					-87.22223917643237,
					68.88888041178387
				],
				[
					-87.22223917643237,
					68.33333333333337
				],
				[
					-87.22223917643237,
					67.77778625488284
				],
				[
					-87.22223917643237,
					67.22221374511722
				],
				[
					-87.77781168619799,
					66.66666666666669
				],
				[
					-88.33333333333337,
					66.66666666666669
				],
				[
					-88.33333333333337,
					66.11111958821618
				],
				[
					-88.33333333333337,
					65.55554707845056
				],
				[
					-88.888905843099,
					64.4444529215495
				],
				[
					-89.44447835286462,
					63.88888041178387
				],
				[
					-89.44447835286462,
					63.33333333333334
				],
				[
					-90.00000000000011,
					63.33333333333334
				],
				[
					-90.55557250976574,
					62.77778625488284
				],
				[
					-91.11114501953136,
					62.222213745117216
				],
				[
					-91.11114501953136,
					62.222213745117216
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 71,
			"versionNonce": 847256188,
			"isDeleted": false,
			"id": "PoW-v02xEw5chT6Jdn3qO",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 395.63327280680346,
			"y": 45.57183757724988,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 21.666666666666742,
			"height": 39.4444529215495,
			"seed": 1087882492,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					-0.555572509765625
				],
				[
					0.5555216471354923,
					-0.5555725097656252
				],
				[
					1.1110941569011175,
					-1.1111195882161269
				],
				[
					1.1110941569011172,
					-1.6666666666666854
				],
				[
					1.6666666666667422,
					-1.6666666666666858
				],
				[
					2.2221883138021212,
					-2.2222391764323106
				],
				[
					2.2221883138021212,
					-2.7777862548828125
				],
				[
					2.7777608235677467,
					-3.3333333333333144
				],
				[
					3.3333333333333703,
					-3.888905843098939
				],
				[
					3.8888549804687496,
					-4.444452921549498
				],
				[
					4.444427490234375,
					-5.000000000000001
				],
				[
					5.000000000000001,
					-5.555572509765625
				],
				[
					5.5555216471354925,
					-6.666666666666686
				],
				[
					6.6666666666667425,
					-7.7777862548828125
				],
				[
					7.222188313802121,
					-8.888905843098968
				],
				[
					7.222188313802121,
					-9.444452921549498
				],
				[
					7.777760823567746,
					-10.555572509765625
				],
				[
					8.333333333333371,
					-11.111119588216155
				],
				[
					8.88885498046875,
					-11.111119588216155
				],
				[
					8.88885498046875,
					-11.666666666666686
				],
				[
					9.444427490234375,
					-12.777786254882812
				],
				[
					9.444427490234375,
					-13.333333333333343
				],
				[
					10,
					-15
				],
				[
					10.555521647135492,
					-15
				],
				[
					10.555521647135492,
					-15.555572509765625
				],
				[
					10.555521647135492,
					-16.111119588216155
				],
				[
					11.111094156901117,
					-17.22223917643231
				],
				[
					11.111094156901117,
					-17.77778625488284
				],
				[
					11.666666666666742,
					-18.333333333333343
				],
				[
					12.222188313802121,
					-18.888905843098968
				],
				[
					12.222188313802121,
					-19.444452921549498
				],
				[
					12.777760823567746,
					-20.00000000000003
				],
				[
					13.333333333333371,
					-20.555572509765653
				],
				[
					13.333333333333371,
					-21.111119588216155
				],
				[
					13.333333333333371,
					-21.666666666666686
				],
				[
					13.88885498046875,
					-22.22223917643231
				],
				[
					13.88885498046875,
					-22.77778625488284
				],
				[
					14.444427490234375,
					-23.333333333333343
				],
				[
					14.444427490234375,
					-23.888905843098968
				],
				[
					14.444427490234375,
					-24.444452921549498
				],
				[
					15,
					-25.00000000000003
				],
				[
					15,
					-25.555572509765653
				],
				[
					15,
					-26.111119588216155
				],
				[
					15.555521647135492,
					-27.22223917643231
				],
				[
					15.555521647135492,
					-27.77778625488284
				],
				[
					16.111094156901117,
					-27.77778625488284
				],
				[
					16.666666666666742,
					-28.888905843098968
				],
				[
					17.22218831380212,
					-31.111119588216155
				],
				[
					17.22218831380212,
					-31.666666666666686
				],
				[
					17.777760823567746,
					-32.77778625488284
				],
				[
					18.88885498046875,
					-33.88890584309897
				],
				[
					19.444427490234375,
					-34.4444529215495
				],
				[
					19.444427490234375,
					-35.00000000000003
				],
				[
					20,
					-35.55557250976565
				],
				[
					20,
					-36.111119588216155
				],
				[
					20.555521647135492,
					-36.111119588216155
				],
				[
					21.111094156901117,
					-37.22223917643231
				],
				[
					21.111094156901117,
					-37.77778625488284
				],
				[
					21.666666666666742,
					-38.33333333333334
				],
				[
					21.666666666666742,
					-38.88890584309897
				],
				[
					21.666666666666742,
					-39.4444529215495
				],
				[
					21.666666666666742,
					-39.4444529215495
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 85,
			"versionNonce": 1224858436,
			"isDeleted": false,
			"id": "JynGxxNuABLZEA2RJmM-z",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 368.4110336303712,
			"y": -39.98373493251577,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 38.33333333333337,
			"height": 28.888905843098968,
			"seed": 1907469820,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.555572509765625,
					0
				],
				[
					1.1110941569010038,
					0
				],
				[
					2.2222391764322538,
					-6.661338147750939e-16
				],
				[
					2.7777608235677462,
					2.220446049250313e-16
				],
				[
					3.8889058430989962,
					0.555572509765625
				],
				[
					4.444427490234375,
					0.555572509765625
				],
				[
					5,
					1.1111195882161269
				],
				[
					5.555572509765625,
					1.1111195882161269
				],
				[
					6.1110941569010055,
					1.666666666666659
				],
				[
					6.666666666666629,
					1.6666666666666572
				],
				[
					7.222239176432253,
					1.6666666666666563
				],
				[
					7.777760823567746,
					2.222239176432282
				],
				[
					8.333333333333371,
					2.222239176432282
				],
				[
					9.444427490234375,
					2.7777862548828125
				],
				[
					10,
					2.7777862548828125
				],
				[
					10.555572509765625,
					2.7777862548828125
				],
				[
					11.111094156901004,
					2.7777862548828125
				],
				[
					11.666666666666629,
					2.7777862548828125
				],
				[
					12.222239176432254,
					2.7777862548828125
				],
				[
					12.777760823567746,
					2.7777862548828125
				],
				[
					13.333333333333371,
					3.3333333333333144
				],
				[
					13.888905843098996,
					3.3333333333333144
				],
				[
					14.444427490234375,
					3.3333333333333144
				],
				[
					15,
					3.3333333333333144
				],
				[
					15.555572509765625,
					3.3333333333333144
				],
				[
					16.111094156901004,
					3.3333333333333144
				],
				[
					16.66666666666663,
					3.3333333333333144
				],
				[
					17.222239176432254,
					3.3333333333333144
				],
				[
					17.777760823567746,
					3.3333333333333144
				],
				[
					18.33333333333337,
					3.3333333333333144
				],
				[
					18.888905843098996,
					3.8889058430989394
				],
				[
					19.444427490234375,
					4.44445292154947
				],
				[
					20,
					5
				],
				[
					20,
					5.555572509765625
				],
				[
					20,
					6.111119588216127
				],
				[
					20.555572509765625,
					6.666666666666657
				],
				[
					21.111094156901004,
					7.222239176432282
				],
				[
					21.66666666666663,
					7.7777862548828125
				],
				[
					21.66666666666663,
					8.333333333333314
				],
				[
					22.222239176432254,
					8.88890584309894
				],
				[
					22.222239176432254,
					9.44445292154947
				],
				[
					22.777760823567746,
					10
				],
				[
					23.33333333333337,
					10.555572509765625
				],
				[
					23.888905843098996,
					10.555572509765625
				],
				[
					23.888905843098996,
					11.666666666666657
				],
				[
					24.444427490234375,
					12.222239176432282
				],
				[
					25,
					12.777786254882812
				],
				[
					25.555572509765625,
					13.333333333333314
				],
				[
					26.111094156901004,
					13.88890584309894
				],
				[
					26.66666666666663,
					13.88890584309894
				],
				[
					27.222239176432254,
					14.44445292154947
				],
				[
					27.777760823567746,
					15
				],
				[
					28.33333333333337,
					15.555572509765625
				],
				[
					28.888905843098996,
					16.111119588216127
				],
				[
					29.444427490234375,
					16.666666666666657
				],
				[
					30,
					16.666666666666657
				],
				[
					30.555572509765625,
					16.666666666666657
				],
				[
					31.111094156901004,
					17.222239176432282
				],
				[
					31.66666666666663,
					17.777786254882812
				],
				[
					32.222239176432254,
					18.333333333333314
				],
				[
					32.777760823567746,
					18.88890584309894
				],
				[
					33.33333333333337,
					19.44445292154947
				],
				[
					33.33333333333337,
					20
				],
				[
					33.888905843098996,
					20
				],
				[
					33.888905843098996,
					20.555572509765625
				],
				[
					34.444427490234375,
					21.111119588216127
				],
				[
					35,
					21.666666666666657
				],
				[
					35,
					22.222239176432282
				],
				[
					35.555572509765625,
					22.777786254882812
				],
				[
					36.111094156901004,
					23.888905843098968
				],
				[
					36.66666666666663,
					23.888905843098968
				],
				[
					36.66666666666663,
					24.44445292154947
				],
				[
					36.66666666666663,
					25
				],
				[
					37.222239176432254,
					26.666666666666657
				],
				[
					37.777760823567746,
					27.777786254882812
				],
				[
					37.777760823567746,
					28.333333333333343
				],
				[
					38.33333333333337,
					28.888905843098968
				],
				[
					38.33333333333337,
					28.888905843098968
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 43,
			"versionNonce": 621767420,
			"isDeleted": false,
			"id": "FhwOHHl8o7oHROOOByzEP",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 322.2999394734701,
			"y": -41.094829089416834,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 26.111094156901117,
			"height": 9.44445292154947,
			"seed": 738481788,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.5555216471354925,
					-5.551115123125783e-17
				],
				[
					1.1110941569011175,
					0
				],
				[
					1.6666666666667425,
					0
				],
				[
					2.2221883138021212,
					4.440892098500626e-16
				],
				[
					4.444427490234489,
					-0.5555725097656263
				],
				[
					5.000000000000114,
					-0.555572509765625
				],
				[
					5.555521647135493,
					-0.5555725097656241
				],
				[
					6.1110941569011175,
					-1.1111195882161562
				],
				[
					6.6666666666667425,
					-1.1111195882161549
				],
				[
					7.777760823567746,
					-1.1111195882161549
				],
				[
					8.333333333333371,
					-1.6666666666666572
				],
				[
					8.888854980468864,
					-1.6666666666666572
				],
				[
					10.000000000000114,
					-2.222239176432282
				],
				[
					11.111094156901117,
					-2.7777862548828125
				],
				[
					11.666666666666742,
					-2.7777862548828125
				],
				[
					12.222188313802121,
					-3.333333333333343
				],
				[
					12.777760823567746,
					-3.333333333333343
				],
				[
					13.888854980468864,
					-3.888905843098968
				],
				[
					14.444427490234489,
					-3.888905843098968
				],
				[
					15.000000000000114,
					-3.888905843098968
				],
				[
					15.555521647135492,
					-4.44445292154947
				],
				[
					16.111094156901117,
					-5
				],
				[
					16.666666666666742,
					-5.555572509765625
				],
				[
					17.22218831380212,
					-5.555572509765625
				],
				[
					18.33333333333337,
					-6.111119588216155
				],
				[
					18.888854980468864,
					-6.666666666666657
				],
				[
					19.44442749023449,
					-6.666666666666657
				],
				[
					20.000000000000114,
					-6.666666666666657
				],
				[
					21.111094156901117,
					-7.222239176432282
				],
				[
					22.22218831380212,
					-7.7777862548828125
				],
				[
					22.777760823567746,
					-7.7777862548828125
				],
				[
					24.44442749023449,
					-8.333333333333343
				],
				[
					25.000000000000114,
					-8.888905843098968
				],
				[
					25.555521647135492,
					-9.44445292154947
				],
				[
					26.111094156901117,
					-8.888905843098968
				],
				[
					26.111094156901117,
					-8.888905843098968
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 145,
			"versionNonce": 103666372,
			"isDeleted": false,
			"id": "sGpUPxbSvWGddsmuSOEv6",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 330.63327280680346,
			"y": -40.5392820109663,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 78.888905843099,
			"height": 18.333333333333343,
			"seed": 1998987260,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-0.555572509765625,
					-0.5555470784505303
				],
				[
					-0.555572509765625,
					-1.1111195882161553
				],
				[
					-0.5555725097656252,
					-1.6666666666666856
				],
				[
					-0.555572509765625,
					-2.7777862548828125
				],
				[
					0,
					-3.333333333333343
				],
				[
					0.5555216471354925,
					-3.333333333333343
				],
				[
					1.1110941569011186,
					-3.888880411783873
				],
				[
					1.6666666666667422,
					-4.444452921549498
				],
				[
					2.2221883138021212,
					-5
				],
				[
					2.7777608235677467,
					-5.55554707845053
				],
				[
					2.777760823567747,
					-6.111119588216157
				],
				[
					3.3333333333333712,
					-6.666666666666686
				],
				[
					3.8888549804687496,
					-6.666666666666686
				],
				[
					4.444427490234375,
					-7.2222137451171875
				],
				[
					5,
					-7.7777862548828125
				],
				[
					5.5555216471354925,
					-7.7777862548828125
				],
				[
					6.1110941569011175,
					-8.333333333333343
				],
				[
					6.6666666666667425,
					-8.888880411783873
				],
				[
					7.222188313802121,
					-8.888880411783873
				],
				[
					8.333333333333371,
					-9.444452921549498
				],
				[
					8.88885498046875,
					-9.444452921549498
				],
				[
					9.444427490234375,
					-9.444452921549498
				],
				[
					10.555521647135492,
					-9.444452921549498
				],
				[
					11.111094156901117,
					-10
				],
				[
					11.666666666666742,
					-10
				],
				[
					12.222188313802121,
					-10.55554707845053
				],
				[
					12.777760823567746,
					-10.55554707845053
				],
				[
					13.333333333333371,
					-10.55554707845053
				],
				[
					13.88885498046875,
					-11.111119588216155
				],
				[
					14.444427490234375,
					-11.111119588216155
				],
				[
					15,
					-11.666666666666686
				],
				[
					15.555521647135492,
					-11.666666666666686
				],
				[
					15.555521647135492,
					-12.222213745117188
				],
				[
					16.111094156901117,
					-12.777786254882812
				],
				[
					16.666666666666742,
					-12.777786254882812
				],
				[
					17.22218831380212,
					-12.777786254882812
				],
				[
					17.777760823567746,
					-12.777786254882812
				],
				[
					18.33333333333337,
					-12.777786254882812
				],
				[
					18.88885498046875,
					-12.777786254882812
				],
				[
					18.88885498046875,
					-13.333333333333343
				],
				[
					19.444427490234375,
					-13.888880411783873
				],
				[
					20,
					-13.888880411783873
				],
				[
					20.555521647135492,
					-13.888880411783873
				],
				[
					21.111094156901117,
					-13.888880411783873
				],
				[
					21.666666666666742,
					-13.888880411783873
				],
				[
					22.22218831380212,
					-13.888880411783873
				],
				[
					22.777760823567746,
					-13.888880411783873
				],
				[
					23.33333333333337,
					-13.888880411783873
				],
				[
					23.88885498046875,
					-13.888880411783873
				],
				[
					24.444427490234375,
					-13.888880411783873
				],
				[
					25,
					-13.888880411783873
				],
				[
					25.555521647135492,
					-13.888880411783873
				],
				[
					26.111094156901117,
					-13.888880411783873
				],
				[
					27.22218831380212,
					-13.888880411783873
				],
				[
					27.777760823567746,
					-13.888880411783873
				],
				[
					28.33333333333337,
					-13.888880411783873
				],
				[
					28.88885498046875,
					-13.888880411783873
				],
				[
					29.444427490234375,
					-13.888880411783873
				],
				[
					30,
					-13.888880411783873
				],
				[
					30.555521647135492,
					-13.888880411783873
				],
				[
					31.111094156901117,
					-13.888880411783873
				],
				[
					31.666666666666742,
					-13.888880411783873
				],
				[
					32.22218831380212,
					-13.888880411783873
				],
				[
					32.777760823567746,
					-13.888880411783873
				],
				[
					33.33333333333337,
					-13.888880411783873
				],
				[
					33.88885498046875,
					-13.888880411783873
				],
				[
					34.444427490234375,
					-13.888880411783873
				],
				[
					35.55552164713549,
					-13.333333333333343
				],
				[
					36.11109415690112,
					-13.333333333333343
				],
				[
					36.66666666666674,
					-13.333333333333343
				],
				[
					37.22218831380212,
					-13.333333333333343
				],
				[
					37.777760823567746,
					-13.333333333333343
				],
				[
					38.88885498046875,
					-12.777786254882812
				],
				[
					39.444427490234375,
					-12.222213745117188
				],
				[
					40,
					-12.222213745117188
				],
				[
					40.55552164713549,
					-12.222213745117188
				],
				[
					41.66666666666674,
					-12.222213745117188
				],
				[
					42.22218831380212,
					-11.666666666666686
				],
				[
					42.777760823567746,
					-11.666666666666686
				],
				[
					43.88885498046875,
					-11.666666666666686
				],
				[
					45.55552164713549,
					-11.111119588216155
				],
				[
					46.11109415690112,
					-10.55554707845053
				],
				[
					47.22218831380212,
					-10.55554707845053
				],
				[
					48.33333333333337,
					-10
				],
				[
					48.88885498046875,
					-10
				],
				[
					49.444427490234375,
					-10
				],
				[
					50,
					-9.444452921549498
				],
				[
					50.55552164713549,
					-9.444452921549498
				],
				[
					51.66666666666674,
					-9.444452921549498
				],
				[
					52.22218831380212,
					-9.444452921549498
				],
				[
					52.777760823567746,
					-9.444452921549498
				],
				[
					53.33333333333337,
					-8.888880411783873
				],
				[
					53.33333333333337,
					-8.333333333333343
				],
				[
					53.88885498046875,
					-8.333333333333343
				],
				[
					54.444427490234375,
					-8.333333333333343
				],
				[
					55,
					-8.333333333333343
				],
				[
					55.55552164713549,
					-8.333333333333343
				],
				[
					56.11109415690112,
					-8.333333333333343
				],
				[
					56.66666666666674,
					-8.333333333333343
				],
				[
					57.22218831380212,
					-8.333333333333343
				],
				[
					58.33333333333337,
					-8.333333333333343
				],
				[
					58.88885498046875,
					-8.333333333333343
				],
				[
					60,
					-8.333333333333343
				],
				[
					60.55552164713549,
					-8.333333333333343
				],
				[
					61.11109415690112,
					-8.333333333333343
				],
				[
					61.66666666666674,
					-8.333333333333343
				],
				[
					62.22218831380212,
					-8.333333333333343
				],
				[
					62.777760823567746,
					-7.7777862548828125
				],
				[
					63.33333333333337,
					-7.7777862548828125
				],
				[
					63.33333333333337,
					-7.2222137451171875
				],
				[
					63.88885498046875,
					-6.666666666666686
				],
				[
					64.44442749023438,
					-6.666666666666686
				],
				[
					65,
					-6.111119588216155
				],
				[
					65.55552164713549,
					-5.55554707845053
				],
				[
					66.11109415690112,
					-5.55554707845053
				],
				[
					66.66666666666674,
					-5
				],
				[
					67.77776082356775,
					-4.444452921549498
				],
				[
					68.88885498046875,
					-3.888880411783873
				],
				[
					70,
					-3.333333333333343
				],
				[
					70,
					-2.7777862548828125
				],
				[
					70.55552164713549,
					-2.2222137451171875
				],
				[
					71.11109415690112,
					-1.6666666666666856
				],
				[
					71.66666666666674,
					-1.6666666666666856
				],
				[
					71.66666666666674,
					-1.1111195882161553
				],
				[
					72.22218831380212,
					-0.5555470784505303
				],
				[
					72.77776082356775,
					0
				],
				[
					73.33333333333337,
					0.5555470784505303
				],
				[
					73.88885498046875,
					1.1111195882161553
				],
				[
					74.44442749023438,
					1.6666666666666572
				],
				[
					75,
					1.6666666666666572
				],
				[
					75.55552164713549,
					2.2222137451171875
				],
				[
					76.11109415690112,
					2.7777862548828125
				],
				[
					76.66666666666674,
					2.7777862548828125
				],
				[
					76.66666666666674,
					3.333333333333343
				],
				[
					77.22218831380212,
					3.8888804117838447
				],
				[
					77.77776082356775,
					3.8888804117838447
				],
				[
					78.33333333333337,
					4.44445292154947
				],
				[
					78.33333333333337,
					4.44445292154947
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 161,
			"versionNonce": 442542972,
			"isDeleted": false,
			"id": "rKg5JKJnhToHJ9YMdBthj",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 357.8554611206056,
			"y": -54.42816242275018,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 68.888905843099,
			"height": 53.33333333333334,
			"seed": 1375146748,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.555572509765625,
					-0.5555725097656249
				],
				[
					1.11114501953125,
					-0.555572509765625
				],
				[
					1.6666666666666288,
					-0.5555725097656251
				],
				[
					2.2222391764322538,
					-0.5555725097656254
				],
				[
					3.3333333333333712,
					-0.555572509765625
				],
				[
					3.8889058430989962,
					-1.1111195882161264
				],
				[
					4.444478352864621,
					-1.1111195882161264
				],
				[
					5,
					-1.1111195882161258
				],
				[
					5.555572509765625,
					-1.1111195882161273
				],
				[
					6.11114501953125,
					-1.1111195882161273
				],
				[
					6.666666666666629,
					-1.1111195882161264
				],
				[
					8.333333333333371,
					-1.6666666666666572
				],
				[
					8.888905843098996,
					-1.6666666666666572
				],
				[
					9.444478352864621,
					-1.6666666666666572
				],
				[
					10,
					-2.222239176432282
				],
				[
					10.555572509765625,
					-2.222239176432282
				],
				[
					11.11114501953125,
					-2.7777862548828125
				],
				[
					11.666666666666629,
					-2.7777862548828125
				],
				[
					12.222239176432254,
					-2.7777862548828125
				],
				[
					12.777811686197879,
					-2.7777862548828125
				],
				[
					13.333333333333371,
					-2.7777862548828125
				],
				[
					13.888905843098996,
					-2.7777862548828125
				],
				[
					14.444478352864621,
					-2.7777862548828125
				],
				[
					15,
					-2.7777862548828125
				],
				[
					15.555572509765625,
					-2.7777862548828125
				],
				[
					16.11114501953125,
					-2.7777862548828125
				],
				[
					16.66666666666663,
					-3.3333333333333144
				],
				[
					17.222239176432254,
					-3.3333333333333144
				],
				[
					17.77781168619788,
					-3.3333333333333144
				],
				[
					18.33333333333337,
					-3.3333333333333144
				],
				[
					18.888905843098996,
					-3.3333333333333144
				],
				[
					20,
					-2.7777862548828125
				],
				[
					20.555572509765625,
					-2.222239176432282
				],
				[
					21.11114501953125,
					-2.222239176432282
				],
				[
					21.66666666666663,
					-2.222239176432282
				],
				[
					22.222239176432254,
					-1.6666666666666572
				],
				[
					22.77781168619788,
					-1.6666666666666572
				],
				[
					23.33333333333337,
					-1.1111195882161269
				],
				[
					23.888905843098996,
					-1.1111195882161269
				],
				[
					24.44447835286462,
					-0.555572509765625
				],
				[
					25,
					-0.555572509765625
				],
				[
					25.555572509765625,
					-0.555572509765625
				],
				[
					26.11114501953125,
					-0.555572509765625
				],
				[
					26.11114501953125,
					0
				],
				[
					27.222239176432254,
					0.5555470784505303
				],
				[
					27.77781168619788,
					1.1110941569010606
				],
				[
					28.888905843098996,
					1.1110941569010606
				],
				[
					29.44447835286462,
					1.6666666666666856
				],
				[
					29.44447835286462,
					2.2222137451171875
				],
				[
					30,
					2.777760823567718
				],
				[
					30.555572509765625,
					2.777760823567718
				],
				[
					31.11114501953125,
					2.777760823567718
				],
				[
					31.66666666666663,
					3.333333333333343
				],
				[
					32.222239176432254,
					3.333333333333343
				],
				[
					32.77781168619788,
					3.888880411783873
				],
				[
					32.77781168619788,
					4.444427490234375
				],
				[
					33.888905843098996,
					5
				],
				[
					34.44447835286462,
					5
				],
				[
					35.555572509765625,
					5.55554707845053
				],
				[
					36.11114501953125,
					6.111094156901061
				],
				[
					36.66666666666663,
					6.111094156901061
				],
				[
					37.222239176432254,
					6.111094156901061
				],
				[
					37.77781168619788,
					6.666666666666686
				],
				[
					38.888905843098996,
					7.2222137451171875
				],
				[
					40,
					7.777760823567718
				],
				[
					40.555572509765625,
					7.777760823567718
				],
				[
					41.11114501953125,
					8.333333333333343
				],
				[
					41.66666666666663,
					8.333333333333343
				],
				[
					42.222239176432254,
					8.888880411783873
				],
				[
					42.77781168619788,
					8.888880411783873
				],
				[
					43.33333333333337,
					9.444427490234375
				],
				[
					43.888905843098996,
					9.444427490234375
				],
				[
					44.44447835286462,
					9.444427490234375
				],
				[
					45,
					10
				],
				[
					46.11114501953125,
					10
				],
				[
					46.11114501953125,
					10.55554707845053
				],
				[
					46.66666666666663,
					11.11109415690106
				],
				[
					47.222239176432254,
					11.11109415690106
				],
				[
					47.77781168619788,
					11.11109415690106
				],
				[
					48.33333333333337,
					11.666666666666686
				],
				[
					48.888905843098996,
					11.666666666666686
				],
				[
					49.44447835286462,
					11.666666666666686
				],
				[
					50,
					11.666666666666686
				],
				[
					50.555572509765625,
					11.666666666666686
				],
				[
					51.11114501953125,
					12.222213745117188
				],
				[
					51.66666666666663,
					12.222213745117188
				],
				[
					52.222239176432254,
					12.777760823567718
				],
				[
					52.77781168619788,
					12.777760823567718
				],
				[
					52.77781168619788,
					13.333333333333343
				],
				[
					53.33333333333337,
					13.888880411783873
				],
				[
					53.888905843098996,
					14.444427490234403
				],
				[
					54.44447835286462,
					15.000000000000028
				],
				[
					54.44447835286462,
					15.55554707845053
				],
				[
					55,
					16.11109415690106
				],
				[
					55.555572509765625,
					16.11109415690106
				],
				[
					55.555572509765625,
					16.666666666666686
				],
				[
					56.11114501953125,
					17.777760823567718
				],
				[
					56.11114501953125,
					18.333333333333343
				],
				[
					56.11114501953125,
					18.888880411783873
				],
				[
					56.66666666666663,
					19.444427490234403
				],
				[
					57.222239176432254,
					19.444427490234403
				],
				[
					57.222239176432254,
					20.00000000000003
				],
				[
					58.33333333333337,
					21.11109415690106
				],
				[
					58.888905843098996,
					21.666666666666686
				],
				[
					59.44447835286462,
					22.222213745117216
				],
				[
					60,
					22.777760823567718
				],
				[
					60.555572509765625,
					23.333333333333343
				],
				[
					60.555572509765625,
					23.888880411783873
				],
				[
					61.11114501953125,
					24.444427490234403
				],
				[
					61.11114501953125,
					25.00000000000003
				],
				[
					61.66666666666663,
					25.55554707845053
				],
				[
					61.66666666666663,
					26.11109415690106
				],
				[
					62.22223917643237,
					27.222213745117216
				],
				[
					62.22223917643237,
					27.777760823567718
				],
				[
					62.22223917643237,
					28.333333333333343
				],
				[
					62.22223917643237,
					28.888880411783873
				],
				[
					62.77781168619799,
					29.444427490234403
				],
				[
					62.77781168619799,
					30.00000000000003
				],
				[
					63.33333333333337,
					30.55554707845053
				],
				[
					63.888905843098996,
					31.11109415690106
				],
				[
					63.888905843098996,
					31.666666666666686
				],
				[
					63.888905843098996,
					32.222213745117216
				],
				[
					64.44447835286462,
					32.222213745117216
				],
				[
					64.44447835286462,
					32.77776082356772
				],
				[
					65,
					33.33333333333334
				],
				[
					65,
					33.88888041178387
				],
				[
					65,
					35.00000000000003
				],
				[
					65.55557250976562,
					35.55554707845053
				],
				[
					65.55557250976562,
					36.11109415690106
				],
				[
					65.55557250976562,
					36.666666666666686
				],
				[
					65.55557250976562,
					37.222213745117216
				],
				[
					65.55557250976562,
					37.777760823567746
				],
				[
					65.55557250976562,
					38.33333333333337
				],
				[
					66.11114501953125,
					38.88888041178387
				],
				[
					66.11114501953125,
					40.00000000000003
				],
				[
					66.11114501953125,
					40.55554707845056
				],
				[
					66.66666666666674,
					41.11109415690106
				],
				[
					66.66666666666674,
					41.666666666666686
				],
				[
					66.66666666666674,
					42.222213745117216
				],
				[
					67.22223917643237,
					42.777760823567746
				],
				[
					67.22223917643237,
					43.33333333333337
				],
				[
					67.22223917643237,
					43.88888041178387
				],
				[
					67.22223917643237,
					44.4444274902344
				],
				[
					67.77781168619799,
					45.00000000000003
				],
				[
					67.77781168619799,
					45.55554707845056
				],
				[
					67.77781168619799,
					46.11109415690106
				],
				[
					68.33333333333337,
					47.222213745117216
				],
				[
					68.33333333333337,
					47.777760823567746
				],
				[
					68.33333333333337,
					48.33333333333337
				],
				[
					68.888905843099,
					48.33333333333337
				],
				[
					68.888905843099,
					48.88888041178387
				],
				[
					68.888905843099,
					49.4444274902344
				],
				[
					68.888905843099,
					50.00000000000003
				],
				[
					68.888905843099,
					50.00000000000003
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 141,
			"versionNonce": 971954756,
			"isDeleted": false,
			"id": "geP54HB7zl-iChw5EPMTv",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 421.18879445393895,
			"y": -20.539282010966303,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 26.66666666666663,
			"height": 71.66666666666669,
			"seed": 381356540,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772173,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-1.1102230246251565e-16,
					0.5555470784505303
				],
				[
					0.555572509765625,
					0.5555470784505303
				],
				[
					1.11114501953125,
					0.5555470784505304
				],
				[
					1.11114501953125,
					1.1111195882161553
				],
				[
					1.11114501953125,
					1.6666666666666574
				],
				[
					1.6666666666666294,
					1.6666666666666576
				],
				[
					1.6666666666666288,
					2.2222137451171875
				],
				[
					1.6666666666666283,
					3.333333333333343
				],
				[
					2.2222391764322538,
					3.888880411783873
				],
				[
					2.2222391764322538,
					4.444452921549498
				],
				[
					2.7778116861978797,
					5
				],
				[
					3.3333333333333712,
					6.111119588216155
				],
				[
					3.3333333333333712,
					6.666666666666686
				],
				[
					3.8889058430989962,
					7.2222137451171875
				],
				[
					3.8889058430989962,
					7.7777862548828125
				],
				[
					3.8889058430989962,
					8.333333333333343
				],
				[
					4.444478352864621,
					8.333333333333343
				],
				[
					4.444478352864621,
					8.888880411783873
				],
				[
					4.444478352864621,
					10
				],
				[
					4.444478352864621,
					10.55554707845053
				],
				[
					4.444478352864621,
					11.111119588216155
				],
				[
					5,
					11.666666666666686
				],
				[
					5.555572509765625,
					12.222213745117188
				],
				[
					5.555572509765625,
					12.777786254882812
				],
				[
					6.11114501953125,
					13.888880411783873
				],
				[
					6.666666666666629,
					14.444452921549498
				],
				[
					6.666666666666629,
					15
				],
				[
					7.222239176432254,
					15.55554707845053
				],
				[
					7.222239176432254,
					16.111119588216155
				],
				[
					7.777811686197879,
					16.666666666666686
				],
				[
					7.777811686197879,
					17.222213745117188
				],
				[
					7.777811686197879,
					17.777786254882812
				],
				[
					7.777811686197879,
					18.333333333333343
				],
				[
					7.777811686197879,
					18.888880411783873
				],
				[
					7.777811686197879,
					19.444452921549498
				],
				[
					7.777811686197879,
					20
				],
				[
					7.777811686197879,
					20.55554707845053
				],
				[
					7.777811686197879,
					21.111119588216155
				],
				[
					7.777811686197879,
					21.666666666666686
				],
				[
					7.777811686197879,
					22.222213745117188
				],
				[
					7.777811686197879,
					22.777786254882812
				],
				[
					7.777811686197879,
					23.333333333333343
				],
				[
					7.777811686197879,
					23.888880411783873
				],
				[
					7.777811686197879,
					24.444452921549498
				],
				[
					7.777811686197879,
					25
				],
				[
					7.777811686197879,
					25.55554707845053
				],
				[
					7.777811686197879,
					26.111119588216155
				],
				[
					7.777811686197879,
					26.666666666666686
				],
				[
					7.777811686197879,
					27.222213745117216
				],
				[
					7.777811686197879,
					27.77778625488284
				],
				[
					7.777811686197879,
					28.333333333333343
				],
				[
					7.777811686197879,
					28.888880411783873
				],
				[
					7.777811686197879,
					29.444452921549498
				],
				[
					7.777811686197879,
					30.00000000000003
				],
				[
					7.777811686197879,
					30.55554707845053
				],
				[
					7.777811686197879,
					31.111119588216155
				],
				[
					7.777811686197879,
					31.666666666666686
				],
				[
					7.777811686197879,
					32.222213745117216
				],
				[
					7.777811686197879,
					32.77778625488284
				],
				[
					7.777811686197879,
					33.33333333333334
				],
				[
					7.777811686197879,
					33.88888041178387
				],
				[
					7.777811686197879,
					34.4444529215495
				],
				[
					7.777811686197879,
					35.00000000000003
				],
				[
					7.777811686197879,
					35.55554707845053
				],
				[
					7.777811686197879,
					36.111119588216155
				],
				[
					7.777811686197879,
					36.666666666666686
				],
				[
					7.777811686197879,
					37.222213745117216
				],
				[
					7.777811686197879,
					37.77778625488284
				],
				[
					7.777811686197879,
					38.88888041178387
				],
				[
					7.777811686197879,
					39.4444529215495
				],
				[
					7.777811686197879,
					40.00000000000003
				],
				[
					7.777811686197879,
					40.55554707845053
				],
				[
					7.777811686197879,
					41.111119588216155
				],
				[
					7.222239176432254,
					41.111119588216155
				],
				[
					7.222239176432254,
					41.666666666666686
				],
				[
					7.222239176432254,
					42.222213745117216
				],
				[
					6.666666666666629,
					42.77778625488284
				],
				[
					6.666666666666629,
					43.33333333333334
				],
				[
					6.666666666666629,
					43.88888041178387
				],
				[
					6.11114501953125,
					44.4444529215495
				],
				[
					5.555572509765625,
					45.00000000000003
				],
				[
					5.555572509765625,
					45.55554707845053
				],
				[
					5,
					46.111119588216155
				],
				[
					5,
					47.222213745117216
				],
				[
					4.444478352864621,
					47.77778625488284
				],
				[
					4.444478352864621,
					48.33333333333334
				],
				[
					3.8889058430989962,
					48.88888041178387
				],
				[
					3.8889058430989962,
					49.4444529215495
				],
				[
					3.3333333333333712,
					50.00000000000003
				],
				[
					2.7778116861978788,
					50.55554707845056
				],
				[
					2.2222391764322538,
					51.111119588216184
				],
				[
					1.6666666666666288,
					51.666666666666686
				],
				[
					1.6666666666666288,
					52.222213745117216
				],
				[
					1.11114501953125,
					52.77778625488284
				],
				[
					1.11114501953125,
					53.33333333333337
				],
				[
					0.555572509765625,
					53.88888041178387
				],
				[
					0,
					53.88888041178387
				],
				[
					0,
					54.4444529215495
				],
				[
					-1.1110941569010038,
					55.55554707845056
				],
				[
					-1.6666666666667425,
					56.111119588216184
				],
				[
					-2.2221883138021212,
					56.666666666666686
				],
				[
					-2.2221883138021212,
					57.222213745117216
				],
				[
					-2.2221883138021212,
					57.77778625488284
				],
				[
					-2.7777608235677462,
					58.33333333333337
				],
				[
					-3.3333333333333712,
					58.88888041178387
				],
				[
					-3.88885498046875,
					59.4444529215495
				],
				[
					-4.444427490234375,
					60.00000000000003
				],
				[
					-5,
					60.55554707845056
				],
				[
					-5,
					61.111119588216184
				],
				[
					-5.5555216471354925,
					61.666666666666686
				],
				[
					-6.1110941569011175,
					62.222213745117244
				],
				[
					-6.6666666666667425,
					62.77778625488287
				],
				[
					-7.222188313802121,
					63.33333333333337
				],
				[
					-7.777760823567746,
					63.88888041178387
				],
				[
					-8.333333333333371,
					63.88888041178387
				],
				[
					-8.333333333333371,
					64.4444529215495
				],
				[
					-8.88885498046875,
					64.4444529215495
				],
				[
					-9.444427490234375,
					65.00000000000006
				],
				[
					-10,
					65.55554707845056
				],
				[
					-11.111094156901117,
					66.11111958821618
				],
				[
					-11.666666666666742,
					66.66666666666669
				],
				[
					-12.222188313802121,
					67.22221374511724
				],
				[
					-12.777760823567746,
					67.22221374511724
				],
				[
					-13.333333333333371,
					67.77778625488287
				],
				[
					-14.444427490234375,
					68.33333333333337
				],
				[
					-15,
					68.88888041178387
				],
				[
					-15.555521647135492,
					69.4444529215495
				],
				[
					-16.111094156901117,
					70.00000000000006
				],
				[
					-16.666666666666742,
					70.55554707845056
				],
				[
					-17.22218831380212,
					71.11111958821618
				],
				[
					-17.777760823567746,
					71.66666666666669
				],
				[
					-18.33333333333337,
					71.66666666666669
				],
				[
					-18.88885498046875,
					71.66666666666669
				],
				[
					-18.88885498046875,
					71.66666666666669
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 76,
			"versionNonce": 1152769020,
			"isDeleted": false,
			"id": "lU1j5qdEvzlUfP4Sb4u-n",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 424.5221277872723,
			"y": -22.20594867763296,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 3.888905843098883,
			"height": 28.333333333333343,
			"seed": 2011319292,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					0.5555470784505019
				],
				[
					3.3306690738754696e-16,
					1.1111195882161269
				],
				[
					-1.1102230246251565e-16,
					1.6666666666666572
				],
				[
					2.220446049250313e-16,
					2.2222137451171875
				],
				[
					-2.220446049250313e-16,
					3.3333333333333144
				],
				[
					0,
					4.44445292154947
				],
				[
					0,
					5
				],
				[
					-4.440892098500626e-16,
					6.111119588216155
				],
				[
					0.5555725097656241,
					7.2222137451171875
				],
				[
					0.555572509765625,
					7.7777862548828125
				],
				[
					0.5555725097656259,
					8.333333333333343
				],
				[
					0.555572509765625,
					8.888880411783845
				],
				[
					0.5555725097656259,
					9.44445292154947
				],
				[
					0.555572509765625,
					10
				],
				[
					0.555572509765625,
					10.55554707845053
				],
				[
					0.5555725097656259,
					11.111119588216154
				],
				[
					0.555572509765625,
					11.666666666666657
				],
				[
					1.1111450195312491,
					12.222213745117188
				],
				[
					1.6666666666666297,
					11.666666666666657
				],
				[
					1.6666666666666297,
					10.55554707845053
				],
				[
					1.6666666666666279,
					10
				],
				[
					1.6666666666666283,
					9.44445292154947
				],
				[
					1.6666666666666288,
					8.888880411783845
				],
				[
					1.6666666666666266,
					8.333333333333343
				],
				[
					1.6666666666666292,
					7.7777862548828125
				],
				[
					1.6666666666666283,
					7.2222137451171875
				],
				[
					1.6666666666666279,
					6.666666666666657
				],
				[
					1.6666666666666283,
					6.111119588216155
				],
				[
					1.6666666666666288,
					5.55554707845053
				],
				[
					1.666666666666629,
					5
				],
				[
					1.6666666666666288,
					5.55554707845053
				],
				[
					1.6666666666666283,
					6.111119588216155
				],
				[
					1.6666666666666279,
					6.666666666666657
				],
				[
					1.6666666666666283,
					7.2222137451171875
				],
				[
					1.6666666666666288,
					8.333333333333343
				],
				[
					2.2222391764322538,
					8.333333333333343
				],
				[
					2.7778116861978788,
					8.888880411783845
				],
				[
					3.3333333333332575,
					9.44445292154947
				],
				[
					3.3333333333332575,
					10
				],
				[
					3.3333333333332575,
					10.55554707845053
				],
				[
					3.3333333333332575,
					11.111119588216155
				],
				[
					3.3333333333332575,
					12.222213745117188
				],
				[
					3.8889058430988825,
					12.222213745117188
				],
				[
					3.8889058430988825,
					13.333333333333343
				],
				[
					3.8889058430988825,
					13.888880411783845
				],
				[
					3.8889058430988825,
					14.44445292154947
				],
				[
					3.8889058430988825,
					15
				],
				[
					3.8889058430988825,
					15.55554707845053
				],
				[
					3.8889058430988825,
					16.111119588216155
				],
				[
					3.8889058430988825,
					16.666666666666657
				],
				[
					3.8889058430988825,
					17.222213745117188
				],
				[
					3.8889058430988825,
					17.777786254882812
				],
				[
					3.8889058430988825,
					18.333333333333343
				],
				[
					3.8889058430988825,
					18.888880411783845
				],
				[
					3.8889058430988825,
					20
				],
				[
					3.8889058430988825,
					20.55554707845053
				],
				[
					3.8889058430988825,
					21.666666666666657
				],
				[
					3.8889058430988825,
					22.222213745117188
				],
				[
					3.3333333333332575,
					23.333333333333343
				],
				[
					3.3333333333332575,
					23.888880411783845
				],
				[
					3.3333333333332575,
					24.44445292154947
				],
				[
					2.7778116861978788,
					25
				],
				[
					2.2222391764322538,
					25.55554707845053
				],
				[
					2.2222391764322538,
					26.111119588216155
				],
				[
					1.6666666666666288,
					26.666666666666657
				],
				[
					1.6666666666666288,
					27.222213745117188
				],
				[
					1.11114501953125,
					27.777786254882812
				],
				[
					0.555572509765625,
					28.333333333333343
				],
				[
					0.555572509765625,
					28.333333333333343
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 98,
			"versionNonce": 1864561092,
			"isDeleted": false,
			"id": "UJxc1kvlaDah38BTXAris",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 387.8554611206056,
			"y": 40.57183757724988,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 21.66666666666663,
			"height": 24.444452921549498,
			"seed": 1827611900,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0.555572509765625,
					-0.5555725097656249
				],
				[
					0.5555725097656251,
					-1.1111195882161553
				],
				[
					1.11114501953125,
					-1.6666666666666856
				],
				[
					1.6666666666666288,
					-1.6666666666666856
				],
				[
					2.2222391764322538,
					-1.6666666666666852
				],
				[
					3.3333333333333712,
					-2.2222391764323106
				],
				[
					3.888905843098997,
					-2.7777862548828125
				],
				[
					4.444478352864621,
					-2.7777862548828125
				],
				[
					5,
					-3.333333333333343
				],
				[
					5.555572509765625,
					-3.333333333333342
				],
				[
					6.111145019531249,
					-3.3333333333333437
				],
				[
					6.66666666666663,
					-3.3333333333333424
				],
				[
					7.222239176432254,
					-3.888905843098968
				],
				[
					7.777811686197879,
					-3.888905843098968
				],
				[
					8.888905843098996,
					-4.444452921549498
				],
				[
					9.444478352864621,
					-4.444452921549498
				],
				[
					10,
					-5
				],
				[
					10.555572509765625,
					-5
				],
				[
					10.555572509765625,
					-5.555572509765625
				],
				[
					11.11114501953125,
					-6.111119588216155
				],
				[
					11.666666666666629,
					-6.111119588216155
				],
				[
					12.222239176432254,
					-6.666666666666686
				],
				[
					12.777811686197879,
					-7.222239176432311
				],
				[
					13.333333333333371,
					-7.222239176432311
				],
				[
					13.333333333333371,
					-7.7777862548828125
				],
				[
					13.888905843098996,
					-8.333333333333343
				],
				[
					14.444478352864621,
					-8.888905843098968
				],
				[
					14.444478352864621,
					-9.444452921549498
				],
				[
					15,
					-10
				],
				[
					15,
					-10.555572509765625
				],
				[
					15.555572509765625,
					-11.666666666666686
				],
				[
					15.555572509765625,
					-12.22223917643231
				],
				[
					15.555572509765625,
					-12.777786254882841
				],
				[
					16.66666666666663,
					-13.888905843098968
				],
				[
					16.66666666666663,
					-14.444452921549498
				],
				[
					16.66666666666663,
					-15.000000000000028
				],
				[
					17.222239176432254,
					-15.555572509765653
				],
				[
					17.77781168619788,
					-16.111119588216155
				],
				[
					18.33333333333337,
					-16.666666666666686
				],
				[
					18.888905843098996,
					-16.666666666666686
				],
				[
					18.888905843098996,
					-17.22223917643231
				],
				[
					19.44447835286462,
					-17.77778625488284
				],
				[
					19.44447835286462,
					-18.333333333333343
				],
				[
					20,
					-19.444452921549498
				],
				[
					20,
					-20.00000000000003
				],
				[
					20,
					-20.555572509765653
				],
				[
					20.555572509765625,
					-21.111119588216155
				],
				[
					20.555572509765625,
					-21.666666666666686
				],
				[
					21.11114501953125,
					-22.77778625488284
				],
				[
					21.11114501953125,
					-23.333333333333343
				],
				[
					21.11114501953125,
					-23.888905843098968
				],
				[
					21.66666666666663,
					-24.444452921549498
				],
				[
					21.66666666666663,
					-23.888905843098968
				],
				[
					21.11114501953125,
					-23.888905843098968
				],
				[
					20.555572509765625,
					-23.888905843098968
				],
				[
					20,
					-23.888905843098968
				],
				[
					19.44447835286462,
					-22.77778625488284
				],
				[
					18.33333333333337,
					-22.22223917643231
				],
				[
					18.33333333333337,
					-21.666666666666686
				],
				[
					17.77781168619788,
					-21.111119588216155
				],
				[
					17.77781168619788,
					-20.555572509765653
				],
				[
					17.222239176432254,
					-20.555572509765653
				],
				[
					16.66666666666663,
					-20.00000000000003
				],
				[
					16.66666666666663,
					-19.444452921549498
				],
				[
					16.11114501953125,
					-18.888905843098968
				],
				[
					16.11114501953125,
					-18.333333333333343
				],
				[
					15.555572509765625,
					-17.77778625488284
				],
				[
					15.555572509765625,
					-17.22223917643231
				],
				[
					15.555572509765625,
					-16.666666666666686
				],
				[
					15,
					-16.111119588216155
				],
				[
					15,
					-15.555572509765653
				],
				[
					15,
					-15.000000000000028
				],
				[
					14.444478352864621,
					-13.888905843098968
				],
				[
					13.888905843098996,
					-13.888905843098968
				],
				[
					13.888905843098996,
					-13.333333333333343
				],
				[
					13.333333333333371,
					-12.777786254882841
				],
				[
					13.333333333333371,
					-12.22223917643231
				],
				[
					12.777811686197879,
					-11.666666666666686
				],
				[
					12.777811686197879,
					-11.111119588216155
				],
				[
					12.222239176432254,
					-10.555572509765625
				],
				[
					12.222239176432254,
					-10
				],
				[
					11.666666666666629,
					-8.888905843098968
				],
				[
					11.666666666666629,
					-7.7777862548828125
				],
				[
					11.666666666666629,
					-7.222239176432311
				],
				[
					11.11114501953125,
					-6.666666666666686
				],
				[
					11.11114501953125,
					-6.111119588216155
				],
				[
					11.11114501953125,
					-5.555572509765625
				],
				[
					11.11114501953125,
					-5
				],
				[
					11.11114501953125,
					-3.888905843098968
				],
				[
					11.11114501953125,
					-3.333333333333343
				],
				[
					11.11114501953125,
					-2.7777862548828125
				],
				[
					11.11114501953125,
					-2.7777862548828125
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "freedraw",
			"version": 26,
			"versionNonce": 1041300604,
			"isDeleted": false,
			"id": "27__N2smHPfR8W0uXXo2C",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 320.63327280680346,
			"y": 40.016265067484255,
			"strokeColor": "#f08c00",
			"backgroundColor": "#ffec99",
			"width": 10.555572509765625,
			"height": 5.55554707845053,
			"seed": 725331068,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-0.555572509765625,
					0
				],
				[
					-1.11114501953125,
					-0.5555470784505304
				],
				[
					-1.6666666666666288,
					-0.5555470784505305
				],
				[
					-2.2222391764322538,
					-0.5555470784505301
				],
				[
					-2.7778116861978788,
					-1.1110941569010606
				],
				[
					-3.3333333333333703,
					-1.111094156901061
				],
				[
					-3.8889058430989967,
					-1.666666666666686
				],
				[
					-4.444478352864621,
					-1.6666666666666852
				],
				[
					-4.444478352864622,
					-2.2222137451171875
				],
				[
					-5,
					-2.2222137451171875
				],
				[
					-6.11114501953125,
					-2.777760823567718
				],
				[
					-6.666666666666629,
					-2.777760823567718
				],
				[
					-7.222239176432254,
					-2.7777608235677183
				],
				[
					-7.777811686197879,
					-3.333333333333343
				],
				[
					-8.333333333333371,
					-3.333333333333343
				],
				[
					-8.888905843098996,
					-3.888880411783873
				],
				[
					-9.444478352864621,
					-4.444427490234375
				],
				[
					-10,
					-5
				],
				[
					-10.555572509765625,
					-5.55554707845053
				],
				[
					-10.555572509765625,
					-5.55554707845053
				]
			],
			"lastCommittedPoint": null,
			"simulatePressure": true,
			"pressures": []
		},
		{
			"type": "text",
			"version": 1146,
			"versionNonce": 619190596,
			"isDeleted": false,
			"id": "rkwCvjqL",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 311.66334788004576,
			"y": 25.46759821359166,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 1715588220,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1219,
			"versionNonce": 271846652,
			"isDeleted": false,
			"id": "G5HheJ2U",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 408.10492197672534,
			"y": 15.327226230388444,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 180490236,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "8EumC7qzvC_yEFLer0mjd",
					"type": "arrow"
				}
			],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1157,
			"versionNonce": 1184649412,
			"isDeleted": false,
			"id": "0oCHACyb",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 326.9937769571941,
			"y": 37.827226230388476,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 1340581188,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1219,
			"versionNonce": 1017187708,
			"isDeleted": false,
			"id": "7J8jnrO4",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 395.8826828002931,
			"y": 32.27167915193803,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 472498556,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1166,
			"versionNonce": 601724996,
			"isDeleted": false,
			"id": "ONaHK26L",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 364.7715886433921,
			"y": 47.827226230388476,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 621580924,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1161,
			"versionNonce": 1150942716,
			"isDeleted": false,
			"id": "7b7qzrLk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 344.77153778076183,
			"y": 46.99389289705516,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 312728260,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1186,
			"versionNonce": 1753870276,
			"isDeleted": false,
			"id": "UufpOxj7",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 381.4382553100587,
			"y": 44.77170458325304,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 1475695612,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1227,
			"versionNonce": 824237692,
			"isDeleted": false,
			"id": "qqKUtcfZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 309.2160669962566,
			"y": -12.172773769611567,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 1999952452,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1187,
			"versionNonce": 348084036,
			"isDeleted": false,
			"id": "9aXb5lAB",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 305.32711029052734,
			"y": 1.993892897055158,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 1219369284,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1188,
			"versionNonce": 452439804,
			"isDeleted": false,
			"id": "uAgXvaTy",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 335.8826828002931,
			"y": -48.0061071029449,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.949981689453125,
			"height": 16.044744340858202,
			"seed": 237213764,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.951951600746263,
			"fontFamily": 2,
			"text": "kv",
			"rawText": "kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "arrow",
			"version": 33,
			"versionNonce": 1960562372,
			"isDeleted": false,
			"id": "BD4BtqLYvNF4NljPkkmEY",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 248.41103363037118,
			"y": 48.905170910583195,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"width": 16.96009862337783,
			"height": 44.14370316230738,
			"seed": 195108604,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "jNy7B1Zj",
				"focus": -0.1525362477533831,
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
					16.96009862337783,
					-44.14370316230738
				]
			]
		},
		{
			"type": "arrow",
			"version": 291,
			"versionNonce": 849375100,
			"isDeleted": false,
			"id": "9cJsHoruYhzk3LOcA-oAE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 267.4838591938454,
			"y": -7.205948677632939,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"width": 43.14936275032784,
			"height": 25.55559794108077,
			"seed": 1116591740,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "jNy7B1Zj",
				"focus": 0.05209942185988473,
				"gap": 1.3145102083579356
			},
			"endBinding": {
				"elementId": "IHde9w4x2fT9YD6xtOZZL",
				"focus": 0.6231489833879374,
				"gap": 2.2318346711772676
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
					14.759073202952026,
					-24.16669601049699
				],
				[
					43.14936275032784,
					-25.55559794108077
				]
			]
		},
		{
			"type": "text",
			"version": 1189,
			"versionNonce": 427644484,
			"isDeleted": false,
			"id": "rs6gGZGT",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 444.3298848470055,
			"y": -61.44697283206797,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 161.99134826660156,
			"height": 24.394268676053322,
			"seed": 980756092,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 10.606203772197098,
			"fontFamily": 2,
			"text": "分区：按Partitioner的getPartition()\n排序：按key的compareTo",
			"rawText": "分区：按Partitioner的getPartition()\n排序：按key的compareTo",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "分区：按Partitioner的getPartition()\n排序：按key的compareTo",
			"lineHeight": 1.15,
			"baseline": 22
		},
		{
			"type": "text",
			"version": 101,
			"versionNonce": 1214062588,
			"isDeleted": false,
			"id": "PxyvinCz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 514.5221786499026,
			"y": -27.20592324631781,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"width": 53.40234375,
			"height": 15.624122023809537,
			"seed": 1986509564,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "xC3lI5UzIytl2x454nhz0",
					"type": "arrow"
				}
			],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 13.020101686507948,
			"fontFamily": 3,
			"text": "spiller",
			"rawText": "spiller",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "spiller",
			"lineHeight": 1.2,
			"baseline": 12
		},
		{
			"type": "arrow",
			"version": 58,
			"versionNonce": 1333915076,
			"isDeleted": false,
			"id": "xC3lI5UzIytl2x454nhz0",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 508.41108449300145,
			"y": -19.98370950120062,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"width": 75.55557250976562,
			"height": 0.5555470784505303,
			"seed": 992470140,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "PxyvinCz",
				"focus": 0.10378010500867883,
				"gap": 6.111094156901174
			},
			"endBinding": {
				"elementId": "IHde9w4x2fT9YD6xtOZZL",
				"focus": -0.36352675028917464,
				"gap": 4.873468340564472
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
					-75.55557250976562,
					0.5555470784505303
				]
			]
		},
		{
			"type": "arrow",
			"version": 586,
			"versionNonce": 1412415612,
			"isDeleted": false,
			"id": "8EumC7qzvC_yEFLer0mjd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 432.30044858969546,
			"y": 3.543185749898605,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"width": 80.55506339354025,
			"height": 25.16207810488482,
			"seed": 975594236,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "G5HheJ2U",
				"focus": -2.4688972575875314,
				"gap": 11.42205531980522
			},
			"endBinding": {
				"elementId": "pfZk2ner",
				"focus": -0.3896572124967271,
				"gap": 2.3077061971029025
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
					80.55506339354025,
					25.16207810488482
				]
			]
		},
		{
			"type": "rectangle",
			"version": 62,
			"versionNonce": 403150148,
			"isDeleted": false,
			"id": "_ptLyDZRsoU1zC5XHX4mF",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 511.74446868896496,
			"y": -30.539256579651152,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 63.33333333333337,
			"height": 23.888880411783873,
			"seed": 719285884,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1230,
			"versionNonce": 2111459580,
			"isDeleted": false,
			"id": "pfZk2ner",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 515.1632181803386,
			"y": 22.997492805139117,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 101.06417846679688,
			"height": 23.116980134224885,
			"seed": 47167300,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "8EumC7qzvC_yEFLer0mjd",
					"type": "arrow"
				}
			],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 10.050860927923864,
			"fontFamily": 2,
			"text": "a,1 a,1 d,1\n kv kv   kv   kv kv kv kv",
			"rawText": "a,1 a,1 d,1\n kv kv   kv   kv kv kv kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "a,1 a,1 d,1\n kv kv   kv   kv kv kv kv",
			"lineHeight": 1.15,
			"baseline": 21
		},
		{
			"type": "rectangle",
			"version": 228,
			"versionNonce": 543968452,
			"isDeleted": false,
			"id": "CNoXP4agXAQh_1ZftmkSQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 510.07770029703795,
			"y": 52.79407675368222,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 112.77781168619799,
			"height": 17.777786254882784,
			"seed": 1091377604,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1206,
			"versionNonce": 618582396,
			"isDeleted": false,
			"id": "XCDT7cHE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 516.513122558594,
			"y": 54.29110833370525,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 98.27197265625,
			"height": 11.558490067112443,
			"seed": 1678418372,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 10.050860927923864,
			"fontFamily": 2,
			"text": "kv kv   kv   kv kv kv kv",
			"rawText": "kv kv   kv   kv kv kv kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "kv kv   kv   kv kv kv kv",
			"lineHeight": 1.15,
			"baseline": 9
		},
		{
			"type": "rectangle",
			"version": 106,
			"versionNonce": 978195524,
			"isDeleted": false,
			"id": "6rfWwVAs_f8PE7pC-AhjW",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 509.5221277872723,
			"y": 20.01629049879938,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 110,
			"height": 26.111119588216184,
			"seed": 877738820,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1185,
			"versionNonce": 1753170428,
			"isDeleted": false,
			"id": "CinskEHp",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 564.9936040242515,
			"y": 9.63429031157347,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 75.50999450683594,
			"height": 9.652906217550814,
			"seed": 1649412476,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "溢出文件，本地磁盘",
			"rawText": "溢出文件，本地磁盘",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "溢出文件，本地磁盘",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "text",
			"version": 1272,
			"versionNonce": 1401613252,
			"isDeleted": false,
			"id": "6PidAHLk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 594.5449422200522,
			"y": -8.143495943309372,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 138.9060821533203,
			"height": 9.652906217550814,
			"seed": 1914307396,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "区号小的在前面，同区中的按key有序",
			"rawText": "区号小的在前面，同区中的按key有序",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "区号小的在前面，同区中的按key有序",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "text",
			"version": 1257,
			"versionNonce": 800957052,
			"isDeleted": false,
			"id": "7DcEXl2R",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 487.32270304362,
			"y": 34.63426488025837,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 21.44610595703125,
			"height": 9.652906217550814,
			"seed": 469051716,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "0号区",
			"rawText": "0号区",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0号区",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "rectangle",
			"version": 96,
			"versionNonce": 468778180,
			"isDeleted": false,
			"id": "nwPyY5f2PrdrsMxCcMYBU",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 771.0222295125329,
			"y": -23.16147032476845,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#868e96",
			"width": 92,
			"height": 30,
			"seed": 423871612,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "yKhCetOs"
				},
				{
					"id": "FFJEpH_4fhqPR5fGl1cy0",
					"type": "arrow"
				},
				{
					"id": "slp0hXN2p-x32KiaL-deh",
					"type": "arrow"
				}
			],
			"updated": 1694315283956,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 21,
			"versionNonce": 828109564,
			"isDeleted": false,
			"id": "yKhCetOs",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 776.1316045125329,
			"y": -17.36147032476845,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 81.78125,
			"height": 18.4,
			"seed": 533707844,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "kv kv kv kv ",
			"rawText": "kv kv kv kv ",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "nwPyY5f2PrdrsMxCcMYBU",
			"originalText": "kv kv kv kv ",
			"lineHeight": 1.15,
			"baseline": 15
		},
		{
			"type": "rectangle",
			"version": 145,
			"versionNonce": 416139972,
			"isDeleted": false,
			"id": "JU1HVuZYQNzCW-mJALGXr",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 861.022229512533,
			"y": -23.317042834534078,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffec99",
			"width": 92,
			"height": 30,
			"seed": 988771268,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "GEbYcxIi"
				}
			],
			"updated": 1694313772174,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 75,
			"versionNonce": 2078028668,
			"isDeleted": false,
			"id": "GEbYcxIi",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 866.131604512533,
			"y": -17.517042834534077,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 81.78125,
			"height": 18.4,
			"seed": 131357508,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "kv kv kv kv ",
			"rawText": "kv kv kv kv ",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "JU1HVuZYQNzCW-mJALGXr",
			"originalText": "kv kv kv kv ",
			"lineHeight": 1.15,
			"baseline": 15
		},
		{
			"type": "rectangle",
			"version": 241,
			"versionNonce": 1708118596,
			"isDeleted": false,
			"id": "pKavbySpTD0sLBqqnVd5t",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 948.2444178263353,
			"y": -23.87258991298458,
			"strokeColor": "#1971c2",
			"backgroundColor": "#a5d8ff",
			"width": 54,
			"height": 29,
			"seed": 1697679228,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "FgCqCyfQ"
				}
			],
			"updated": 1694313772174,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 176,
			"versionNonce": 2105566204,
			"isDeleted": false,
			"id": "FgCqCyfQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 954.7991053263353,
			"y": -18.57258991298458,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 40.890625,
			"height": 18.4,
			"seed": 1630156796,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "kv kv ",
			"rawText": "kv kv ",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "pKavbySpTD0sLBqqnVd5t",
			"originalText": "kv kv ",
			"lineHeight": 1.15,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 1261,
			"versionNonce": 1680140740,
			"isDeleted": false,
			"id": "KLjpsL9d",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 486.8546854654952,
			"y": 50.745409899789536,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 21.44610595703125,
			"height": 9.652906217550814,
			"seed": 982451964,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "1号区",
			"rawText": "1号区",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1号区",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "text",
			"version": 1278,
			"versionNonce": 206210172,
			"isDeleted": false,
			"id": "AuODT0gm",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 809.0769246419276,
			"y": 14.078743233122822,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 21.44610595703125,
			"height": 9.652906217550814,
			"seed": 1658792316,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "0号区",
			"rawText": "0号区",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0号区",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "text",
			"version": 1273,
			"versionNonce": 384504132,
			"isDeleted": false,
			"id": "YjY8A8Nc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 899.0769246419276,
			"y": 11.856529488005663,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 21.44610595703125,
			"height": 9.652906217550814,
			"seed": 2072509052,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "1号区",
			"rawText": "1号区",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1号区",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "text",
			"version": 1261,
			"versionNonce": 1845269756,
			"isDeleted": false,
			"id": "I7HiG0Mo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 964.3546854654952,
			"y": 12.412076566456165,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 21.44610595703125,
			"height": 9.652906217550814,
			"seed": 1794149244,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "2号区",
			"rawText": "2号区",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "2号区",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "text",
			"version": 1116,
			"versionNonce": 1837123780,
			"isDeleted": false,
			"id": "egwNsdPY",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 775.7188288370774,
			"y": -42.55805427331154,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 60.99998474121094,
			"height": 14.037595387320575,
			"seed": 1703568580,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "FFJEpH_4fhqPR5fGl1cy0",
					"type": "arrow"
				}
			],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "分区且有序",
			"rawText": "分区且有序",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "分区且有序",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "line",
			"version": 28,
			"versionNonce": 1612223868,
			"isDeleted": false,
			"id": "UMFsmRPTT9B1cMaD7QAOU",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 622.2999776204431,
			"y": 33.90519634189826,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#a5d8ff",
			"width": 38.33333333333337,
			"height": 0.5555470784505303,
			"seed": 1111483644,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772174,
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
					38.33333333333337,
					-0.5555470784505303
				]
			]
		},
		{
			"type": "line",
			"version": 45,
			"versionNonce": 998412356,
			"isDeleted": false,
			"id": "v49Nzl837PZPd4eTCcr1a",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 626.7444051106776,
			"y": 61.68298259678107,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#a5d8ff",
			"width": 34.999999999999886,
			"height": 0.5555470784505019,
			"seed": 1366440956,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772174,
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
					34.999999999999886,
					0.5555470784505019
				]
			]
		},
		{
			"type": "line",
			"version": 33,
			"versionNonce": 534808060,
			"isDeleted": false,
			"id": "ZxuDyOGt0qZxxm-ILMGX0",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 663.9666442871098,
			"y": 61.12743551833057,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#a5d8ff",
			"width": 0.555572509765625,
			"height": 29.444452921549498,
			"seed": 1975257084,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772174,
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
					-0.555572509765625,
					-29.444452921549498
				]
			]
		},
		{
			"type": "arrow",
			"version": 135,
			"versionNonce": 818931652,
			"isDeleted": false,
			"id": "FFJEpH_4fhqPR5fGl1cy0",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 664.5222167968755,
			"y": 44.460768851663886,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#a5d8ff",
			"width": 100.55557250976574,
			"height": 65.55554707845056,
			"seed": 759135684,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "egwNsdPY",
				"focus": -1.2330056064930786,
				"gap": 10.64103953043616
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
					25,
					-60.00000000000003
				],
				[
					100.55557250976574,
					-65.55554707845056
				]
			]
		},
		{
			"type": "text",
			"version": 1163,
			"versionNonce": 1918264956,
			"isDeleted": false,
			"id": "iP4BLjED",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 689.0221735636399,
			"y": -44.22469550866312,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 58.98051452636719,
			"height": 14.037595387320575,
			"seed": 622019140,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "Merge合并",
			"rawText": "Merge合并",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Merge合并",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "rectangle",
			"version": 49,
			"versionNonce": 1367756612,
			"isDeleted": false,
			"id": "JPK3NjxwAqTV9BygcQwFE",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 680.6333109537765,
			"y": -49.42813699143514,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 75.00000000000011,
			"height": 23.333333333333343,
			"seed": 1170058436,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1230,
			"versionNonce": 742143740,
			"isDeleted": false,
			"id": "LJlciteX",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 675.8652928670252,
			"y": -74.22472093997824,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 101.68644714355469,
			"height": 14.037595387320575,
			"seed": 1069358788,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "Combiner局部聚合",
			"rawText": "Combiner局部聚合",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Combiner局部聚合",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "rectangle",
			"version": 63,
			"versionNonce": 1845741252,
			"isDeleted": false,
			"id": "x_kBZfkc4B2SR336h3aAv",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 663.4110717773442,
			"y": -82.20591053066039,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 128.888905843099,
			"height": 27.77777353922528,
			"seed": 761291388,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1344,
			"versionNonce": 974387068,
			"isDeleted": false,
			"id": "oT6v8B4N",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 859.2358932495126,
			"y": 37.6898373900238,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 50.339996337890625,
			"height": 9.652906217550814,
			"seed": 1543436996,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "分区索引文件",
			"rawText": "分区索引文件",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "分区索引文件",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "rectangle",
			"version": 44,
			"versionNonce": 451300932,
			"isDeleted": false,
			"id": "T4KZQAt5dh91rBmaTP7Zo",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 848.4111735026049,
			"y": 34.46076885166383,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 73.88885498046875,
			"height": 15.555547078450502,
			"seed": 54073084,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1402,
			"versionNonce": 673163260,
			"isDeleted": false,
			"id": "Pbz2Z1lv",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 875.3469874064137,
			"y": -82.61975403987147,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 176.1531219482422,
			"height": 9.807484277355792,
			"seed": 613818692,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false,
			"fontSize": 8.52824719770069,
			"fontFamily": 2,
			"text": "纳入nodeManager 的web程序document目录中",
			"rawText": "纳入nodeManager 的web程序document目录中",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "纳入nodeManager 的web程序document目录中",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "line",
			"version": 85,
			"versionNonce": 688645572,
			"isDeleted": false,
			"id": "SEpXiB8HeVL2zbWwPURGy",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 876.7445068359382,
			"y": -71.65035073655235,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 178.88885498046898,
			"height": 1.6666666666666856,
			"seed": 1166325444,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313772174,
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
					178.88885498046898,
					-1.6666666666666856
				]
			]
		},
		{
			"type": "rectangle",
			"version": 208,
			"versionNonce": 711018620,
			"isDeleted": false,
			"id": "xTmn2hFQZiOHgzfnwc30I",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 68.37973961463376,
			"y": -92.60825032010075,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 1022.5641338641824,
			"height": 184.6153846153847,
			"seed": 894354684,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313772174,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 352,
			"versionNonce": 1312017988,
			"isDeleted": false,
			"id": "HLg769fAceiqaShhLpK7F",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 97.0675389906944,
			"y": 229.0278337747452,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 831.8824821920957,
			"height": 231.29418945312506,
			"seed": 1313753796,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694314923581,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 151,
			"versionNonce": 784189948,
			"isDeleted": false,
			"id": "aUVTDSwkJokgi5bE0tkzH",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 125.30283310834142,
			"y": 236.24363972603192,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#868e96",
			"width": 138.82352941176464,
			"height": 34.50977998621329,
			"seed": 290766660,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313780747,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 135,
			"versionNonce": 1102890052,
			"isDeleted": false,
			"id": "f3NwfHeW",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 135.49893547506935,
			"y": 237.8121714355907,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 114.8836669921875,
			"height": 32.7907045392489,
			"seed": 1326678396,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "slp0hXN2p-x32KiaL-deh",
					"type": "arrow"
				}
			],
			"updated": 1694315283956,
			"link": null,
			"locked": false,
			"fontSize": 14.256828060543,
			"fontFamily": 2,
			"text": "a,1 a,1 a,1 d,1 g,1\nkv   kv   kv  kv  kv",
			"rawText": "a,1 a,1 a,1 d,1 g,1\nkv   kv   kv  kv  kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "a,1 a,1 a,1 d,1 g,1\nkv   kv   kv  kv  kv",
			"lineHeight": 1.15,
			"baseline": 29
		},
		{
			"type": "rectangle",
			"version": 182,
			"versionNonce": 469532740,
			"isDeleted": false,
			"id": "4mrEiPlk2MviK-S7T2_3s",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 126.47930369657678,
			"y": 308.00823789928575,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#868e96",
			"width": 138.82352941176464,
			"height": 34.50977998621329,
			"seed": 1494683132,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694313800923,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 162,
			"versionNonce": 2100225660,
			"isDeleted": false,
			"id": "UsamHWyk",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 138.07584695255105,
			"y": 310.4363791383928,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 114.8836669921875,
			"height": 32.7907045392489,
			"seed": 1419069180,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313929461,
			"link": null,
			"locked": false,
			"fontSize": 14.256828060543,
			"fontFamily": 2,
			"text": "a,1 a,1 g,1 g,1 g,1\nkv   kv   kv  kv  kv",
			"rawText": "a,1 a,1 g,1 g,1 g,1\nkv   kv   kv  kv  kv",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "a,1 a,1 g,1 g,1 g,1\nkv   kv   kv  kv  kv",
			"lineHeight": 1.15,
			"baseline": 29
		},
		{
			"type": "line",
			"version": 44,
			"versionNonce": 587004156,
			"isDeleted": false,
			"id": "YqwNnukoMUnDt2fcP7Dhd",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 270.4009201947385,
			"y": 252.71408434919385,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#868e96",
			"width": 50.19603056066171,
			"height": 0,
			"seed": 1610731460,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313824459,
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
					50.19603056066171,
					0
				]
			]
		},
		{
			"type": "line",
			"version": 44,
			"versionNonce": 1752805316,
			"isDeleted": false,
			"id": "4o4It5Dv73JaFm4-9jJDa",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 270.4008483886723,
			"y": 325.65526081978203,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#868e96",
			"width": 49.411836511948536,
			"height": 0,
			"seed": 1993272388,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313841035,
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
					49.411836511948536,
					0
				]
			]
		},
		{
			"type": "line",
			"version": 37,
			"versionNonce": 1900682236,
			"isDeleted": false,
			"id": "yYaIF2kSzRO5UZ9tEmVvt",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 321.3812884162458,
			"y": 253.49842201003946,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#868e96",
			"width": 0.7843376608456083,
			"height": 71.37257295496318,
			"seed": 1406608124,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694313837554,
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
					0.7843376608456083,
					71.37257295496318
				]
			]
		},
		{
			"type": "arrow",
			"version": 187,
			"versionNonce": 138708804,
			"isDeleted": false,
			"id": "AvaR1EX6iw3uSiojoZppR",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 324.518567253562,
			"y": 284.08665730415703,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#868e96",
			"width": 47.95901818248109,
			"height": 0.5788893197976677,
			"seed": 11999044,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694314041592,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "CfKIjZFntjVuH5SemUICm",
				"gap": 4.982158288107326,
				"focus": 0.22726733625054352
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
					47.95901818248109,
					-0.5788893197976677
				]
			]
		},
		{
			"type": "text",
			"version": 1323,
			"versionNonce": 869102276,
			"isDeleted": false,
			"id": "qiFdVFTq",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 133.01117661420085,
			"y": 276.90733482497706,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 21.44610595703125,
			"height": 9.652906217550814,
			"seed": 1266156740,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313869426,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "0号区",
			"rawText": "0号区",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0号区",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "text",
			"version": 1300,
			"versionNonce": 2133972476,
			"isDeleted": false,
			"id": "vP6Xmdhc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 135.7562507180611,
			"y": 352.20130885990375,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 21.44610595703125,
			"height": 9.652906217550814,
			"seed": 927928444,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694313866163,
			"link": null,
			"locked": false,
			"fontSize": 8.393831493522448,
			"fontFamily": 2,
			"text": "0号区",
			"rawText": "0号区",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0号区",
			"lineHeight": 1.15,
			"baseline": 7
		},
		{
			"type": "rectangle",
			"version": 328,
			"versionNonce": 1906147068,
			"isDeleted": false,
			"id": "CfKIjZFntjVuH5SemUICm",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 377.4597437241504,
			"y": 269.1845648753887,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#868e96",
			"width": 265.09808708639713,
			"height": 33.7255141314339,
			"seed": 346250364,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "AvaR1EX6iw3uSiojoZppR",
					"type": "arrow"
				}
			],
			"updated": 1694314041592,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 251,
			"versionNonce": 491425404,
			"isDeleted": false,
			"id": "Lep36iK2",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 380.8021760828359,
			"y": 277.8873355951945,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 257.4949951171875,
			"height": 16.39535226962445,
			"seed": 121875068,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694314033844,
			"link": null,
			"locked": false,
			"fontSize": 14.256828060543,
			"fontFamily": 2,
			"text": "a,1 a,1 a,1 a,1 a,1 d,1 d,1 g,1 g,1 g,1 g,1",
			"rawText": "a,1 a,1 a,1 a,1 a,1 d,1 d,1 g,1 g,1 g,1 g,1",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "a,1 a,1 a,1 a,1 a,1 d,1 d,1 g,1 g,1 g,1 g,1",
			"lineHeight": 1.15,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 1188,
			"versionNonce": 1525705862,
			"isDeleted": false,
			"id": "IDQaPuWF",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 370.0969583847949,
			"y": 244.91094899468803,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 48.79998779296875,
			"height": 14.037595387320575,
			"seed": 819273156,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694354547248,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "合并排序",
			"rawText": "合并排序",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "合并排序",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "text",
			"version": 1197,
			"versionNonce": 1230428028,
			"isDeleted": false,
			"id": "kXkiey3m",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 506.3499010871443,
			"y": 249.61683134762907,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 24.399993896484375,
			"height": 14.037595387320575,
			"seed": 532416964,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694314088473,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "文件",
			"rawText": "文件",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "文件",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "text",
			"version": 1124,
			"versionNonce": 1874356348,
			"isDeleted": false,
			"id": "GAtWD4i1",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 518.3625820384311,
			"y": 385.54087111361525,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 191.8637237548828,
			"height": 70.18797693660288,
			"seed": 509735364,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694314309664,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "WcReduce类\n        reduce(k,迭代器,context)         \n                    v.values\n            context.write(k,v)\n",
			"rawText": "WcReduce类\n        reduce(k,迭代器,context)         \n                    v.values\n            context.write(k,v)\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "WcReduce类\n        reduce(k,迭代器,context)         \n                    v.values\n            context.write(k,v)\n",
			"lineHeight": 1.15,
			"baseline": 67
		},
		{
			"type": "rectangle",
			"version": 88,
			"versionNonce": 1442996220,
			"isDeleted": false,
			"id": "fGLIwpKIQuIzVs8If2ecp",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 503.28812722598843,
			"y": 372.64701568823335,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 178.66668701171872,
			"height": 73.33331298828125,
			"seed": 1913518332,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694314345200,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 70,
			"versionNonce": 235974468,
			"isDeleted": false,
			"id": "WHM4HrSfFG_tAmHHBX0IS",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 380.62150124942593,
			"y": 277.31367218237403,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 114.6666259765625,
			"height": 19.333343505859375,
			"seed": 2066546812,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "LOot0zA40ld41Z2vZdzAf",
					"type": "arrow"
				}
			],
			"updated": 1694314444023,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 74,
			"versionNonce": 445904124,
			"isDeleted": false,
			"id": "sgHzy8etDe1x-_apyOqPQ",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 499.2881882611447,
			"y": 275.98032867651466,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"width": 46.66668701171875,
			"height": 22,
			"seed": 539335620,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694314371584,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 73,
			"versionNonce": 2052251204,
			"isDeleted": false,
			"id": "t5enN5DX3g4XdmisPNd30",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 547.9548752728634,
			"y": 274.6469851706553,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 92.6666259765625,
			"height": 22.666656494140625,
			"seed": 1120138620,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694314383808,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1254,
			"versionNonce": 1811196540,
			"isDeleted": false,
			"id": "TUN9u1MD",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 550.0214493695431,
			"y": 306.2948439711355,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 175.6011962890625,
			"height": 14.037595387320575,
			"seed": 295913724,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694314427415,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "分组比较器GroupingComparator",
			"rawText": "分组比较器GroupingComparator",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "分组比较器GroupingComparator",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "arrow",
			"version": 118,
			"versionNonce": 1903387260,
			"isDeleted": false,
			"id": "LOot0zA40ld41Z2vZdzAf",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 424.62150124942593,
			"y": 300.6469851706553,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 160.6666259765625,
			"height": 98.00003051757807,
			"seed": 1634859772,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694314514719,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WHM4HrSfFG_tAmHHBX0IS",
				"focus": 0.4883640062795674,
				"gap": 3.999969482421875
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
					160.6666259765625,
					98.00003051757807
				]
			]
		},
		{
			"type": "arrow",
			"version": 98,
			"versionNonce": 1214534724,
			"isDeleted": false,
			"id": "6eqXMYT4CuUErjxZVlKb4",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 397.28812722598843,
			"y": 290.6469851706553,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 190.66668701171875,
			"height": 126.66665649414057,
			"seed": 26638588,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694314507912,
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
					190.66668701171875,
					126.66665649414057
				]
			]
		},
		{
			"type": "text",
			"version": 1264,
			"versionNonce": 37578108,
			"isDeleted": false,
			"id": "PzzOZLNU",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 435.0881913129025,
			"y": 346.29490500629186,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 60.99998474121094,
			"height": 14.037595387320575,
			"seed": 382079556,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694314536207,
			"link": null,
			"locked": false,
			"fontSize": 12.206604684626589,
			"fontFamily": 2,
			"text": "每迭代一下",
			"rawText": "每迭代一下",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "每迭代一下",
			"lineHeight": 1.15,
			"baseline": 11
		},
		{
			"type": "rectangle",
			"version": 232,
			"versionNonce": 1963285628,
			"isDeleted": false,
			"id": "hN7Jwmym21tet8J6rAfOo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 785.9548142377073,
			"y": 267.75805135310964,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 96,
			"height": 33,
			"seed": 1075406404,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694314900583,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 127,
			"versionNonce": 1497494652,
			"isDeleted": false,
			"id": "NFZ7Ijxo",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 788.6214402142698,
			"y": 279.313672182374,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 89.31257629394531,
			"height": 12.845373703477767,
			"seed": 1202504516,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694314915358,
			"link": null,
			"locked": false,
			"fontSize": 11.169890176937189,
			"fontFamily": 2,
			"text": "TextOutputFormat",
			"rawText": "TextOutputFormat",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "TextOutputFormat",
			"lineHeight": 1.15,
			"baseline": 10
		},
		{
			"type": "text",
			"version": 1274,
			"versionNonce": 1189524604,
			"isDeleted": false,
			"id": "JDQqEXoZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 821.2042344037229,
			"y": 309.71476204568324,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 84.67988586425781,
			"height": 20.265566630831326,
			"seed": 1248021116,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "2z6hR8pNb_j0LWvGWcmI1",
					"type": "arrow"
				}
			],
			"updated": 1694315238885,
			"link": null,
			"locked": false,
			"fontSize": 8.811115926448403,
			"fontFamily": 2,
			"text": "LineRecordWriter\n                    write(k,v)",
			"rawText": "LineRecordWriter\n                    write(k,v)",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "LineRecordWriter\n                    write(k,v)",
			"lineHeight": 1.15,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 97,
			"versionNonce": 1901006788,
			"isDeleted": false,
			"id": "mv-tljY5ZxG3ul9yI64VJ",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 1083.9548752728633,
			"y": 219.31367218237403,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 167.3333740234375,
			"height": 237.3333129882812,
			"seed": 1613873348,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1694315105408,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 170,
			"versionNonce": 210991100,
			"isDeleted": false,
			"id": "kCrqKbN9",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 1102.6215012494258,
			"y": 245.61367218237405,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 127.1953125,
			"height": 202.39999999999998,
			"seed": 645377604,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694315102905,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "/wordcount/output\n\n    part-r-0000\n        key \\t value\n        key \\t value\n        ....\n    \n    part-r-0001\n        key \\t value\n        key \\t value\n        ....",
			"rawText": "/wordcount/output\n\n    part-r-0000\n        key \\t value\n        key \\t value\n        ....\n    \n    part-r-0001\n        key \\t value\n        key \\t value\n        ....",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "/wordcount/output\n\n    part-r-0000\n        key \\t value\n        key \\t value\n        ....\n    \n    part-r-0001\n        key \\t value\n        key \\t value\n        ....",
			"lineHeight": 1.15,
			"baseline": 199
		},
		{
			"type": "text",
			"version": 1024,
			"versionNonce": 1370607996,
			"isDeleted": false,
			"id": "rC6gjDZS",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 364.58573464786343,
			"y": 192.81364166479585,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 240.107421875,
			"height": 23,
			"seed": 1252558844,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694315180789,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "reduce task（yarn child）0",
			"rawText": "reduce task（yarn child）0",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "reduce task（yarn child）0",
			"lineHeight": 1.15,
			"baseline": 18
		},
		{
			"type": "text",
			"version": 1017,
			"versionNonce": 382620668,
			"isDeleted": false,
			"id": "UUqNZP1A",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1145.2522995892696,
			"y": 180.8137026999521,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 54.443359375,
			"height": 23,
			"seed": 577485052,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694315136758,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 2,
			"text": "HDFS",
			"rawText": "HDFS",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "HDFS",
			"lineHeight": 1.15,
			"baseline": 18
		},
		{
			"type": "arrow",
			"version": 89,
			"versionNonce": 665745476,
			"isDeleted": false,
			"id": "QOh7gZuxCDIByn9c7mr9G",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 666.3344257828461,
			"y": 437.1337100241709,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 205.1852077907986,
			"height": 107.40743001302087,
			"seed": 1430474308,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694315221148,
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
					205.1852077907986,
					-107.40743001302087
				]
			]
		},
		{
			"type": "arrow",
			"version": 89,
			"versionNonce": 809351620,
			"isDeleted": false,
			"id": "2z6hR8pNb_j0LWvGWcmI1",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 918.186232423471,
			"y": 328.244821135282,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 201.48145887586816,
			"height": 30.370381673177064,
			"seed": 2134822852,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1694315238885,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "JDQqEXoZ",
				"focus": 1.007196881475227,
				"gap": 12.30211215549025
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
					201.48145887586816,
					-30.370381673177064
				]
			]
		},
		{
			"type": "arrow",
			"version": 195,
			"versionNonce": 1372435974,
			"isDeleted": false,
			"id": "slp0hXN2p-x32KiaL-deh",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 790.0380390640961,
			"y": 8.24473636423167,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 612.5925699869791,
			"height": 220.00001695421003,
			"seed": 644894076,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "iZqQmEgd"
				}
			],
			"updated": 1694342658672,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "nwPyY5f2PrdrsMxCcMYBU",
				"gap": 1.406206689000122,
				"focus": -0.11153787122447802
			},
			"endBinding": {
				"elementId": "f3NwfHeW",
				"focus": -0.9435947835989035,
				"gap": 9.567418117149003
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
					-287.40743001302087,
					131.8518575032552
				],
				[
					-612.5925699869791,
					220.00001695421003
				]
			]
		},
		{
			"type": "text",
			"version": 12,
			"versionNonce": 1922962044,
			"isDeleted": false,
			"id": "iZqQmEgd",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 473.2868590510752,
			"y": 130.89659386748687,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 58.6875,
			"height": 18.4,
			"seed": 999612284,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1694315309769,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 2,
			"text": "http下载",
			"rawText": "http下载",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "slp0hXN2p-x32KiaL-deh",
			"originalText": "http下载",
			"lineHeight": 1.15,
			"baseline": 15
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#1e1e1e",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 2,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": -63.963562310911016,
		"scrollY": 231.87437786425113,
		"zoom": {
			"value": 0.9000000000000004
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