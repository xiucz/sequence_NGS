### sam文件格式sam格式的定义：https://samtools.github.io/hts-specs/SAMv1.pdf
 1. 第3和第7列，可以用来判断某条reads是否比对成功到了基因组的染色体，左右两条reads是否比对到同一条染色体。
 而第1，10，11列可以提取出来还原成我们的测序数据fastq格式的。  
 2. 第2列是 http://picard.sourceforge.net/explain-flags.html ，二进制转换成十进制后的和。   
 3. 第5列，比对结果的质量值，也是因工具而异。  
 4. 第6列CIGAR是比较重要的，解释如下，其中M并不是说match，所以我们的PE 150的reads，大部分都会是150M，但是并不代表着跟参考序列一模一样。其中S/H是比较特殊的，很难讲清楚，但是大部分情况下用不到。
 （soft-clipping碱基是指一条reads未匹配上当前基因组位置的部分，如果有多个reads在这种情况并且这些reads的soft-clipping碱基都能够比对在基因组另一位置，那么就可能存在SV）   
 5. RG代表着你的sam文件比对来自于哪个样本的fastq程序结果。NM这个tag是编辑距离，大概就是你的reads如果想转变成参考基因组，需要改变多少个碱基，如果编辑距离是0才说明你的这个150bp长度的序列跟参考基因组一模一样。
# samtools
#### 对fasta文件建立索引
```
bashsamtools faidx ref_genome.fasta
```
#### sam文件<=>bam文件
```bashsamtools view -h sample.bam > sample.sam samtools view -bS sample.sam > sample.bam
```
#### 提取
```
0x1 PAIRED paired-end (or multiple-segment) sequencing technology 
0x2 PROPER_PAIR each segment properly aligned according to the aligner 
0x4 UNMAP segment unmapped 0x8 MUNMAP next segment in the template unmapped 
0x10 REVERSE SEQ is reverse complemented 
0x20 MREVERSE SEQ of the next segment in the template is reverse complemented 
0x40 READ1 the first segment in the template 
0x80 READ2 the last segment in the template 
0x100 SECONDARY secondary alignment 
0x200 QCFAIL not passing quality controls 
0x400 DUP PCR or optical duplicate 
0x800 SUPPLEMENTARY supplementary alignment
```

```bash
a.提取没有比对到参考序列上的比对结果
samtools view -bf 4 abc.bam > abc.f.bam

b.提取比对到参考序列上的比对结果
samtools view -bF 4 abc.bam > abc.F.bam 

c.提取paired reads中两条reads都比对到参考序列上的比对结果，只需要把两个4+8的值12作为过滤参数即可
samtools view -bF 12 abc.bam > abc.F12.bam

d.提取bam文件中比对到caffold1上的比对结果，并保存到sam文件格式
samtools view abc.bam scaffold1 > scaffold1.sam

e.提取scaffold1上能比对到30k到100k区域的比对结果
samtools view abc.bam scaffold1:30000-100000 $gt; scaffold1_30k-100k.sam

f.根据fasta文件，将 header 加入到 sam 或 bam 文件中
samtools view -T genome.fasta -h scaffold1.sam > scaffold1.h.sam

g.查看bwa比对结果中比对上基因组的unique mapped reads
samtools view xx.bam |grep "XT：A：U" | wc -l
```
测序数据的双端的，那么sam文件的第3列是reads1的比对情况，第6列是reads2的比对情况。所以未比对成功的测序数据可以分成3类，仅reads1，仅reads2，和两端reads都没有比对成功。
也可以用下面的代码分步提取这3类未比对成功的reads:
```
samtools view -u  -f 4 -F264 alignments.bam  > tmps1.bam
samtools view -u -f 8 -F 260 alignments.bam  > tmps2.bam
samtools view -u -f 12 -F 256 alignments.bam > tmps3.bam
samtools merge -u - tmps[123].bam | samtools sort -n - unmapped
bamToFastq -bam unmapped.bam -fq1 unmapped_reads1.fastq -fq2 unmapped_reads2.fastq
```
```bash
samtools view sample_sorted.bam chr1:1234-123456samtools flagstat sample_sorted.bam
```

#### 排序
```bash
samtools sort sample.bam sort_default
samtools sort -n sample.bam sort_left
```
Sort alignments by leftmost coordinates, or by read name when -n is used.  默认按照染色体位置进行排序，而-n参数则是根据read名进行排序。
# QC软件
* Picard https://broadinstitute.github.io/picard/ 
* RSeQC http://rseqc.sourceforge.net/ 
* Qualimap http://qualimap.bioinfo.cipf.es/ 
