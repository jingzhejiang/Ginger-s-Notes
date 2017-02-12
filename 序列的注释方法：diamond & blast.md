# makedb
直接makedb，程序一下子占用64个线程，直接被killed

```
~/diamond makedb --in uniref50.fasta -d uniref50
```
使用yhrun命令

```
yhrun -n 1 -N 4 -p free ~/diamond makedb --in uniref50.fasta -d uniref50 --threads 96


# 结果显示……
Loading sequence data (17800000 sequences processed)...  [0.420959s]
Writing trailer...  [0.433892s]
Closing the database file...  [0.003135s]
Processed 17848597 sequences, 5427746582 letters.
Total time = 178.536s

```
如果不加--threads参数，程序依然运行24核：yhrun -n 1 -N 4 -p free ~/diamond makedb --in uniref50.fasta -d uniref50

由于感觉96线程效果和24个的一样，既然如此，为何不试一下下面的参数，省去yhrun的核小时

```
~/diamond makedb --in uniref100.fasta -d uniref100 --threads 2
```
结果运行一段时间后依然被killed！看来不是核数占用的问题，而是运行时间或者内存的问题。时间或者内存占用多一点就会killed。

看来必须要用yhrun了！继续建uniref90库

```
yhrun -n 1 -N 1 -p free ~/diamond makedb --in uniref90.fasta -d uniref90 --threads 24


# 结果显示……
Loading sequence data (43400000 sequences processed)...  [0.053606s]
Writing trailer...  [1.33487s]
Closing the database file...  [0.009028s]
Processed 43405259 sequences, 14692753770 letters.
Total time = 455.167s
```

```
-rwxr-xr-x 1 scsfri_jzjiang_1 scsfri_jzjiang 40356582522 Aug  3 09:44 uniref100.fasta
-rwxr-xr-x 1 scsfri_jzjiang_1 scsfri_jzjiang 19463908869 Aug  3 09:46 uniref90.fasta
-rwxr-xr-x 1 scsfri_jzjiang_1 scsfri_jzjiang  7329657321 Aug  3 09:44 uniref50.fasta
-rw-r--r-- 1 scsfri_jzjiang_1 scsfri_jzjiang  2014880392 Aug  16 10:34 env_nr.fa
-rw-r--r-- 1 scsfri_jzjiang_1 scsfri_jzjiang 60857642178 Aug  29 19:47 nr.fa
```
由上可见，90库是50库的不到三倍，而24线程运行90库的时间也大概是96线程运行50库时间的不到三倍，==说明CPU核数对makedb程序无帮助，该程序无法并行运算。==

继续建100库，不设定threads，程序默认运行全部核数（24）
```
yhrun -n 1 -N 1 -p free ~/diamond makedb --in uniref100.fasta -d uniref100


# 结果显示……
Loading sequence data (83000000 sequences processed)...  [0.840958s]
Writing trailer...  [2.16628s]
Closing the database file...  [0.027459s]
Processed 83050155 sequences, 30948521841 letters.
Total time = 941.359s
```
进入/WORK/scsfri_jzjiang_1/Analysis/JingZhe/文件夹，准备开始blastx分析dCh_asmb数据。新建dCh_dmnd文件夹，将DIAMOND结果保存于此。

运行DIAMOND程序前还要运行如下命令，以便告诉服务器正确的library位置

