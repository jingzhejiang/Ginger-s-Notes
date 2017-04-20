# Taxonomer本地化分析

[说明](http://seqanswers.com/forums/showthread.php?t=58221)

## BBMap

### muxbyname.sh

文库名作为名称，自动加上数字作为reads id，然后合并文库

Multiplexes reads from multiple files after renaming them based on their initial file. Opposite of demuxbyname.

```bash
# 进入目标文件夹/pub/users/vmi/jjz/RawData/MetaG/UBC
# 横向依次（-x）列出所有正向（R1）PE文库

ls *_R1_* -x

# 复制所有文件
# 同样方法复制反向（R2）文库，输入如下口令，-Xmx80g表示只用80G内存

/pub/users/vmi/jjz/bbmap/muxbyname.sh -Xmx80g in=/pub/users/vmi/jjz/RawData/MetaG/UBC/h0618_S3_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0625_S4_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0702_S5_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0730_S9_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0806_S10_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0813_S11_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618-2_S1_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618_3_MDA8_S1_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618_S6_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0625_S7_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702-2_S2_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702_3_MDA22_S2_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702_S8_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0806_S12_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0813_S13_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0820_S14_L001_R1_001.fastq.gz in2=/pub/users/vmi/jjz/RawData/MetaG/UBC/h0618_S3_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0625_S4_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0702_S5_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0730_S9_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0806_S10_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0813_S11_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618-2_S1_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618_3_MDA8_S1_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618_S6_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0625_S7_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702-2_S2_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702_3_MDA22_S2_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702_S8_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0806_S12_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0813_S13_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0820_S14_L001_R2_001.fastq.gz out=/pub/users/vmi/jjz/RawData/MetaG/UBC/UBC_oyster_virome_1.fastq out2=/pub/users/vmi/jjz/RawData/MetaG/UBC/UBC_oyster_virome_2.fastq

# 用nano创建muxbyname.sh文件，并将上述口令粘贴保存
# 在节点上运行

qsub muxbyname.sh -l nodes=cu07

# 运行后日志中显示还有错误，但程序执行完毕。

Set INTERLEAVED to true
Time:                           90.974 seconds.
Reads Processed:       8233k    90.50k reads/sec
Bases Processed:       2214m    24.34m bases/sec

Can't find file /pub/users/vmi/jjz/.vnc/mu01:7.pid
You'll have to kill the Xvnc process manually

# 压缩文件，以便下载（-9为最好的压缩比）。其实这一步在BBMap中用 zl=9 设定
# 这样压缩的效果非常好！大小变为原文件的1/4
gzip UBC_oyster_virome_* -9
```

## 根据名字拆分文库

### demuxbyname.sh

```bash
# This will demultiplex by the first 8 characters of read names.
demuxbyname.sh in=<file> out=<outfile> length=8 prefixmode=t
```

正式运行的代码，不知道是下完一个再下另一个好，还是同时下好？（同时下怕出错，下完一个再下另一个怕链接过期）

···bash
/pub/users/vmi/jjz/bbmap/demuxbyname.sh in=/pub/users/vmi/jjz/RawData/MetaG/UBC/UBC_oyster_virome_1.fastq.gz out=/pub/users/vmi/jjz/RawData/MetaG/UBC/% length=12 prefixmode=t
···

分离出来的文件大小由于没压缩的原因，均比原文件稍大一点，合情合理。

将两个合并后的文件均下载到本地电脑保存。

## Taxonomer分析及数据下载

分析完后，在服务器后台下载原始数据：***.full.taxonomer

```bash
wget "URL" -O /pub/users/vmi/jjz/Jingzhe/UBC_taxonomer/UBC_oyster_virome_2.fastq.full.taxonomer --no-check-certificate -b -nv & wget "URL" -O /pub/users/vmi/jjz/Jingzhe/UBC_taxonomer/UBC_oyster_virome_1.fastq.full.taxonomer --no-check-certificate -b -nv
# b放在后台，nv不显示进度条, &同时运行
```

1. URL为动态链接，经常变化
2. 由于下载经常不完整，可以使用两种方法进行检查：

```bash
wc -l filename # 查看文件的行数，即总reads数，wc为wordcount缩写
```

或

查看文件的最后一行，看看readsID与Raw reads文件中的最后一个ID是否一致


## 根据Taxonomer结果分别画各文库图

### Taxonomer原数据

Raw Results内容如下：

```bash
bacterial       C       s0618_S6_L001_R1_001_54 1445512 10      277
unclassified    U       s0618_S6_L001_R1_001_55 0
human   U       s0618_S6_L001_R1_001_56 2048
unclassified    U       s0618_S6_L001_R1_001_57 0
unclassified    U       s0618_S6_L001_R1_001_58 0
ambiguous       U       s0618_S6_L001_R1_001_59 2112
unclassified    U       s0618_S6_L001_R1_001_60 0
unclassified    U       s0618_S6_L001_R1_001_61 0
viral   C       s0618_S6_L001_R1_001_62 1132026 7       278
unclassified    U       s0618_S6_L001_R1_001_63 0
human   U       s0618_S6_L001_R1_001_64 2048
bacterial       C       s0618_S6_L001_R1_001_65 152509  7       290
ambiguous       U       s0618_S6_L001_R1_001_66 2112
```

第3列的reads ID信息拼接时需要用得到；第4列：Taxid of read assignment. For virus this corresponds to NCBI taxid. For bacteria, this corresponds to an artificial ID to the greengenes database, likewise for fungi except to the UNITE database. For Human this is an artificial ID to a gene.

Taxonomer结果中的json文件，**就是taxonomer用来画图的数据，应该有其它软件可以直接画**；excel文件是给人看的文件，没有json信息全。

> 如果针对不同文库进行作图，首先需要根据Raw results拆分各个库，然后利用里面第4列Taxid，在json结果中调取物种及lineage信息。

### 提取表格中特定数据

这里面涉及到[text manipulation](https://www.ibm.com/developerworks/aix/library/au-unixtext/)，请参阅
[AWK使用详解](http://www.cnblogs.com/ggjucheng/archive/2013/01/13/2858470.html)

#### 拆分出所有病毒数据

#### 拆分各个文库

##### 根据拆分结果画图

Taxonomer文章中并未介绍画图所用的工具，其画图所用原文件为json已知；而[Krona](https://github.com/marbl/Krona/wiki)本身就可以画出类似图，可以尝试用Krona画。





```bash

wget "https://services.taxonomer.com/dev/taxreportsimple/?cmd=&stdin=http%253A%252F%252Fservices.taxonomer.com%252Ftaxonomer%252F%253Fcmd%253D%2526stdin%253Dhttp%2525253A%2525252F%2525252Fservices.taxonomer.com%2525252Ffq2fa%2525252F%2525253Fcmd%2525253Dhttp%25252525253A%25252525252F%25252525252Fservices.taxonomer.com%25252525252Fpartialstream%25252525252F%25252525253Fcmd%25252525253D--url%25252525252520http%25252525253A%25252525252F%25252525252F50.0.17.68%25252525252Ftaxangler%25252525252F%25252525252520--bytes%2525252525252055000000%25252525252520--separator%25252525252520%252525252540%252525252526id%25252525253D41keOYtjz%25252526id%2525253D4JxkluYKsM%2526username%253Dproduction%252F178%2526downloadName%253DXH-1_H7VFTALXX_L2_2.clean.fq.full.taxonomer%2526cache%253D7025.full.taxonomer%2526partialCache%253Dtrue%2526id%253DEJ-1gOtFiz&downloadName=XH-1_H7VFTALXX_L2_2.clean.fq.full.taxonomer&id=Ek7kx_FtoM" -O /pub/users/vmi/jjz/Hongying/taxonomer_results/XH-1_H7VFTALXX_L2_2.clean.fq.full.taxonomer --no-check-certificate -b -nv & wget "https://services.taxonomer.com/dev/taxreportsimple/?cmd=&stdin=http%253A%252F%252Fservices.taxonomer.com%252Ftaxonomer%252F%253Fcmd%253D%2526stdin%253Dhttp%2525253A%2525252F%2525252Fservices.taxonomer.com%2525252Ffq2fa%2525252F%2525253Fcmd%2525253Dhttp%25252525253A%25252525252F%25252525252Fservices.taxonomer.com%25252525252Fpartialstream%25252525252F%25252525253Fcmd%25252525253D--url%25252525252520http%25252525253A%25252525252F%25252525252F50.0.17.68%25252525252Ftaxangler%25252525252F%25252525252520--bytes%2525252525252055000000%25252525252520--separator%25252525252520%252525252540%252525252526id%25252525253D41HCgYYjz%25252526id%2525253D4ylr0xYYif%2526username%253Dproduction%252F178%2526downloadName%253DXH-1_H7VFTALXX_L2_2.clean.fq.full.taxonomer%2526cache%253D7025.full.taxonomer%2526partialCache%253Dtrue%2526id%253DV1-rRlKKiM&downloadName=XH-1_H7VFTALXX_L2_2.clean.fq.full.taxonomer&id=Ek7SCxFtsf" -O /pub/users/vmi/jjz/Hongying/taxonomer_results/XH-2_H7VFTALXX_L2_1.clean.fq.full.taxonomer --no-check-certificate -b -nv

```