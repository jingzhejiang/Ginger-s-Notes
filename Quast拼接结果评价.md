# 拼接结果评估
## 根据megahit网页中的示例进行
###### 结果检查，安装两个包
> $ tar zvxf BBMap_36.02.tar.gz

> $ tar xvjf samtools-1.3.1.tar.bz2

> $ cd samtools-1.3.1 && make -j && cd ..

###### 定义变量，以方便后面引用
> $ BBMAP=~/bbmap

> $ MEGAHIT=~/megahit-master

> $ SAMTOOLS=~/samtools-1.3.1

> $ MDA8PATH=/WORK/scsfri_jzjiang_1/UBC/2015-12-28_SuttC-Jiang-2x300-122815-27418864/s0618_3-MDA8-27268946/Data/Intensities/BaseCalls

###### 检查一下是否正确
> $ echo $BBMAP

> $ echo $MEGAHIT

> $ echo $SAMTOOLS

> $ echo $MDA8PATH

###### Align reads with bbwrap.sh
> $BBMAP/bbwrap.sh ref=/WORK/scsfri_jzjiang_1/UBC/2015-12-28_SuttC-Jiang-2x300-122815-27418864/s0618_3-MDA8-27268946/Data/Intensities/BaseCalls/MDA8_test_asmb02/final.contigs.fa in=/WORK/scsfri_jzjiang_1/UBC/2015-12-28_SuttC-Jiang-2x300-122815-27418864/s0618_3-MDA8-27268946/Data/Intensities/BaseCalls/MDA8_S1_L001_R1_001.fastq.gz in2=/WORK/scsfri_jzjiang_1/UBC/2015-12-28_SuttC-Jiang-2x300-122815-27418864/s0618_3-MDA8-27268946/Data/Intensities/BaseCalls/MDA8_S1_L001_R2_001.fastq.gz out=MDA8_aln

###### Output per contig coverage to cov.txt with pileup.sh
> $ $BBMAP/pileup.sh in=MDA8_aln out=coverage.txt

###### 上面方法无法分析组装结果，使用quast试试

# 使用quast对拼接结果进行评价
###### It is also highly recommended to install the Matplotlib Python library for drawing plots. We recommend to use Matplotlib version 1.1。但是天河上的版本是0.99
###### 试运行时出现以下警告

> WARNING: Can't draw plots: matplotlib version is old! Please use matplotlib version 1.1 or higher.

> WARNING: Failed to compile E-MEM (/HOME/scsfri_jzjiang_1/quast-4.1/libs/E-MEM-linux)! Try to compile it manually. You can restart Quast with the --debug flag to see the command line.

> WARNING: Previous try of E-MEM compilation was unsuccessful! For forced retrying, please remove /HOME/scsfri_jzjiang_1/quast-4.1/libs/E-MEM-linux/make.failed and restart QUAST.

> URLError: <urlopen error [Errno -3] Temporary failure in name resolution>
ERROR! exception caught!

###### 发邮件给天河技术支持，matplotlib的问题解决！先要如下指令
> module load  Python/2.7.9-fPIC

###### 发邮件给quast.support@bioinf.spbau.ru，E-MEM问题

Hi Jingzhe Jiang,

This warning is not crucial for further QUAST work, so you may just ignore it. If you are interested in details, please read the text below.

Since version 4.1 we added E-MEM aligner in addition to Nucmer aligner which was used by default in all previous versions of Quast.
Their outputs are absolutely the same and the only difference is speed: E-MEM is parallelised and thus faster then single-threaded Nucmer. 
However, E-MEM compilation requires additional dependencies, e.g. it needs boost  (tested with v1.56.0) which is probably lacking on your server. 
In such cases (E-MEM compilation is failed) we just switch Quast to Nucmer aligner and it works absolutely correctly and with the same output 
as all previous Quast versions, so no worries about that!

If you still want to compile E-MEM, you can check error output of make command in /HOME/scsfri_jzjiang_1/quast-4.1/libs/E-MEM-linux/make.err
and find some glues there. I suggest that you need boost installed on your system.

Best regards,
Alexey

# Quast基本运行口令

```
./quast.py test_data/contigs_1.fasta test_data/contigs_2.fasta -R test_data/reference.fasta.gz -G test_data/genes.gff
```
```
python metaquast.py contigs_1 contigs_2 ... -R reference_1,reference_2,reference_3,...
```
++if you are working with metagenome assemblies, we recommend to use metaquast.py instead of quast.py (it is in the same directory as quast.py).++

