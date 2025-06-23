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
mkdir -p contigs
ln -s /home/meta/meta/pm/spades/pm1151/contigs.fasta contigs/pm1151_contigs.fasta
ln -s /home/meta/meta/pm/spades/pm1154/contigs.fasta contigs/pm1154_contigs.fasta
ln -s /home/meta/meta/pm/spades/pm1161/contigs.fasta contigs/pm1161_contigs.fasta
ln -s /home/meta/meta/pm/spades/pm2260/contigs.fasta contigs/pm2260_contigs.fasta
ln -s /home/meta/meta/pm/spades/pm2439/contigs.fasta contigs/pm2439_contigs.fasta
ln -s /home/meta/meta/pm/spades/pm2512/contigs.fasta contigs/pm2512_contigs.fasta
ln -s /home/meta/meta/pm/spades/pm2523/contigs.fasta contigs/pm2523_contigs.fasta
#!/bin/bash
abricate.sh

# Create output directory if it doesn't exist
mkdir -p abricat

# Loop through all pm*.fasta files in the contig directory
for fasta in contig/pm*.fasta; do
    # Extract the base name without directory and extension
    sample=$(basename "$fasta" .fasta)
    # Run abricate and output to abricat/resfinder_<sample>.txt
    abricate --db resfinder "$fasta" --threads 24 > "abricat/resfinder_${sample}.txt"
done

bash abricat.sh
abricate --summary *.txt > abricate_summary.txt
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
