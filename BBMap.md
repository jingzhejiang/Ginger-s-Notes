# BBMap

[说明](http://seqanswers.com/forums/showthread.php?t=58221)

## 文库名为名称自动加上数字作为reads id，然后合并文库

### muxbyname.sh

Multiplexes reads from multiple files after renaming them based on their initial file. Opposite of demuxbyname.

```
# 进入目标文件夹/pub/users/vmi/jjz/RawData/MetaG/UBC
# 横向依次（-x）列出所有正向（R1）PE文库

ls *_R1_* -x

# 复制所有文件
# 同样方法复制反向（R2）文库，输入如下口令

cd /pub/users/vmi/jjz/RawData/MetaG/UBC/
/pub/users/vmi/jjz/bbmap/muxbyname.sh in=/pub/users/vmi/jjz/RawData/MetaG/UBC/h0618_S3_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0625_S4_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0702_S5_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0730_S9_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0806_S10_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0813_S11_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618-2_S1_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618_3_MDA8_S1_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618_S6_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0625_S7_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702-2_S2_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702_3_MDA22_S2_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702_S8_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0806_S12_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0813_S13_L001_R1_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0820_S14_L001_R1_001.fastq.gz in2=/pub/users/vmi/jjz/RawData/MetaG/UBC/h0618_S3_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0625_S4_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0702_S5_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0730_S9_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0806_S10_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/h0813_S11_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618-2_S1_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618_3_MDA8_S1_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0618_S6_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0625_S7_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702-2_S2_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702_3_MDA22_S2_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0702_S8_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0806_S12_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0813_S13_L001_R2_001.fastq.gz,/pub/users/vmi/jjz/RawData/MetaG/UBC/s0820_S14_L001_R2_001.fastq.gz out=UBC_oyster_virome_1.fastq out2=UBC_oyster_virome_2.fastq

/pub/users/vmi/jjz/bbmap/muxbyname.sh in=/pub/users/vmi/jjz/RawData/MetaG/UBC/*_R1_*.gz in2=/pub/users/vmi/jjz/RawData/MetaG/UBC/*_R2_*.gz out=/pub/users/vmi/jjz/RawData/MetaG/UBC/UBC_oyster_virome_1.fastq out2=/pub/users/vmi/jjz/RawData/MetaG/UBC/UBC_oyster_virome_2.fastq

# 用nano创建muxbyname.sh文件，并将上述口令粘贴保存
# 在节点上运行

qsub muxbyname.sh -l nodes=cu07

```
