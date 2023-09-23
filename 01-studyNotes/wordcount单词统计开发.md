---
author: wufm
title: wordcount单词统计开发
rating: 5
time: 2023-09-10 周日
tags:
  - MapReduce
  - 单词统计
代码:
---

## 一、创建 maven 工程

maven名称：hadoop-mapreduce

在 pom.xml 文件中添加如下依赖

```xml
<dependencies>  
    <dependency>  
        <groupId>org.apache.hadoop</groupId>  
        <artifactId>hadoop-client</artifactId>  
        <version>3.3.6</version>  
    </dependency>  
</dependencies>  
  
<build>  
    <finalName>wc</finalName>  
    <plugins>  
        <!--指定jdk版本对代码进行编译-->  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-compiler-plugin</artifactId>  
            <version>3.6.1</version>  
            <configuration>  
                <source>1.8</source>  
                <target>1.8</target>  
            </configuration>  
        </plugin>  
    </plugins>  
</build>
```

在项目的 src/main/resources 目录下，新建一个文件，命名为“log4j.properties”，在文件中填入（作用是显示日志）。
```txt
log4j.rootLogger=INFO,stdout  
  
# 控制台输出  
log4j.appender.stdout=org.apache.log4j.ConsoleAppender  
log4j.appender.stdout.Target=System.out  
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout  
log4j.appender.stdout.layout.ConversionPattern=[%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n
```

## 二、编写Mapper类

```java
package com.bigdata.mr.wordCount;  
  
import org.apache.hadoop.io.IntWritable;  
import org.apache.hadoop.io.LongWritable;  
import org.apache.hadoop.io.Text;  
import org.apache.hadoop.mapreduce.Mapper;  
import java.io.IOException;  
  
/**  
 * Date:2023/9/5 * Author:wfm * Desc: 单词统计的map阶段逻辑  
 * 每行以空格切开，输出结果：单词,1  
 * * Mapper<KEYIN, VALUEIN, KEYOUT, VALUEOUT>  
 * KEYIN：是map task读取到的数据的key的类型，是一行的起始偏移量Long  
 * VALUEIN：是map task读取到的数据的value的类型，是一行的内容String  
 * * KEYOUT：是用户的自定义map方法要返回的结果kv数据的key类型，在wordcount逻辑中，我们需要返回单词的数据类型String  
 * VALUEOUT：是用户的自定义map方法要返回的结果kv数据的value类型,在wordcount逻辑中,我们返回的是整数Integer  
 * * 在mapreduce中，map产生的数据需要传输给reduce,需要进行序列化和反序列化，而jdk中原生序列化机制产生的数据量比较冗余，  
 * 运行过程中传输效率低下  
 * 所以，hadoop专门设计了自己的序列化机制。mapreduce中传输数据类型就必须实现hadoop自己的序列化接口  
 *  
 * hadoop为jdk的常用基本类型Long String Integer Float等数据类型封装了自己实现hadoop序列化接口的的类型  
 * LongWritable Text IntWritable FloatWritable  
 * */
public class WordcountMapper extends Mapper<LongWritable, Text,Text, IntWritable> {  
  
    Text k = new Text();  
    IntWritable v = new IntWritable(1);  
  
    // 重写方法快捷键ctrl+o  
    @Override  
    protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {  
  
        // 切单词  
        String line = value.toString();  
        String[] words = line.split(" ");  
  
        for (String word : words) {  
            k.set(word);  
            context.write(k,v);  
            // 不可取，每次调用一次创建一个对象  
//            context.write(new Text(word),new IntWritable(1));  
        }  
    }  
}
```

## 三、编写Reducer类

```java
package com.bigdata.mr.wordCount;  
  
import org.apache.hadoop.io.IntWritable;  
import org.apache.hadoop.io.Text;  
import org.apache.hadoop.mapreduce.Reducer;  
import java.io.IOException;  
import java.util.Iterator;  
  
/**  
 * Date:2023/9/5 * Author:wfm * Desc: */
public class WordcountReducer extends Reducer<Text, IntWritable, Text, IntWritable> {  
  
    IntWritable v = new IntWritable();  
  
    @Override  
    protected void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {  
  
        int count = 0;  
        // 对相同key的value值进行相加  
        Iterator<IntWritable> iterator = values.iterator();  
  
        while (iterator.hasNext()) {  
            IntWritable value = iterator.next();  
            count += value.get();  
        }  
        v.set(count);  
        context.write(key, v);  
  
    }  
}
```

