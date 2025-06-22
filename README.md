# Xshell
质量控制fastqc
fastqc -o data -t 2 pm1151_1.qf.gz -o data pm1151_2.fq.gz
基因组组装spades
nohup spades.py	-1	data/pm1151_1.fq.gz	-2 data/pm1151_2.fq.gz -o	spades --careful -t 24 -m 100 &
组装质量评估quast
quast.py -o quast/pm1151 spades/pm1151/contigs.fasta
基因组注释prokka

耐药基因Abricate
abricate --db resfinder ./spades/pm1151/contigs.fasta > abricat/resfinder_pm1151.txt
毒力基因abricate-Virulence
abricate --db vfdb ./spades/pm1151/contigs.fasta > abricate-Virulence/vfdb_pm1151.txt
质粒检测plasmid
abricate -db plasmidfinder ./spades/pm1151/contigs.fasta > abricate-Plasmid/plasmid_pm1151.txt
核心基因组比对与构建系统发育树roary

基于参考序列的比对与SNP鉴定snippy
snippy --cpus 4 --o snippy/pm1151_pm1151 --ref reference.fasta --R1 data/pm1151_1.fq.gz --R2 data/pm1151_2.fq.gz
泛基因组可视化
使用工具：GView、Anvi'o、Roary-plots或Phandango
功能分析
COG或KEGG注释

建立文件 mkdir
rm -rf 删除目录和文件
touch创建新文件
cat 查看文件内容
head 查看文件前几行
mv 文件名 / /
cp...
