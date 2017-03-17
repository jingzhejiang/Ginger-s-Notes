# BBMap

[说明](http://seqanswers.com/forums/showthread.php?t=58221)

## 文库名为名称自动加上数字作为reads id，然后合并文库

### muxbyname.sh

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

正式运行的代码

···bash
/pub/users/vmi/jjz/bbmap/demuxbyname.sh in=/pub/users/vmi/jjz/RawData/MetaG/UBC/UBC_oyster_virome_1.fastq.gz out=/pub/users/vmi/jjz/RawData/MetaG/UBC/% length=12 prefixmode=t
···

分离出来的文件大小由于没压缩的原因，均比原文件稍大一点，合情合理。

将两个合并后的文件均下载到本地电脑保存。

## 上传到Taxonomer网址直接分析