## 四、编写客户端类

### 4.1 在windows系统上执行（连接hadoop集群，以yarn的方式执行）

```java
package com.bigdata.mr.wordCount;  
  
import org.apache.hadoop.conf.Configuration;  
import org.apache.hadoop.fs.Path;  
import org.apache.hadoop.io.IntWritable;  
import org.apache.hadoop.io.Text;  
import org.apache.hadoop.mapreduce.Job;  
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;  
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;  
  
/**  
 * Date:2023/9/5 * Author:wfm * Desc: 单词统计主主入口  
 * 以空格切分，统计单词出现次数  
 *  
 * 程序写完记得打包 package，后右键点击run  
 * 准备：  
 * 在hdfs的/input准备几份数据  
 *  
 * 用于提交mapreduce job的客户端程序  
 * 功能：  
 * 1.封装本次job运行时所需要的必要参数  
 * 2.跟yarn进行交互，将mapreduce程序成功的启动运行  
 */  
public class JobSubmitterWindowsToYarn {  
  
    public static void main(String[] args) throws Exception {  
        // 在代码中设置JVM系统参数，用于给job对象来获取访问HDFS的用户身份  
        System.setProperty("HADOOP_USER_NAME", "hadoop");  
  
        Configuration conf = new Configuration();  
        // 设置job运行时要访问的默认文件系统  
        conf.set("fs.defaultFS", "hdfs://hadoop101:8020");  
        // 设置job提交到哪里运行,默认时local  
        conf.set("mapreduce.framework.name", "yarn");  
        // resoucemanager的机器  
        conf.set("yarn.resourcemanager.hostname", "hadoop102");  
        // 如果要从windows系统上运行这个job提交客户端程序，则需要夹这个跨平台提交参数  
        conf.set("mapreduce.app-submission.cross-platform", "true");  
  
        // 1.创建job对象  
        Job job = Job.getInstance(conf);  
  
        // 2.封装参数  
        // 2.1 jar包所在位置  
        job.setJar("C:/code/bigdata/hadoop-mapreduce/target/wc.jar");  
  
        // 2.2 本次job所要调用的mapper实现类、reduce实现类  
        job.setMapperClass(WordcountMapper.class);  
        job.setReducerClass(WordcountReducer.class);  
  
        // 2.3 本次job的mapper实现类、reduce实现类产生的结果数据的key、value类型  
        job.setMapOutputKeyClass(Text.class);  
        job.setMapOutputValueClass(IntWritable.class);  
  
        job.setOutputKeyClass(Text.class);  
        job.setOutputValueClass(IntWritable.class);  

        // 2.4 本次job要处理的输入数据集所在路径、最终结果的输出路径  
        FileInputFormat.setInputPaths(job, new Path("/input"));  
        FileOutputFormat.setOutputPath(job, new Path("/output"));// 注意：输出路径必须不存在  
  
        // 2.5 想要启动的reduce task的数量  
        job.setNumReduceTasks(2);  
  
        // 3.提交job给yarn  
        boolean res = job.waitForCompletion(true);  
  
        // 非必要的，程序退出  
        System.exit(res?0:-1);  
    }  
}
```

### 4.2 在linux系统上执行（连接hadoop集群，以yarn的方式执行）