# 试运行

```
module load  Python/2.7.9-fPIC
python ~/quast-4.1/metaquast.py /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R1.clean.fastq.gz /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R2.clean.fastq.gz -R /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_asmb/final.contigs.fa
```
没有问题，终止后计划用yhbatch运行

# 在linux中直接创建quast.sh文件

```
cat > quast.sh
```
输入如下内容

```
#!/bin/bash
source /WORK/app/osenv/ln1/set2.sh
module load Python/2.7.9-fPIC
yhrun -n 1 -N 1 -p free python ~/quast-4.1/metaquast.py /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R1.clean.fastq.gz /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R2.clean.fastq.gz -R /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_asmb/final.contigs.fa
```
也可以用vi编辑，**i**进入编辑模式，**Esc**退出，**:wq**保存

查看权限
```
ll
```
或

```
ls -l
```
增加权限

```
chmod +x quast.sh
```
运行yhbatch

```
yhbatch -n 1 -N 1 -p free quast.sh
```
运行正常！

但检查slurm-1407094.out文件时发现，python版本不对，依然无法作图，说明module load  Python/2.7.9-fPIC没有执行因此取消进程

```
yhcancel 1407094

```
更改quast.sh内容，把++module load Python/2.7.9-fPIC++一行改为：*yhrun -n 1 -N 1 -p free module load Python/2.7.9-fPIC*依然不行，干脆删除，并增加一个--threads参数，以保证24个cpu被充分利用。
```
#!/bin/bash
source /WORK/app/osenv/ln1/set2.sh
yhrun -n 1 -N 1 -p free python ~/quast-4.1/metaquast.py /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R1.clean.fastq.gz /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R2.clean.fastq.gz -R /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_asmb/final.contigs.fa --threads 24 --no-snps --memory-efficient
```
再运行yhbatch前，先运行

```
module load Python/2.7.9-fPIC
```
然后再运行yhbatch

```
yhbatch -n 1 -N 1 -p free quast.sh
```
显示cpu被充分利用，python版本也是2.7.9，一切正常！
```
Version: 4.1, build 26.05.2016 14:25

System information:
  OS: Linux-2.6.32-279-TH2-x86_64-with-redhat-6.2-Santiago (linux_64)
  Python version: 2.7.9
  CPUs number: 24

Started: 2016-07-24 21:23:08

```
运行大概18分钟，发现fastq格式不是quast的准许格式，应该使用fasta格式。程序终止
```
  Contigs:
    1  /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R1.clean.fastq.gz ==> dCh_4_L5_I043.R1.clean.fastq
    2  /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R2.clean.fastq.gz ==> dCh_4_L5_I043.R2.clean.fastq
      WARNING: Skipping /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R1.clean.fastq.gz because it contains non-ACGTN characters.
      WARNING: Skipping /WORK/scsfri_jzjiang_1/scsfri/dCh_4_L5_I043.R2.clean.fastq.gz because it contains non-ACGTN characters.
  
  NOTICE: None of the assembly files contains correct contigs. Please, provide different files or decrease --min-contig threshold.

Failed aligning the contigs for all the references. Try to restart MetaQUAST with another references.

MetaQUAST finished.
  Log saved to /WORK/scsfri_jzjiang_1/Analysis/JingZhe/dCh_asmb/quast_results/results_2016_07_24_21_23_06/metaquast.log

Finished: 2016-07-24 21:41:06
Elapsed time: 0:17:58.528792
Total NOTICEs: 1; WARNINGs: 2; non-fatal ERRORs: 0

```



# Quast manual摘抄
--labels (or -l) <label,label...>
Human-readable assembly names. Those names will be used in reports, plots and logs. For example:
-l SPAdes,IDBA-UD
If your labels include spaces, use quotes:
-l SPAdes,"Assembly 2",Assembly3
-l "SPAdes 2.5, SPAdes 2.4, IDBA-UD"

==--memory-efficient==
Run Nucmer using one thread, separately per each assembly and each chromosome. This may significantly reduce memory consumption on large genomes.

==--threads (or -t) <int>==
Maximum number of threads. The default value is 25% of all available CPUs but not less than 1. If QUAST fails to determine the number of CPUs, maximum threads number is set to 4.

