# 我的分析思路
1. Overall assembly:

**Ideally**: Binning-->Assembly with different tools-->QC-->Deredundency and pickout uniq-contigs (OTUs)-->Taxonomy-->Calculate RPKM and OTU table

**Tianhe sever**: Assembly with only Megahit --> alignment with any possible tools for best alignment rate--> QC --> calculate RPKM and get OTU table

2. Data digging:

Binning based on viral genome characters-->Assembly with different tools-->Mapping reads to contigs-->PCR based full lenght clone.

# 超算、云及本地服务器的分工
1. 天河用来做上游分析（拼接和注释），由于内存受限拼接工作可能会受限制，因此只能使用megahit做拼接
2. 亚马逊云方便调用各种环境和工具，利于做各种分析，但考虑其数据传输是短板，加上费用较高，因此最好定位于资源占用量少的下游相关分析
3. 其它宏基因组在线分析工具由于数据传输的限制，依然定位于下游分析比较合适
4. 本地服务器购置后基本可以代替超算和云服务