```java
package com.bigdata.mr.wordCount;  
  
import org.apache.hadoop.conf.Configuration;  
import org.apache.hadoop.fs.Path;  
import org.apache.hadoop.io.IntWritable;  
import org.apache.hadoop.io.Text;  
import org.apache.hadoop.mapreduce.Job;  
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;  
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;  
  
/**  
 * Date:2023/9/5 * Author:wfm * Desc:单词统计主主入口  
 * 以空格切分，统计单词出现次数  
 * 准备：  
 * 在hdfs的/input准备几份数据  
 *  
 * 程序写完记得打包 package，然后把包放在集群中的任意机器中，最后执行  
 * hadoop jar 包名.jar JobSubmitterLinuxToYarn  
 * 或者  
 *hadoop jar 包名.jar JobSubmitterLinuxToYarn 参数1 参数2 ...  
 * 如果要在hadoop集群的某台机器上启动这个job提交客户端的话  
 * conf里面就不需要指定fs.defaultFS  mapreduce.framework.name等  
 *  
 * 因为在集群上用hadoop jar xx.jar JobSubmitterLinuxToYarn  
 * 命令来启动客户端main方法时  
 * hadoop jar这个命令会将所在机器上的hadoop安装目录中的jar包和配置文件加入到运行时的classpath中  
 *  
 * 那么我们在客户端main方法中的new Configuration()语句就会加载classpath中的配置文件，自然就有了  
 * fs.defaultFS  mapreduce.framework.name等这些参数配置  
 */  
public class JobSubmitterLinuxToYarn {  
  
    public static void main(String[] args) throws Exception{  
  
        Configuration conf = new Configuration();  
  
        // 1.创建job对象  
        Job job = Job.getInstance(conf);  
  
        // 2.封装参数  
        // 2.1 jar包所在位置  
        job.setJarByClass(JobSubmitterLinuxToYarn.class);  
  
        // 2.2 本次job所要调用的mapper实现类、reduce实现类  
        job.setMapperClass(WordcountMapper.class);  
        job.setReducerClass(WordcountReducer.class);  
  
        // 2.3 本次job的mapper实现类、reduce实现类产生的结果数据的key、value类型  
        job.setMapOutputKeyClass(Text.class);  
        job.setMapOutputValueClass(IntWritable.class);  
  
        job.setOutputKeyClass(Text.class);  
        job.setOutputValueClass(IntWritable.class);  
  
        // 2.4 本次job要处理的输入数据集所在路径、最终结果的输出路径  
        FileInputFormat.setInputPaths(job, new Path("/input"));  
        FileOutputFormat.setOutputPath(job, new Path("/output"));// 注意：输出路径必须不存在  
  
        // 2.5 想要启动的reduce task的数量  
        job.setNumReduceTasks(2);  
  
        // 3.提交job给yarn  
        boolean res = job.waitForCompletion(true);  
  
        // 非必要的，程序退出  
        System.exit(res?0:-1);  
  
    }  
}
```

### 4.3 在windows系统上执行（以本地方式执行）

```java
package com.bigdata.mr.wordCount;  
  
import org.apache.hadoop.conf.Configuration;  
import org.apache.hadoop.fs.Path;  
import org.apache.hadoop.io.IntWritable;  
import org.apache.hadoop.io.Text;  
import org.apache.hadoop.mapreduce.Job;  
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;  
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;  
  
/**  
 * Date:2023/9/5 * Author:wfm * Desc:单词统计主主入口  
 * 以空格切分，统计单词出现次数  
 * 准备：  
 * 在hdfs的/input准备几份数据  
 *  
 * 用windows安装的hadoop环境。  
 *  
 * 文件系统用本地系统conf.set("fs.defaultFS", "file:///")  
 * */public class JobSubmitterWindowsLocal {  
  
    public static void main(String[] args)throws Exception {  
  
        // 1.创建job对象  
        Configuration conf = new Configuration();  
        Job job = Job.getInstance(conf);  
  
        // 2.封装参数  
        // 2.1 jar包所在位置  
        job.setJarByClass(JobSubmitterWindowsLocal.class);  
  
        // 2.2 本次job所要调用的mapper实现类、reduce实现类  
        job.setMapperClass(WordcountMapper.class);  
        job.setReducerClass(WordcountReducer.class);  
  
        // 2.3 本次job的mapper实现类、reduce实现类产生的结果数据的key、value类型  
        job.setMapOutputKeyClass(Text.class);  
        job.setMapOutputValueClass(IntWritable.class);  
  
        job.setOutputKeyClass(Text.class);  
        job.setOutputValueClass(IntWritable.class);  
  
        // 判断输出目录是否存在，存在删除  
//        File output = new File("C:/Alearning/data/mr/wc/output");  
//        if (output.exists()){  
//            FileUtils.deleteDirectory(output);  
//        }  
  
        // 2.4 本次job要处理的输入数据集所在路径、最终结果的输出路径  
        FileInputFormat.setInputPaths(job,new Path("C:/Alearning/data/mr/wc/input"));  
        FileOutputFormat.setOutputPath(job,new Path("C:/Alearning/data/mr/wc/output"));  
  
        // 2.5 想要启动的reduce task的数量  
        job.setNumReduceTasks(2);  
  
        // 3.提交job给yarn  
        boolean res = job.waitForCompletion(true);  
  
        System.exit(res?0:-1);  
    }  
}
```