```
source /WORK/app/osenv/ln1/set2.sh
```
直接在服务器上创建一个sh脚本文件，[How to Quickly Create a Text File Using the Command Line in Linux](http://www.howtogeek.com/199687/how-to-quickly-create-a-text-file-using-the-command-line-in-linux/)
```
cat > dCh_dmnd.sh
```
粘贴如下内容

```
#!/bin/bash
yhrun -n 1 -N 1 -p free ~/diamond blastx -d /WORK/scsfri_jzjiang_1/PubData/Uniref20160801/uniref50 -q /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_asmb/final.contigs.fa -o /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_dmnd/matches.tab --more-sensitive --block-size 10 --index-chunks 1 --outfmt tab

# 输出格式matches.tab与--outfmt tab相对应；
# --block-size 10，大概使用内存为60G，在64G/节点的能力之内；
# --index-chunks 1，据说运算效率最高
# --threads默认最大线程数，因此未做设定
```
**然后按Ctrl+D退出**

修改该文件权限，使其变成可执行文件

```
chmod +x dCh_dmnd.sh
```
运行yhbatch

```
yhbatch -n 1 -N 1 -p free dCh_dmnd.sh
```
开始运行

```
JOBID    PARTITION     NAME    USER         ST     TIME  NODES NODELIST(REASON)
1582508    free     dCh_dmnd scsfri_jzjia   R      1:03    1     cn9301

# 结果出错退出，内存不够，但是不知何时推出来的
Failed to allocate sufficient memory. Please refer to the readme for instructions on memory usage.
yhrun: error: cn9301: task 0: Exited with exit code 1

```
将--index-chunks改为4，再次运行试试。

```
#!/bin/bash
yhrun -n 1 -N 1 -p free ~/diamond blastx -d /WORK/scsfri_jzjiang_1/PubData/Uniref20160801/uniref50 -q /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_asmb/final.contigs.fa -o /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_dmnd/matches.tab --more-sensitive --block-size 10 --index-chunks 4 --outfmt tab

```
运行一段时间后又出现错误

```
Building query index...  [2.9508s]
Building seed filter...  [4.20147s]
yhrun: error: Node failure on cn9232
slurmd[cn9232]: *** JOB 1585974 CANCELLED AT 2016-08-05T16:52:11 DUE TO NODE FAILURE ***
```
天河的答复是
> 这个是结点的问题，请重新提交试试，并在提交命令中加入“-x cn9232”排除掉这个结点。

再次运行

```
yhbatch -n 1 -N 3 -p free dCh_dmnd.sh -x cn9232

# 显示问题：
yhbatch: Warning: can't run 1 processes on 3 nodes, setting nnodes to 1
Submitted batch job 1591206

```
请教天河，答复如下
> 首先需要你的程序需要支持mpi才能跨节点运行。其次提交命令中的-n设置的数字要大于等于3且小于等于3*24。-n对应的是mpi并行的进程数，若你的程序是串行的-n需设为1，且不能使用多个结点计算。（关于线程和进程的[形象解释](http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html)）

重新改回原指令运行

```
yhbatch -n 1 -N 1 -p free dCh_dmnd.sh -x cn9232

………
Total time = 34893.6s
Reported 394276 pairwise alignments, 736884 HSSPs.
42208 queries aligned.

```
运行完毕，用时==大概9.69个小时==

# 比较DIAMOND和Blast的比对结果

Sequences in FASTA format can be generated from the pre-formatted databases by using the [blastdbcmd](http://www.ncbi.nlm.nih.gov/books/NBK279689/) utility，进行NR数据库的格式转换

```
/HOME/scsfri_jzjiang_1/ncbi-blast-2.4.0+/bin/blastdbcmd -db /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/NR20160802/nr -dbtype prot -out /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/NR20160802/nr.fa -outfmt %f -entry all
```
再使用diamond makedb转换为diamond数据库

```
#!/bin/bash
/HOME/scsfri_jzjiang_1/diamond makedb --in /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/NR20160802/nr.fa -d /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/NR20160802/nr
```
同时也转换env_nr数据库
```
#!/bin/bash
/HOME/scsfri_jzjiang_1/diamond makedb --in /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/NR20160802/env_nr.fa -d /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/NR20160802/env_nr
```




使用AbHV的部分数据

```
head -2000 final.contigs.fa | cat > final.contigs_head2000.fa
```


出现错误

```
diamond v0.8.17.79 | by Benjamin Buchfink <buchfink@gmail.com>
Check http://github.com/bbuchfink/diamond for updates.

#CPU threads: 24
Scoring parameters: (Matrix=BLOSUM62 Lambda=0.267 K=0.041 Penalties=11/1)
#Target sequences to report alignments for: 25
Error: Error opening temporary file: HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_diamond/diamond-57c60294-0.tmp

```
==结果是脚本路径写错了==，-o 的路径HOME前面缺少/。修改后运行正常！


```
#!/bin/bash
# 比对Uniref50库
/HOME/scsfri_jzjiang_1/diamond blastx -d /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/Uniref20160801/uniref50 -q /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_megahit/final.contigs_head2000.fa -o /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_dmnd/matches_head2000_ms.tab --more-sensitive --block-size 10 --index-chunks 4 --outfmt tab
/HOME/scsfri_jzjiang_1/diamond blastx -d /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/Uniref20160801/uniref50 -q /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_megahit/final.contigs_head2000.fa -o /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_dmnd/matches_head2000_s.tab --sensitive --block-size 10 --index-chunks 4 --outfmt tab
/HOME/scsfri_jzjiang_1/diamond blastx -d /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/Uniref20160801/uniref50 -q /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_megahit/final.contigs_head2000.fa -o /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_dmnd/matches_head2000_n.tab --block-size 10 --index-chunks 4 --outfmt tab
# 比对Uniref100库
/HOME/scsfri_jzjiang_1/diamond blastx -d /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/Uniref20160801/uniref100 -q /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_megahit/final.contigs_head2000.fa -o /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_dmnd/matches_head2000_n_urf100.tab --block-size 10 --index-chunks 4 --outfmt tab
# 比对NR库
/HOME/scsfri_jzjiang_1/diamond blastx -d /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/NR20160802/nr -q /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_megahit/final.contigs_head2000.fa -o /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_dmnd/matches_head2000_n_nr.tab --block-size 10 --index-chunks 4 --outfmt tab
```
使用Blastp比对Uniref库

首先建blast库

```
#!/bin/bash
/HOME/scsfri_jzjiang_1/ncbi-blast-2.4.0+/bin/makeblastdb -in /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/Uniref20160801/uniref50.fasta -out /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/Uniref20160801/uniref50 -dbtype prot
```
然后同时比对
```
#!/bin/bash
/HOME/scsfri_jzjiang_1/ncbi-blast-2.4.0+/bin/blastx -query /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_megahit/final.contigs_head2000.fa -task blastx -db /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/Uniref20160801/uniref50 -evalue 1e-5 -num_threads 24 -out /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_blast/blast_head2000_b_e5.tab -sum_stats t

#!/bin/bash
/HOME/scsfri_jzjiang_1/ncbi-blast-2.4.0+/bin/blastx -query /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_megahit/final.contigs_head2000.fa -task blastx-fast -db /HOME/scsfri_jzjiang_1/WORKSPACE/PubData/Uniref20160801/uniref50 -evalue 1e-5 -num_threads 24 -out /HOME/scsfri_jzjiang_1/WORKSPACE/Analysis/JingZhe/AbHV_blast/blast_head2000_bf_e5.tab -sum_stats t
```


比较diamond的三种模式比对的速率和比对量

Diamond-ms | Diamond-s | Diamond | Diamond | Diamond | Blast-b-e5 | Blast-bf-e5 | 
---|---|---|---|---|---|---
Uniref50 | Uniref50 | Uniref50 | Uniref100 | NR | Uniref50 | Uniref50 | 
**218 hits** | 216 hits | 190 hits | 191 hits | 187 hits | **234 hits** | 223 hits | 
771.656 s | 733.415 s | **221.305 s** | 2290.91 s | 2522.24 s | 4188.502 s | **816.571 s** | 

* 注：unifef100大概是uniref50六倍的数据量，是NR的2/3
* 结论：
1. uniref50好于uniref100，快而且灵敏度不低
2. Blast-bf速度是Blast的5倍，比对效果差不多
3. Blast-bf速度稍慢于Diamond-ms，灵敏度稍好
4. Diamond速度最快，是Blast-bf的4倍，灵敏度低1/6
5. 根据数据集大小和时间要求可以考虑使用Diamond和Blast-bf-e5，数据库使用Uniref50即可，包含了taxonomy信息


## Blast结果的几种[格式](http://etutorials.org/Misc/blast/Part+VI+Appendixes/Appendix+A.+NCBI+Display+Formats/A.2+Detailed+Descriptions+and+Examples/)