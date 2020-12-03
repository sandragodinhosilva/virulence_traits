# Virulence Factors (VF) identification in bacterial genomes
A tutorial on how to annotate genomes with virulence factors.

### Summary
* [1. Workflow description](#tool)
* [2. Installation](#installation)
* [3. How to run](#run)
* [References](#references)
* [Author - contact](#author---contact)

## 1. <a name="tool"></a>Workflow description
This a small pipeline to annotate bacterial genomes regarding virulence factors. \
The database in use is the following: http://www.mgc.ac.cn/VFs/main.htm


## 2. <a name="installation"></a>Installation
```
conda create -n virulence_analysis
conda activate virulence_analysis
conda install -c conda-forge -c bioconda -c defaults prokka
conda install pandas 
```
### 2.1 Download VFDB database (Faa format) and create local dabase
```
#Download:
wget http://www.mgc.ac.cn/VFs/Down/VFDB_setB_pro.fas.gz
#Decompress:
gzip -d VFDB_setB_pro.fas.gz
#Make database:
makeblastdb -in VFDB_setB_pro.fas -dbtype prot -input_type fasta
```

## 3. <a name="run"></a>How to run
**Necessary input:** 
Annotated genomes (in Faa format) in a single directory.

### 3.1 Perform a blastp against the database with all genomes
```
#In the folder with the genomes (Faa format):
for i in *.faa; do blastp -query $i -db VFDB_setB_pro.fas -out ${i%.*}_merops.txt -evalue 1E-5 -outfmt 6 -num_threads 8; done
```
Note: cut-off is set for **E-value 1e-5**. If you want to change it, just modify it in the above command.

### 3.2 Run script that unifies results
```
python 3 virulence_annotation.py /path/to/directory
```

### Output files:
- counts table with virulence factors per genome

### <a name="references"></a>References
Liu B, Zheng DD, Jin Q, Chen LH and Yang J, 2019. VFDB 2019: a comparative pathogenomic platform with an interactive web interface. Nucleic Acids Res. 47(D1):D687-D692.

### <a name="author---contact"></a>How to cite:

### Author
Sandra Godinho Silva \
sandragodinhosilva@gmail.com
