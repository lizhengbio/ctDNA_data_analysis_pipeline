# Circulating tumour DNA whole-exome sequencing pipeline

This repository describes pipeline for analysing data from whole-exome sequencing (WES) of circulating tumour DNA (ctDNA).
The pipeline is implemented using ctDNA from plasma samples derived from pancreatic cancer patients. The analyses are conducted on [QMUL Apocrita (**sm11**) High Performance Computing](https://docs.hpc.qmul.ac.uk/) (HPC) cluster in the following directory<br>

*/data/BCI-BioInformatics/PC_ctDNA/WES_data*

#### Start with loading modules and installing necessary tools

- *[SAMtools](http://samtools.sourceforge.net/)*
- *[Burrows-Wheeler Alignmer](http://bio-bwa.sourceforge.net/)* (*BWA*)
- *[Picard](https://broadinstitute.github.io/picard/)*


```
    module load samtools
    module load bwa
```

**NOTE**: Check if the most recent *BWA* is installed

```
bwa
```

If not, download the most recent version (0.7.15 on 30.11.2016) from [here](https://sourceforge.net/projects/bio-bwa/files/) and install in home directory on *sm11* (*$HOME/applications*)

```
tar -xjf bwa-0.7.15.tar.bz2  
cd bwa-0.7.15
make
cp bwa $HOME/applications
```

Download *Picard* from [here](https://github.com/broadinstitute/picard/zipball/master) and install in home directory on *sm11* (*$HOME/applications*)
<br><br>

## Alignment with *BWA*

----------------------
#### 1. Construct the FM-index for the reference genome

```
mkdir /data/BCI-BioInformatics/Jun/reference_hg38/index_bwa_0.7.15
cd /data/BCI-BioInformatics/Jun/reference_hg38/
```

----------------------
#### 2. Construct index using the '*bwtsw*' algorithm implemented in *BWT-SW*

This method is recommended for *BWA-MEM* alignment algorithm.

```
mkdir index_bwa_0.7.15_bwtsw
cp hg38.fa index_bwa_0.7.15_bwtsw
cd index_bwa_0.7.15_bwtsw
$HOME/applications/bwa index -p hg38bwa -a bwtsw /data/BCI-BioInformatics/Jun/reference_hg38/index_bwa_0.7.15_bwtsw/hg38.fa
```

----------------------
#### 3. Perform alignment using '*mem*' algorithm

*BWA-MEM* is generally recommended for high-quality queries as it is faster and more accurate. For this use the index generated '*bwtsw*' algorithm.

Paramter | Value | Description
------------ | ------------ | ------------
-M | NA | Mark shorter split hits as secondary (for Picard compatibility)
-t | 4 | Number of threads
<br />

Run *[BWA_mem.sh](https://github.research.its.qmul.ac.uk/hfw456/ctDNA_WES_pipeline/blob/master/BWA_mem.sh)* script for each sample

* **Sequencing batch 1**

Sample 45_1_B
```
nohup ./BWA_mem.sh 45_1_B SLX-12721.iPCRtagT002.HGJWLBBXX.s_5.r_1.fq.gz SLX-12721.iPCRtagT002.HGJWLBBXX.s_5.r_2.fq.gz > SLX-12721.iPCRtagT002.HGJWLBBXX.s_5.BWA_mem.log &
```

Sample 45_2_C
```
nohup ./BWA_mem.sh 45_2_C SLX-12721.iPCRtagT004.HGJWLBBXX.s_5.r_1.fq.gz SLX-12721.iPCRtagT004.HGJWLBBXX.s_5.r_2.fq.gz > SLX-12721.iPCRtagT004.HGJWLBBXX.s_5.BWA_mem.log &
```

Sample 45_3_D
```
nohup ./BWA_mem.sh 45_3_D SLX-12721.iPCRtagT005.HGJWLBBXX.s_5.r_1.fq.gz SLX-12721.iPCRtagT005.HGJWLBBXX.s_5.r_2.fq.gz > SLX-12721.iPCRtagT005.HGJWLBBXX.s_5.BWA_mem.log &
```

Sample 45_4_E
```
nohup ./BWA_mem.sh 45_4_E SLX-12721.iPCRtagT006.HGJWLBBXX.s_5.r_1.fq.gz SLX-12721.iPCRtagT006.HGJWLBBXX.s_5.r_2.fq.gz > SLX-12721.iPCRtagT006.HGJWLBBXX.s_5.BWA_mem.log &
```

Sample 95_1_A
```
nohup ./BWA_mem.sh 95_1_A SLX-12721.iPCRtagT007.HGJWLBBXX.s_5.r_1.fq.gz SLX-12721.iPCRtagT007.HGJWLBBXX.s_5.r_2.fq.gz > SLX-12721.iPCRtagT007.HGJWLBBXX.s_5.BWA_mem.log &
```

Sample 95_2_B
```
nohup ./BWA_mem.sh 95_2_B SLX-12721.iPCRtagT009.HGJWLBBXX.s_5.r_1.fq.gz SLX-12721.iPCRtagT009.HGJWLBBXX.s_5.r_2.fq.gz > SLX-12721.iPCRtagT009.HGJWLBBXX.s_5.BWA_mem.log &
```

Sample 95_3_C
```
nohup ./BWA_mem.sh 95_3_C SLX-12721.iPCRtagT010.HGJWLBBXX.s_5.r_1.fq.gz SLX-12721.iPCRtagT010.HGJWLBBXX.s_5.r_2.fq.gz > SLX-12721.iPCRtagT010.HGJWLBBXX.s_5.BWA_mem.log &
```

Sample 95_4_D
```
nohup ./BWA_mem.sh 95_4_D SLX-12721.iPCRtagT012.HGJWLBBXX.s_5.r_1.fq.gz SLX-12721.iPCRtagT012.HGJWLBBXX.s_5.r_2.fq.gz > SLX-12721.iPCRtagT012.HGJWLBBXX.s_5.BWA_mem.log &
```
<br>

* **Sequencing batch 2**

Sample 45_1_B
```
nohup ./BWA_mem.sh 45_1_B.2 SLX-12721.iPCRtagT002.HGYHFBBXX.s_2.r_1.fq.gz SLX-12721.iPCRtagT002.HGYHFBBXX.s_2.r_2.fq.gz > SLX-12721.iPCRtagT002.HGYHFBBXX.s_2.BWA_mem.log &
```

Sample 45_2_C
```
nohup ./BWA_mem.sh 45_2_C.2 SLX-12721.iPCRtagT004.HGYHFBBXX.s_2.r_1.fq.gz SLX-12721.iPCRtagT004.HGYHFBBXX.s_2.r_2.fq.gz > SLX-12721.iPCRtagT004.HGYHFBBXX.s_2.BWA_mem.log &
```

Sample 45_3_D
```
nohup ./BWA_mem.sh 45_3_D.2 SLX-12721.iPCRtagT005.HGYHFBBXX.s_2.r_1.fq.gz SLX-12721.iPCRtagT005.HGYHFBBXX.s_2.r_2.fq.gz > SLX-12721.iPCRtagT005.HGYHFBBXX.s_2.BWA_mem.log &
```

Sample 45_4_E
```
nohup ./BWA_mem.sh 45_4_E.2 SLX-12721.iPCRtagT006.HGYHFBBXX.s_2.r_1.fq.gz SLX-12721.iPCRtagT006.HGYHFBBXX.s_2.r_2.fq.gz > SLX-12721.iPCRtagT006.HGYHFBBXX.s_2.BWA_mem.log &
```

Sample 95_1_A
```
nohup ./BWA_mem.sh 95_1_A.2 SLX-12721.iPCRtagT007.HGYHFBBXX.s_2.r_1.fq.gz SLX-12721.iPCRtagT007.HGYHFBBXX.s_2.r_2.fq.gz > SLX-12721.iPCRtagT007.HGYHFBBXX.s_2.BWA_mem.log &
```

Sample 95_2_B
```
nohup ./BWA_mem.sh 95_2_B.2 SLX-12721.iPCRtagT009.HGYHFBBXX.s_2.r_1.fq.gz SLX-12721.iPCRtagT009.HGYHFBBXX.s_2.r_2.fq.gz > SLX-12721.iPCRtagT009.HGYHFBBXX.s_2.BWA_mem.log &
```

Sample 95_3_C
```
nohup ./BWA_mem.sh 95_3_C.2 SLX-12721.iPCRtagT010.HGYHFBBXX.s_2.r_1.fq.gz SLX-12721.iPCRtagT010.HGYHFBBXX.s_2.r_2.fq.gz > SLX-12721.iPCRtagT010.HGYHFBBXX.s_2.BWA_mem.log &
```

Sample 95_4_D
```
nohup ./BWA_mem.sh 95_4_D.2 SLX-12721.iPCRtagT012.HGYHFBBXX.s_2.r_1.fq.gz SLX-12721.iPCRtagT012.HGYHFBBXX.s_2.r_2.fq.gz > SLX-12721.iPCRtagT012.HGYHFBBXX.s_2.BWA_mem.log &
```
<br>

* **Sequencing batch 3**

Sample 45_1_B
```
nohup ./BWA_mem.sh 45_1_B.3 SLX-12721.iPCRtagT002.HGYHFBBXX.s_3.r_1.fq.gz SLX-12721.iPCRtagT002.HGYHFBBXX.s_3.r_2.fq.gz > SLX-12721.iPCRtagT002.HGYHFBBXX.s_3.BWA_mem.log &
```

Sample 45_2_C
```
nohup ./BWA_mem.sh 45_2_C.3 SLX-12721.iPCRtagT004.HGYHFBBXX.s_3.r_1.fq.gz SLX-12721.iPCRtagT004.HGYHFBBXX.s_3.r_2.fq.gz > SLX-12721.iPCRtagT004.HGYHFBBXX.s_3.BWA_mem.log &
```

Sample 45_3_D
```
nohup ./BWA_mem.sh 45_3_D.3 SLX-12721.iPCRtagT005.HGYHFBBXX.s_3.r_1.fq.gz SLX-12721.iPCRtagT005.HGYHFBBXX.s_3.r_2.fq.gz > SLX-12721.iPCRtagT005.HGYHFBBXX.s_3.BWA_mem.log &
```

Sample 45_4_E
```
nohup ./BWA_mem.sh 45_4_E.3 SLX-12721.iPCRtagT006.HGYHFBBXX.s_3.r_1.fq.gz SLX-12721.iPCRtagT006.HGYHFBBXX.s_3.r_2.fq.gz > SLX-12721.iPCRtagT006.HGYHFBBXX.s_3.BWA_mem.log &
```

Sample 95_1_A
```
nohup ./BWA_mem.sh 95_1_A.3 SLX-12721.iPCRtagT007.HGYHFBBXX.s_3.r_1.fq.gz SLX-12721.iPCRtagT007.HGYHFBBXX.s_3.r_2.fq.gz > SLX-12721.iPCRtagT007.HGYHFBBXX.s_3.BWA_mem.log &
```

Sample 95_2_B
```
nohup ./BWA_mem.sh 95_2_B.3 SLX-12721.iPCRtagT009.HGYHFBBXX.s_3.r_1.fq.gz SLX-12721.iPCRtagT009.HGYHFBBXX.s_3.r_2.fq.gz > SLX-12721.iPCRtagT009.HGYHFBBXX.s_3.BWA_mem.log &
```

Sample 95_3_C
```
nohup ./BWA_mem.sh 95_3_C.3 SLX-12721.iPCRtagT010.HGYHFBBXX.s_3.r_1.fq.gz SLX-12721.iPCRtagT010.HGYHFBBXX.s_3.r_2.fq.gz > SLX-12721.iPCRtagT010.HGYHFBBXX.s_3.BWA_mem.log &
```

Sample 95_4_D
```
nohup ./BWA_mem.sh 95_4_D.3 SLX-12721.iPCRtagT012.HGYHFBBXX.s_3.r_1.fq.gz SLX-12721.iPCRtagT012.HGYHFBBXX.s_3.r_2.fq.gz > SLX-12721.iPCRtagT012.HGYHFBBXX.s_3.BWA_mem.log &
```
<br>

* **Sequencing batch 4**

Sample 45_1_B
```
nohup ./BWA_mem.sh 45_1_B.4 SLX-12721.iPCRtagT002.HGYHFBBXX.s_4.r_1.fq.gz SLX-12721.iPCRtagT002.HGYHFBBXX.s_4.r_2.fq.gz > SLX-12721.iPCRtagT002.HGYHFBBXX.s_4.BWA_mem.log &
```

Sample 45_2_C
```
nohup ./BWA_mem.sh 45_2_C.4 SLX-12721.iPCRtagT004.HGYHFBBXX.s_4.r_1.fq.gz SLX-12721.iPCRtagT004.HGYHFBBXX.s_4.r_2.fq.gz > SLX-12721.iPCRtagT004.HGYHFBBXX.s_4.BWA_mem.log &
```

Sample 45_3_D
```
nohup ./BWA_mem.sh 45_3_D.4 SLX-12721.iPCRtagT005.HGYHFBBXX.s_4.r_1.fq.gz SLX-12721.iPCRtagT005.HGYHFBBXX.s_4.r_2.fq.gz > SLX-12721.iPCRtagT005.HGYHFBBXX.s_4.BWA_mem.log &
```

Sample 45_4_E
```
nohup ./BWA_mem.sh 45_4_E.4 SLX-12721.iPCRtagT006.HGYHFBBXX.s_4.r_1.fq.gz SLX-12721.iPCRtagT006.HGYHFBBXX.s_4.r_2.fq.gz > SLX-12721.iPCRtagT006.HGYHFBBXX.s_4.BWA_mem.log &
```

Sample 95_1_A
```
nohup ./BWA_mem.sh 95_1_A.4 SLX-12721.iPCRtagT007.HGYHFBBXX.s_4.r_1.fq.gz SLX-12721.iPCRtagT007.HGYHFBBXX.s_4.r_2.fq.gz > SLX-12721.iPCRtagT007.HGYHFBBXX.s_4.BWA_mem.log &
```

Sample 95_2_B
```
nohup ./BWA_mem.sh 95_2_B.4 SLX-12721.iPCRtagT009.HGYHFBBXX.s_4.r_1.fq.gz SLX-12721.iPCRtagT009.HGYHFBBXX.s_4.r_2.fq.gz > SLX-12721.iPCRtagT009.HGYHFBBXX.s_4.BWA_mem.log &
```

Sample 95_3_C
```
nohup ./BWA_mem.sh 95_3_C.4 SLX-12721.iPCRtagT010.HGYHFBBXX.s_4.r_1.fq.gz SLX-12721.iPCRtagT010.HGYHFBBXX.s_4.r_2.fq.gz > SLX-12721.iPCRtagT010.HGYHFBBXX.s_4.BWA_mem.log &
```

Sample 95_4_D
```
nohup ./BWA_mem.sh 95_4_D.4 SLX-12721.iPCRtagT012.HGYHFBBXX.s_4.r_1.fq.gz SLX-12721.iPCRtagT012.HGYHFBBXX.s_4.r_2.fq.gz > SLX-12721.iPCRtagT012.HGYHFBBXX.s_4.BWA_mem.log &
```

----------------------
#### 4. Convert SAM to BAM files using Picard

Run *[Picard_SAM2BAM.sh](https://github.research.its.qmul.ac.uk/hfw456/ctDNA_WES_pipeline/blob/master/Picard_SAM2BAM.sh)* script for each sample

* **Sequencing batch 1**

Sample 45_1_B
```
nohup ./Picard_SAM2BAM.sh  45_1_B > 45_1_B.Picard_SAM2BAM.log &
```

Sample 45_2_C
```
nohup ./Picard_SAM2BAM.sh  45_2_C > 45_2_C.Picard_SAM2BAM.log &
```

Sample 45_3_D
```
nohup ./Picard_SAM2BAM.sh  45_3_D > 45_3_D.Picard_SAM2BAM.log &
```

Sample 45_4_E
```
nohup ./Picard_SAM2BAM.sh  45_4_E > 45_4_E.Picard_SAM2BAM.log &
```

Sample 95_1_A
```
nohup ./Picard_SAM2BAM.sh  95_1_A > 95_1_A.Picard_SAM2BAM.log &
```

Sample 95_2_B
```
nohup ./Picard_SAM2BAM.sh  95_2_B > 95_2_B.Picard_SAM2BAM.log &
```

Sample 95_3_C
```
nohup ./Picard_SAM2BAM.sh  95_3_C > 95_3_C.Picard_SAM2BAM.log &
```

Sample 95_4_D
```
nohup ./Picard_SAM2BAM.sh  95_4_D > 95_4_D.Picard_SAM2BAM.log &
```
<br>

* **Sequencing batch 2**

Sample 45_1_B
```
nohup ./Picard_SAM2BAM.sh  45_1_B.2 > 45_1_B.2.Picard_SAM2BAM.log &
```

Sample 45_2_C
```
nohup ./Picard_SAM2BAM.sh  45_2_C.2 > 45_2_C.2.Picard_SAM2BAM.log &
```

Sample 45_3_D
```
nohup ./Picard_SAM2BAM.sh  45_3_D.2 > 45_3_D.2.Picard_SAM2BAM.log &
```

Sample 45_4_E
```
nohup ./Picard_SAM2BAM.sh  45_4_E.2 > 45_4_E.2.Picard_SAM2BAM.log &
```

Sample 95_1_A
```
nohup ./Picard_SAM2BAM.sh  95_1_A.2 > 95_1_A.2.Picard_SAM2BAM.log &
```

Sample 95_2_B
```
nohup ./Picard_SAM2BAM.sh  95_2_B.2 > 95_2_B.2.Picard_SAM2BAM.log &
```

Sample 95_3_C
```
nohup ./Picard_SAM2BAM.sh  95_3_C.2 > 95_3_C.2.Picard_SAM2BAM.log &
```

Sample 95_4_D
```
nohup ./Picard_SAM2BAM.sh  95_4_D.2 > 95_4_D.2.Picard_SAM2BAM.log &
```
<br>

* **Sequencing batch 3**

Sample 45_1_B
```
nohup ./Picard_SAM2BAM.sh  45_1_B.3 > 45_1_B.3.Picard_SAM2BAM.log &
```

Sample 45_2_C
```
nohup ./Picard_SAM2BAM.sh  45_2_C.3 > 45_2_C.3.Picard_SAM2BAM.log &
```

Sample 45_3_D
```
nohup ./Picard_SAM2BAM.sh  45_3_D.3 > 45_3_D.3.Picard_SAM2BAM.log &
```

Sample 45_4_E
```
nohup ./Picard_SAM2BAM.sh  45_4_E.3 > 45_4_E.3.Picard_SAM2BAM.log &
```

Sample 95_1_A
```
nohup ./Picard_SAM2BAM.sh  95_1_A.3 > 95_1_A.3.Picard_SAM2BAM.log &
```

Sample 95_2_B
```
nohup ./Picard_SAM2BAM.sh  95_2_B.3 > 95_2_B.3.Picard_SAM2BAM.log &
```

Sample 95_3_C
```
nohup ./Picard_SAM2BAM.sh  95_3_C.3 > 95_3_C.3.Picard_SAM2BAM.log &
```

Sample 95_4_D
```
nohup ./Picard_SAM2BAM.sh  95_4_D.3 > 95_4_D.3.Picard_SAM2BAM.log &
```
<br>

* **Sequencing batch 4**

Sample 45_1_B
```
nohup ./Picard_SAM2BAM.sh  45_1_B.4 > 45_1_B.4.Picard_SAM2BAM.log &
```

Sample 45_2_C
```
nohup ./Picard_SAM2BAM.sh  45_2_C.4 > 45_2_C.4.Picard_SAM2BAM.log &
```

Sample 45_3_D
```
nohup ./Picard_SAM2BAM.sh  45_3_D.4 > 45_3_D.4.Picard_SAM2BAM.log &
```

Sample 45_4_E
```
nohup ./Picard_SAM2BAM.sh  45_4_E.4 > 45_4_E.4.Picard_SAM2BAM.log &
```

Sample 95_1_A
```
nohup ./Picard_SAM2BAM.sh  95_1_A.4 > 95_1_A.4.Picard_SAM2BAM.log &
```

Sample 95_2_B
```
nohup ./Picard_SAM2BAM.sh  95_2_B.4 > 95_2_B.4.Picard_SAM2BAM.log &
```

Sample 95_3_C
```
nohup ./Picard_SAM2BAM.sh  95_3_C.4 > 95_3_C.4.Picard_SAM2BAM.log &
```

Sample 95_4_D
```
nohup ./Picard_SAM2BAM.sh  95_4_D.4 > 95_4_D.4.Picard_SAM2BAM.log &
```

----------------------
#### 5. Mark PCR duplicates using Picard

Run *[Picard_markDupl.sh](https://github.research.its.qmul.ac.uk/hfw456/ctDNA_WES_pipeline/blob/master/Picard_markDupl.sh)* script for each sample

* **Sequencing batch 1**

Sample 45_1_B
```
nohup ./Picard_markDupl.sh  45_1_B > 45_1_B.Picard_markDupl.log &
```

Sample 45_2_C
```
nohup ./Picard_markDupl.sh  45_2_C > 45_2_C.Picard_markDupl.log &
```

Sample 45_3_D
```
nohup ./Picard_markDupl.sh  45_3_D > 45_3_D.Picard_markDupl.log &
```

Sample 45_4_E
```
nohup ./Picard_markDupl.sh  45_4_E > 45_4_E.Picard_markDupl.log &
```

Sample 95_1_A
```
nohup ./Picard_markDupl.sh  95_1_A > 95_1_A.Picard_markDupl.log &
```

Sample 95_2_B
```
nohup ./Picard_markDupl.sh  95_2_B > 95_2_B.Picard_markDupl.log &
```

Sample 95_3_C
```
nohup ./Picard_markDupl.sh  95_3_C > 95_3_C.Picard_markDupl.log &
```

Sample 95_4_D
```
nohup ./Picard_markDupl.sh  95_4_D > 95_4_D.Picard_markDupl.log &
```
<br>

* **Sequencing batch 2**

Sample 45_1_B
```
nohup ./Picard_markDupl.sh  45_1_B.2 > 45_1_B.2.Picard_markDupl.log &
```

Sample 45_2_C
```
nohup ./Picard_markDupl.sh  45_2_C.2 > 45_2_C.2.Picard_markDupl.log &
```

Sample 45_3_D
```
nohup ./Picard_markDupl.sh  45_3_D.2 > 45_3_D.2.Picard_markDupl.log &
```

Sample 45_4_E
```
nohup ./Picard_markDupl.sh  45_4_E.2 > 45_4_E.2.Picard_markDupl.log &
```

Sample 95_1_A
```
nohup ./Picard_markDupl.sh  95_1_A.2 > 95_1_A.2.Picard_markDupl.log &
```

Sample 95_2_B
```
nohup ./Picard_markDupl.sh  95_2_B.2 > 95_2_B.2.Picard_markDupl.log &
```

Sample 95_3_C
```
nohup ./Picard_markDupl.sh  95_3_C.2 > 95_3_C.2.Picard_markDupl.log &
```

Sample 95_4_D
```
nohup ./Picard_markDupl.sh  95_4_D.2 > 95_4_D.2.Picard_markDupl.log &
```
<br>

* **Sequencing batch 3**

Sample 45_1_B
```
nohup ./Picard_markDupl.sh  45_1_B.3 > 45_1_B.3.Picard_markDupl.log &
```

Sample 45_2_C
```
nohup ./Picard_markDupl.sh  45_2_C.3 > 45_2_C.3.Picard_markDupl.log &
```

Sample 45_3_D
```
nohup ./Picard_markDupl.sh  45_3_D.3 > 45_3_D.3.Picard_markDupl.log &
```

Sample 45_4_E
```
nohup ./Picard_markDupl.sh  45_4_E.3 > 45_4_E.3.Picard_markDupl.log &
```

Sample 95_1_A
```
nohup ./Picard_markDupl.sh  95_1_A.3 > 95_1_A.3.Picard_markDupl.log &
```

Sample 95_2_B
```
nohup ./Picard_markDupl.sh  95_2_B.3 > 95_2_B.3.Picard_markDupl.log &
```

Sample 95_3_C
```
nohup ./Picard_markDupl.sh  95_3_C.3 > 95_3_C.3.Picard_markDupl.log &
```

Sample 95_4_D
```
nohup ./Picard_markDupl.sh  95_4_D.3 > 95_4_D.3.Picard_markDupl.log &
```
<br>

* **Sequencing batch 4**

Sample 45_1_B
```
nohup ./Picard_markDupl.sh  45_1_B.4 > 45_1_B.4.Picard_markDupl.log &
```

Sample 45_2_C
```
nohup ./Picard_markDupl.sh  45_2_C.4 > 45_2_C.4.Picard_markDupl.log &
```

Sample 45_3_D
```
nohup ./Picard_markDupl.sh  45_3_D.4 > 45_3_D.4.Picard_markDupl.log &
```

Sample 45_4_E
```
nohup ./Picard_markDupl.sh  45_4_E.4 > 45_4_E.4.Picard_markDupl.log &
```

Sample 95_1_A
```
nohup ./Picard_markDupl.sh  95_1_A.4 > 95_1_A.4.Picard_markDupl.log &
```

Sample 95_2_B
```
nohup ./Picard_markDupl.sh  95_2_B.4 > 95_2_B.4.Picard_markDupl.log &
```

Sample 95_3_C
```
nohup ./Picard_markDupl.sh  95_3_C.4 > 95_3_C.4.Picard_markDupl.log &
```

Sample 95_4_D
```
nohup ./Picard_markDupl.sh  95_4_D.4 > 95_4_D.4.Picard_markDupl.log &
```

----------------------
#### 6. Collect statistics on a bam file and then uses plot-bamstat to create various graphs based on the bamstats file

Check the *marked.BAM* files with *SAMtools flagstat*

####################################################################################################

#### Collect statistics on a bam file and then uses plot-bamstat to create various graphs based on the bamstats file


#### Batch 1

# Sample 45_1_B
mkdir 45_1_B.marked.bam.stats

samtools stats 45_1_B.marked.bam > 45_1_B.marked.bam.stats/45_1_B.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_1_B.marked.bam.stats/45_1_B.marked.bam.stats.plot 45_1_B.marked.bam.stats/45_1_B.marked.bam.stats


# Sample 45_2_C
mkdir 45_2_C.marked.bam.stats

samtools stats 45_2_C.marked.bam > 45_2_C.marked.bam.stats/45_2_C.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_2_C.marked.bam.stats/45_2_C.marked.bam.stats.plot 45_2_C.marked.bam.stats/45_2_C.marked.bam.stats


# Sample 45_3_D
mkdir 45_3_D.marked.bam.stats

samtools stats 45_3_D.marked.bam > 45_3_D.marked.bam.stats/45_3_D.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_3_D.marked.bam.stats/45_3_D.marked.bam.stats.plot 45_3_D.marked.bam.stats/45_3_D.marked.bam.stats


# Sample 45_4_E
mkdir 45_4_E.marked.bam.stats

samtools stats 45_4_E.marked.bam > 45_4_E.marked.bam.stats/45_4_E.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_4_E.marked.bam.stats/45_4_E.marked.bam.stats.plot 45_4_E.marked.bam.stats/45_4_E.marked.bam.stats


# Sample 95_1_A
mkdir 95_1_A.marked.bam.stats

samtools stats 95_1_A.marked.bam > 95_1_A.marked.bam.stats/95_1_A.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_1_A.marked.bam.stats/95_1_A.marked.bam.stats.plot 95_1_A.marked.bam.stats/95_1_A.marked.bam.stats


# Sample 95_2_B
mkdir 95_2_B.marked.bam.stats

samtools stats 95_2_B.marked.bam > 95_2_B.marked.bam.stats/95_2_B.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_2_B.marked.bam.stats/95_2_B.marked.bam.stats.plot 95_2_B.marked.bam.stats/95_2_B.marked.bam.stats


# Sample 95_3_C
mkdir 95_3_C.marked.bam.stats

samtools stats 95_3_C.marked.bam > 95_3_C.marked.bam.stats/95_3_C.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_3_C.marked.bam.stats/95_3_C.marked.bam.stats.plot 95_3_C.marked.bam.stats/95_3_C.marked.bam.stats


# Sample 95_4_D
mkdir 95_4_D.marked.bam.stats

samtools stats 95_4_D.marked.bam > 95_4_D.marked.bam.stats/95_4_D.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_4_D.marked.bam.stats/95_4_D.marked.bam.stats.plot 95_4_D.marked.bam.stats/95_4_D.marked.bam.stats


#### Batch 2

# Sample 45_1_B
mkdir 45_1_B.2.marked.bam.stats

samtools stats 45_1_B.2.marked.bam > 45_1_B.2.marked.bam.stats/45_1_B.2.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_1_B.2.marked.bam.stats/45_1_B.2.marked.bam.stats.plot 45_1_B.2.marked.bam.stats/45_1_B.2.marked.bam.stats


# Sample 45_2_C
mkdir 45_2_C.2.marked.bam.stats

samtools stats 45_2_C.2.marked.bam > 45_2_C.2.marked.bam.stats/45_2_C.2.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_2_C.2.marked.bam.stats/45_2_C.2.marked.bam.stats.plot 45_2_C.2.marked.bam.stats/45_2_C.2.marked.bam.stats


# Sample 45_3_D
mkdir 45_3_D.2.marked.bam.stats

samtools stats 45_3_D.2.marked.bam > 45_3_D.2.marked.bam.stats/45_3_D.2.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_3_D.2.marked.bam.stats/45_3_D.2.marked.bam.stats.plot 45_3_D.2.marked.bam.stats/45_3_D.2.marked.bam.stats


# Sample 45_4_E
mkdir 45_4_E.2.marked.bam.stats

samtools stats 45_4_E.2.marked.bam > 45_4_E.2.marked.bam.stats/45_4_E.2.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_4_E.2.marked.bam.stats/45_4_E.2.marked.bam.stats.plot 45_4_E.2.marked.bam.stats/45_4_E.2.marked.bam.stats


# Sample 95_1_A
mkdir 95_1_A.2.marked.bam.stats

samtools stats 95_1_A.2.marked.bam > 95_1_A.2.marked.bam.stats/95_1_A.2.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_1_A.2.marked.bam.stats/95_1_A.2.marked.bam.stats.plot 95_1_A.2.marked.bam.stats/95_1_A.2.marked.bam.stats


# Sample 95_2_B
mkdir 95_2_B.2.marked.bam.stats

samtools stats 95_2_B.2.marked.bam > 95_2_B.2.marked.bam.stats/95_2_B.2.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_2_B.2.marked.bam.stats/95_2_B.2.marked.bam.stats.plot 95_2_B.2.marked.bam.stats/95_2_B.2.marked.bam.stats


# Sample 95_3_C
mkdir 95_3_C.2.marked.bam.stats

samtools stats 95_3_C.2.marked.bam > 95_3_C.2.marked.bam.stats/95_3_C.2.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_3_C.2.marked.bam.stats/95_3_C.2.marked.bam.stats.plot 95_3_C.2.marked.bam.stats/95_3_C.2.marked.bam.stats


# Sample 95_4_D
mkdir 95_4_D.2.marked.bam.stats

samtools stats 95_4_D.2.marked.bam > 95_4_D.2.marked.bam.stats/95_4_D.2.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_4_D.2.marked.bam.stats/95_4_D.2.marked.bam.stats.plot 95_4_D.2.marked.bam.stats/95_4_D.2.marked.bam.stats


#### Batch 3

# Sample 45_1_B
mkdir 45_1_B.3.marked.bam.stats

samtools stats 45_1_B.3.marked.bam > 45_1_B.3.marked.bam.stats/45_1_B.3.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_1_B.3.marked.bam.stats/45_1_B.3.marked.bam.stats.plot 45_1_B.3.marked.bam.stats/45_1_B.3.marked.bam.stats


# Sample 45_2_C
mkdir 45_2_C.3.marked.bam.stats

samtools stats 45_2_C.3.marked.bam > 45_2_C.3.marked.bam.stats/45_2_C.3.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_2_C.3.marked.bam.stats/45_2_C.3.marked.bam.stats.plot 45_2_C.3.marked.bam.stats/45_2_C.3.marked.bam.stats


# Sample 45_3_D
mkdir 45_3_D.3.marked.bam.stats

samtools stats 45_3_D.3.marked.bam > 45_3_D.3.marked.bam.stats/45_3_D.3.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_3_D.3.marked.bam.stats/45_3_D.3.marked.bam.stats.plot 45_3_D.3.marked.bam.stats/45_3_D.3.marked.bam.stats


# Sample 45_4_E
mkdir 45_4_E.3.marked.bam.stats

samtools stats 45_4_E.3.marked.bam > 45_4_E.3.marked.bam.stats/45_4_E.3.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_4_E.3.marked.bam.stats/45_4_E.3.marked.bam.stats.plot 45_4_E.3.marked.bam.stats/45_4_E.3.marked.bam.stats


# Sample 95_1_A
mkdir 95_1_A.3.marked.bam.stats

samtools stats 95_1_A.3.marked.bam > 95_1_A.3.marked.bam.stats/95_1_A.3.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_1_A.3.marked.bam.stats/95_1_A.3.marked.bam.stats.plot 95_1_A.3.marked.bam.stats/95_1_A.3.marked.bam.stats


# Sample 95_2_B
mkdir 95_2_B.3.marked.bam.stats

samtools stats 95_2_B.3.marked.bam > 95_2_B.3.marked.bam.stats/95_2_B.3.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_2_B.3.marked.bam.stats/95_2_B.3.marked.bam.stats.plot 95_2_B.3.marked.bam.stats/95_2_B.3.marked.bam.stats


# Sample 95_3_C
mkdir 95_3_C.3.marked.bam.stats

samtools stats 95_3_C.3.marked.bam > 95_3_C.3.marked.bam.stats/95_3_C.3.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_3_C.3.marked.bam.stats/95_3_C.3.marked.bam.stats.plot 95_3_C.3.marked.bam.stats/95_3_C.3.marked.bam.stats


# Sample 95_4_D
mkdir 95_4_D.3.marked.bam.stats

samtools stats 95_4_D.3.marked.bam > 95_4_D.3.marked.bam.stats/95_4_D.3.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_4_D.3.marked.bam.stats/95_4_D.3.marked.bam.stats.plot 95_4_D.3.marked.bam.stats/95_4_D.3.marked.bam.stats


#### Batch 4

# Sample 45_1_B
mkdir 45_1_B.4.marked.bam.stats

samtools stats 45_1_B.4.marked.bam > 45_1_B.4.marked.bam.stats/45_1_B.4.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_1_B.4.marked.bam.stats/45_1_B.4.marked.bam.stats.plot 45_1_B.4.marked.bam.stats/45_1_B.4.marked.bam.stats


# Sample 45_2_C
mkdir 45_2_C.4.marked.bam.stats

samtools stats 45_2_C.4.marked.bam > 45_2_C.4.marked.bam.stats/45_2_C.4.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_2_C.4.marked.bam.stats/45_2_C.4.marked.bam.stats.plot 45_2_C.4.marked.bam.stats/45_2_C.4.marked.bam.stats


# Sample 45_3_D
mkdir 45_3_D.4.marked.bam.stats

samtools stats 45_3_D.4.marked.bam > 45_3_D.4.marked.bam.stats/45_3_D.4.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_3_D.4.marked.bam.stats/45_3_D.4.marked.bam.stats.plot 45_3_D.4.marked.bam.stats/45_3_D.4.marked.bam.stats


# Sample 45_4_E
mkdir 45_4_E.4.marked.bam.stats

samtools stats 45_4_E.4.marked.bam > 45_4_E.4.marked.bam.stats/45_4_E.4.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 45_4_E.4.marked.bam.stats/45_4_E.4.marked.bam.stats.plot 45_4_E.4.marked.bam.stats/45_4_E.4.marked.bam.stats


# Sample 95_1_A
mkdir 95_1_A.4.marked.bam.stats

samtools stats 95_1_A.4.marked.bam > 95_1_A.4.marked.bam.stats/95_1_A.4.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_1_A.4.marked.bam.stats/95_1_A.4.marked.bam.stats.plot 95_1_A.4.marked.bam.stats/95_1_A.4.marked.bam.stats


# Sample 95_2_B
mkdir 95_2_B.4.marked.bam.stats

samtools stats 95_2_B.4.marked.bam > 95_2_B.4.marked.bam.stats/95_2_B.4.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_2_B.4.marked.bam.stats/95_2_B.4.marked.bam.stats.plot 95_2_B.4.marked.bam.stats/95_2_B.4.marked.bam.stats


# Sample 95_3_C
mkdir 95_3_C.4.marked.bam.stats

samtools stats 95_3_C.4.marked.bam > 95_3_C.4.marked.bam.stats/95_3_C.4.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_3_C.4.marked.bam.stats/95_3_C.4.marked.bam.stats.plot 95_3_C.4.marked.bam.stats/95_3_C.4.marked.bam.stats


# Sample 95_4_D
mkdir 95_4_D.4.marked.bam.stats

samtools stats 95_4_D.4.marked.bam > 95_4_D.4.marked.bam.stats/95_4_D.4.marked.bam.stats
$SAMTOOLSDIR/misc/plot-bamstats -p 95_4_D.4.marked.bam.stats/95_4_D.4.marked.bam.stats.plot 95_4_D.4.marked.bam.stats/95_4_D.4.marked.bam.stats



####################################################################################################

#### Calculate the coverage after marking PCR duplicates

# First, download the Agilent Human Exon V6 exome capture  bed files and use liftOver to change the coordinates from hg19 to hg38
# Note: need to remove the header before and add again after liftover! Do it on local machine

./liftOver /Users/marzec01/data/PC_ctDNA/WES_data/Agilent_Human_Exon_V6/S07604514_Covered.bed /Users/marzec01/Desktop/applications/liftOver/hg19ToHg38.over.chain.gz /Users/marzec01/data/PC_ctDNA/WES_data/Agilent_Human_Exon_V6/S07604514_Covered_hg38.bed /Users/marzec01/data/PC_ctDNA/WES_data/Agilent_Human_Exon_V6/S07604514_Covered_hg38unlifted.bed

#### Note: remove from the converted file unspecific contigs (chr1_KI270766v1_alt etc.)! Do it on local machine
grep '^chr[0-9XY]\{1,2\}\t' /Users/marzec01/data/PC_ctDNA/WES_data/Agilent_Human_Exon_V6/S07604514_Covered_hg38.bed > /Users/marzec01/data/PC_ctDNA/WES_data/Agilent_Human_Exon_V6/S07604514_Covered_hg38_clean.bed


#### Run 'GATK_coverage.sh' script


#### Batch 1

# Sample 45_1_B
nohup ./GATK_coverage.sh  45_1_B > 45_1_B.GATK_coverage.log &

# Sample 45_2_C
nohup ./GATK_coverage.sh  45_2_C > 45_2_C.GATK_coverage.log &

# Sample 45_3_D
nohup ./GATK_coverage.sh  45_3_D > 45_3_D.GATK_coverage.log &

# Sample 45_4_E
nohup ./GATK_coverage.sh  45_4_E > 45_4_E.GATK_coverage.log &

# Sample 95_1_A
nohup ./GATK_coverage.sh  95_1_A > 95_1_A.GATK_coverage.log &

# Sample 95_2_B
nohup ./GATK_coverage.sh  95_2_B > 95_2_B.GATK_coverage.log &

# Sample 95_3_C
nohup ./GATK_coverage.sh  95_3_C > 95_3_C.GATK_coverage.log &

# Sample 95_4_D
nohup ./GATK_coverage.sh  95_4_D > 95_4_D.GATK_coverage.log &


#### Batch 2

# Sample 45_1_B
nohup ./GATK_coverage.sh  45_1_B.2 > 45_1_B.2.GATK_coverage.log &

# Sample 45_2_C
nohup ./GATK_coverage.sh  45_2_C.2 > 45_2_C.2.GATK_coverage.log &

# Sample 45_3_D
nohup ./GATK_coverage.sh  45_3_D.2 > 45_3_D.2.GATK_coverage.log &

# Sample 45_4_E
nohup ./GATK_coverage.sh  45_4_E.2 > 45_4_E.2.GATK_coverage.log &

# Sample 95_1_A
nohup ./GATK_coverage.sh  95_1_A.2 > 95_1_A.2.GATK_coverage.log &

# Sample 95_2_B
nohup ./GATK_coverage.sh  95_2_B.2 > 95_2_B.2.GATK_coverage.log &

# Sample 95_3_C
nohup ./GATK_coverage.sh  95_3_C.2 > 95_3_C.2.GATK_coverage.log &

# Sample 95_4_D
nohup ./GATK_coverage.sh  95_4_D.2 > 95_4_D.2.GATK_coverage.log &


#### Batch 3

# Sample 45_1_B
nohup ./GATK_coverage.sh  45_1_B.3 > 45_1_B.3.GATK_coverage.log &

# Sample 45_2_C
nohup ./GATK_coverage.sh  45_2_C.3 > 45_2_C.3.GATK_coverage.log &

# Sample 45_3_D
nohup ./GATK_coverage.sh  45_3_D.3 > 45_3_D.3.GATK_coverage.log &

# Sample 45_4_E
nohup ./GATK_coverage.sh  45_4_E.3 > 45_4_E.3.GATK_coverage.log &

# Sample 95_1_A
nohup ./GATK_coverage.sh  95_1_A.3 > 95_1_A.3.GATK_coverage.log &

# Sample 95_2_B
nohup ./GATK_coverage.sh  95_2_B.3 > 95_2_B.3.GATK_coverage.log &

# Sample 95_3_C
nohup ./GATK_coverage.sh  95_3_C.3 > 95_3_C.3.GATK_coverage.log &

# Sample 95_4_D
nohup ./GATK_coverage.sh  95_4_D.3 > 95_4_D.3.GATK_coverage.log &


#### Batch 4

# Sample 45_1_B
nohup ./GATK_coverage.sh  45_1_B.4 > 45_1_B.4.GATK_coverage.log &

# Sample 45_2_C
nohup ./GATK_coverage.sh  45_2_C.4 > 45_2_C.4.GATK_coverage.log &

# Sample 45_3_D
nohup ./GATK_coverage.sh  45_3_D.4 > 45_3_D.4.GATK_coverage.log &

# Sample 45_4_E
nohup ./GATK_coverage.sh  45_4_E.4 > 45_4_E.4.GATK_coverage.log &

# Sample 95_1_A
nohup ./GATK_coverage.sh  95_1_A.4 > 95_1_A.4.GATK_coverage.log &

# Sample 95_2_B
nohup ./GATK_coverage.sh  95_2_B.4 > 95_2_B.4.GATK_coverage.log &

# Sample 95_3_C
nohup ./GATK_coverage.sh  95_3_C.4 > 95_3_C.4.GATK_coverage.log &

# Sample 95_4_D
nohup ./GATK_coverage.sh  95_4_D.4 > 95_4_D.4.GATK_coverage.log &




# This creates 7 files per each sample:
1. no suffix: per locus coverage
2. _summary: total, mean, median, quartiles, and threshold proportions, aggregated over all bases
3. _statistics: coverage histograms (# locus with X coverage), aggregated over all bases
4. _interval_summary: total, mean, median, quartiles, and threshold proportions, aggregated per interval
5. _interval_statistics: 2x2 table of # of intervals covered to >= X depth in >=Y samples
6. _cumulative_coverage_counts: coverage histograms (# locus with >= X coverage), aggregated over all bases
7. _cumulative_coverage_proportions: proprotions of loci with >= X coverage, aggregated over all bases



####################################################################################################

#### According to Broad Institute recommendation for pre-processing data from multiplexed sequencing and multi-library designs ( https://software.broadinstitute.org/gatk/guide/article?id=3060 ), after running the initial steps of the pre-processing workflow (mapping, sorting and marking duplicates) separately on each individual BAM file, we merge the data to produce a single BAM file for each sample (aggregation). This is done by re-running Mark Duplicates, this time on all read group BAM files for a sample at the same time. The Indel Realignment and Base Recalibration is then run on the aggregated per-sample BAM files


#### Merge BAM files and mark duplicates per sample (aggregation + dedup)

#### Run 'Picard_merge_4BAMs_markDupl.sh' script

# Sample 45_1_B
nohup ./Picard_merge_4BAMs_markDupl.sh  45_1_B  45_1_B.recalib.bam  45_1_B.2.recalib.bam  45_1_B.3.recalib.bam  45_1_B.4.recalib.bam  >  45_1_B.Picard_merge_4BAMs_markDupl.log &


# Sample 45_2_C
nohup ./Picard_merge_4BAMs_markDupl.sh  45_2_C  45_2_C.recalib.bam  45_2_C.2.recalib.bam  45_2_C.3.recalib.bam  45_2_C.4.recalib.bam  >  45_2_C.Picard_merge_4BAMs_markDupl.log &


# Sample 45_3_D
nohup ./Picard_merge_4BAMs_markDupl.sh  45_3_D  45_3_D.recalib.bam  45_3_D.2.recalib.bam  45_3_D.3.recalib.bam  45_3_D.4.recalib.bam  >  45_3_D.Picard_merge_4BAMs_markDupl.log &


# Sample 45_4_E
nohup ./Picard_merge_4BAMs_markDupl.sh  45_4_E  45_4_E.recalib.bam  45_4_E.2.recalib.bam  45_4_E.3.recalib.bam  45_4_E.4.recalib.bam  >  45_4_E.Picard_merge_4BAMs_markDupl.log &


# Sample 95_1_A
nohup ./Picard_merge_4BAMs_markDupl.sh  95_1_A  95_1_A.recalib.bam  95_1_A.2.recalib.bam  95_1_A.3.recalib.bam  95_1_A.4.recalib.bam  >  95_1_A.Picard_merge_4BAMs_markDupl.log &


# Sample 95_2_B
nohup ./Picard_merge_4BAMs_markDupl.sh  95_2_B  95_2_B.recalib.bam  95_2_B.2.recalib.bam  95_2_B.3.recalib.bam  95_2_B.4.recalib.bam  >  95_2_B.Picard_merge_4BAMs_markDupl.log &


# Sample 95_3_C
nohup ./Picard_merge_4BAMs_markDupl.sh  95_3_C  95_3_C.recalib.bam  95_3_C.2.recalib.bam  95_3_C.3.recalib.bam  95_3_C.4.recalib.bam  >  95_3_C.Picard_merge_4BAMs_markDupl.log &


# Sample 95_4_D
nohup ./Picard_merge_4BAMs_markDupl.sh  95_4_D  95_4_D.recalib.bam  95_4_D.2.recalib.bam  95_4_D.3.recalib.bam  95_4_D.4.recalib.bam  >  95_4_D.Picard_merge_4BAMs_markDupl.log &




####################################################################################################

#### Perform local alignment around indels using GATK

#### First create the reference fasta sequence dictionary file
$HOME/applications/picard-tools-1.119/CreateSequenceDictionary.jar R= /data/BCI-BioInformatics/Jun/reference_hg38/hg38.fa O= /data/BCI-BioInformatics/Jun/reference_hg38/hg38.dict

#### NOTE: Local realignment is not necessary for variant callers that perform a haplotype assembly step, such as HaplotypeCaller or MuTect2. I perfrom this step anyway as it does not take much time (5-10h)


#### Run 'Picard_GATK_localAlign_indels.sh' script


# Sample 45_1_B
nohup ./Picard_GATK_localAlign_indels.sh  45_1_B.merged > 45_1_B.merged.Picard_GATK_localAlign_indels.log &

# Sample 45_2_C
nohup ./Picard_GATK_localAlign_indels.sh  45_2_C.merged > 45_2_C.merged.Picard_GATK_localAlign_indels.log &

# Sample 45_3_D
nohup ./Picard_GATK_localAlign_indels.sh  45_3_D.merged > 45_3_D.merged.Picard_GATK_localAlign_indels.log &

# Sample 45_4_E
nohup ./Picard_GATK_localAlign_indels.sh  45_4_E.merged > 45_4_E.merged.Picard_GATK_localAlign_indels.log &

# Sample 95_1_A
nohup ./Picard_GATK_localAlign_indels.sh  95_1_A.merged > 95_1_A.merged.Picard_GATK_localAlign_indels.log &

# Sample 95_2_B
nohup ./Picard_GATK_localAlign_indels.sh  95_2_B.merged > 95_2_B.merged.Picard_GATK_localAlign_indels.log &

# Sample 95_3_C
nohup ./Picard_GATK_localAlign_indels.sh  95_3_C.merged > 95_3_C.merged.Picard_GATK_localAlign_indels.log &

# Sample 95_4_D
nohup ./Picard_GATK_localAlign_indels.sh  95_4_D.merged > 95_4_D.merged.Picard_GATK_localAlign_indels.log &




####################################################################################################

#### Perform base quality score recalibration using GATK
#### Run 'GATK_baseQrecalib.sh' script


# Sample 45_1_B
nohup ./GATK_baseQrecalib.sh  45_1_B.merged > 45_1_B.merged.GATK_baseQrecalib.log &

# Sample 45_2_C
nohup ./GATK_baseQrecalib.sh  45_2_C.merged > 45_2_C.merged.GATK_baseQrecalib.log &

# Sample 45_3_D
nohup ./GATK_baseQrecalib.sh  45_3_D.merged > 45_3_D.merged.GATK_baseQrecalib.log &

# Sample 45_4_E
nohup ./GATK_baseQrecalib.sh  45_4_E.merged > 45_4_E.merged.GATK_baseQrecalib.log &

# Sample 95_1_A
nohup ./GATK_baseQrecalib.sh  95_1_A.merged > 95_1_A.merged.GATK_baseQrecalib.log &

# Sample 95_2_B
nohup ./GATK_baseQrecalib.sh  95_2_B.merged > 95_2_B.merged.GATK_baseQrecalib.log &

# Sample 95_3_C
nohup ./GATK_baseQrecalib.sh  95_3_C.merged > 95_3_C.merged.GATK_baseQrecalib.log &

# Sample 95_4_D
nohup ./GATK_baseQrecalib.sh  95_4_D.merged > 95_4_D.merged.GATK_baseQrecalib.log &



#### Remove redundant files for each sample
rm [sample_name].sam
rm [sample_name].bam
rm [sample_name].bai
rm [sample_name].merged.marked.bam
rm [sample_name].merged.marked.bai
rm [sample_name].merged.marked.realigned.bam
rm [sample_name].merged.marked.realigned.bai
rm [sample_name].merged.marked.realigned.fixed.bam
rm [sample_name].merged.marked.realigned.fixed.bai



####################################################################################################

----------------------
#### XXXX. Check the *merged.recalib.BAM* files with *SAMtools flagstat*

*[SAMtools flagstat](http://www.htslib.org/doc/samtools.html) flagstat* does a full pass through the input file to calculate statistics



samtools flagstat 45_1_B.merged.recalib.bam > 45_1_B.merged.recalib.flagstat.txt
samtools flagstat 45_2_C.merged.recalib.bam > 45_2_C.merged.recalib.flagstat.txt
samtools flagstat 45_3_D.merged.recalib.bam > 45_3_D.merged.recalib.flagstat.txt
samtools flagstat 45_4_E.merged.recalib.bam > 45_4_E.merged.recalib.flagstat.txt

samtools flagstat 95_1_A.merged.recalib.bam > 95_1_A.merged.recalib.flagstat.txt
samtools flagstat 95_2_B.merged.recalib.bam > 95_2_B.merged.recalib.flagstat.txt
samtools flagstat 95_3_C.merged.recalib.bam > 95_3_C.merged.recalib.flagstat.txt
samtools flagstat 95_4_D.merged.recalib.bam > 95_4_D.merged.recalib.flagstat.txt



####################################################################################################

#### Index merged BAM files

samtools index 45_1_B.merged.recalib.bam
samtools index 45_2_C.merged.recalib.bam
samtools index 45_3_D.merged.recalib.bam
samtools index 45_4_E.merged.recalib.bam

samtools index 95_1_A.merged.recalib.bam
samtools index 95_2_B.merged.recalib.bam
samtools index 95_3_C.merged.recalib.bam
samtools index 95_4_D.merged.recalib.bam



####################################################################################################

#### Call variants with MuTect 2

http://archive.broadinstitute.org/cancer/cga/mutect
http://archive.broadinstitute.org/cancer/cga/mutect_run
http://gatkforums.broadinstitute.org/gatk/discussion/6858/mutect-2
https://software.broadinstitute.org/gatk/gatkdocs/org_broadinstitute_gatk_tools_walkers_cancer_m2_MuTect2.php

#### First download and prepare COSMIC and dbSNP VCF files
https://brb.nci.nih.gov/seqtools/downloadfilesforanno.html

# All steps were described in /Users/marzec01/data/PC_ctDNA/WGS_data_analysis_pipeline.txt


#### Run 'GATK_MuTect2.sh' script

########### Generates error when data from WGS and WES data are interrogated ###########

# Sample 45_1_B vs Sample X16018P001A01 (WGS)
nohup ./GATK_MuTect2.sh  45_1_B.merged  45_1_B.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam > 45_1_B.merged.GATK_MuTect2.log &


# Sample 45_2_C vs Sample X16018P001A01 (WGS)
nohup ./GATK_MuTect2.sh  45_2_C.merged  45_2_C.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam > 45_2_C.merged.GATK_MuTect2.log &


# Sample 45_3_D vs Sample X16018P001A01 (WGS)
nohup ./GATK_MuTect2.sh  45_3_D.merged  45_3_D.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam > 45_3_D.merged.GATK_MuTect2.log &


# Sample 45_4_E vs Sample X16018P001A01 (WGS)
nohup ./GATK_MuTect2.sh  45_4_E.merged  45_4_E.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam > 45_4_E.merged.GATK_MuTect2.log &


# Sample 95_1_A vs Sample X16018P001B01 (WGS)
nohup ./GATK_MuTect2.sh  95_1_A.merged  95_1_A.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam > 95_1_A.merged.GATK_MuTect2.log &


# Sample 95_2_B vs Sample X16018P001B01 (WGS)
nohup ./GATK_MuTect2.sh  95_2_B.merged  95_2_B.recalib_merged.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam > 95_2_B.merged.GATK_MuTect2.log &


# Sample 95_3_C vs Sample X16018P001B01 (WGS)
nohup ./GATK_MuTect2.sh  95_3_C.merged  95_3_C.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam > 95_3_C.merged.GATK_MuTect2.log &


# Sample 95_4_D vs Sample X16018P001B01 (WGS)
nohup ./GATK_MuTect2.sh  95_4_D.merged  95_4_D.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam > 95_4_D.merged.GATK_MuTect2.log &




########################################################################################

#### Calling variants with MuTect 2 only within defined list of regions (interval list)


############################ Getting timeout error, need to stick to Agilent_Human_Exon_V6 capture bed file ###########
#
# First, download the bed file with exonic regions from Ensembl biomart (http://www.ensembl.org/biomart/martview)
#
############################ This query is too extensive for BioMart interface ###########
# Don't set any filters
# 1. In attributes go to "structures" and select (1) "Chromosome/scaffold name", (2) "Exon Chr Start (bp)", (3) "Exon Chr End (bp)" and "Gene ID"
# 2. Export  all results to XLS file (select unique results only)
# 3. Make sure to add "chr" to the chromosome numbers
# 4. Add 150bp upstream and 150bp downstream to each coordinate to capture possibly all reads information around the exons (reads length 150bp).
##########################################################################################
#
############################ Getting timeout error #######################################
# Run the BioMart-generated Perl script 'ensembl_Homo_sapiens.GRCh38.exons.pl' (/data/home/hfw456/genome_annotation)
#
cd /data/home/hfw456/genome_annotation
#
perl ensembl_Homo_sapiens.GRCh38.exons.pl  >  ensembl_Homo_sapiens.GRCh38.exons.bed
#
##########################################################################################


#### modify the Agilent_Human_Exon_V6 capture bed file by adding 150bp upstream and 150bp downstream to each coordinate to capture possibly all reads information around the exons (reads length 150bp) (use excel to edit the original bed file)

 
S07604514_Covered_hg38_clean.bed  ->  S07604514_Covered_hg38_clean_extended.bed



#### Run 'GATK_MuTect2_interval.sh' script

# Sample 45_1_B vs Sample X16018P001A01 (WGS)
nohup ./GATK_MuTect2_interval.sh  45_1_B.merged_exome  45_1_B.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam > 45_1_B.merged_exome.GATK_MuTect2.log &


# Sample 45_2_C vs Sample X16018P001A01 (WGS)
nohup ./GATK_MuTect2_interval.sh  45_2_C.merged_exome  45_2_C.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam > 45_2_C.merged_exome.GATK_MuTect2.log &


# Sample 45_3_D vs Sample X16018P001A01 (WGS)
nohup ./GATK_MuTect2_interval.sh  45_3_D.merged_exome  45_3_D.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam > 45_3_D.merged_exome.GATK_MuTect2.log &


# Sample 45_4_E vs Sample X16018P001A01 (WGS)
nohup ./GATK_MuTect2_interval.sh  45_4_E.merged_exome  45_4_E.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam > 45_4_E.merged_exome.GATK_MuTect2.log &


# Sample 95_1_A vs Sample X16018P001B01 (WGS)
nohup ./GATK_MuTect2_interval.sh  95_1_A.merged_exome  95_1_A.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam > 95_1_A.merged_exome.GATK_MuTect2.log &


# Sample 95_2_B vs Sample X16018P001B01 (WGS)
nohup ./GATK_MuTect2_interval.sh  95_2_B_exome  95_2_B.recalib_merged.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam > 95_2_B_exome.GATK_MuTect2.log &


# Sample 95_3_C vs Sample X16018P001B01 (WGS)
nohup ./GATK_MuTect2_interval.sh  95_3_C.merged_exome  95_3_C.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam > 95_3_C.merged_exome.GATK_MuTect2.log &


# Sample 95_4_D vs Sample X16018P001B01 (WGS)
nohup ./GATK_MuTect2_interval.sh  95_4_D.merged_exome  95_4_D.merged.recalib.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam > 95_4_D.merged_exome.GATK_MuTect2.log &




#### Annotate somatic variants with Annovar
http://annovar.openbioinformatics.org/en/latest/
http://annovar.openbioinformatics.org/en/latest/user-guide/download/


#### Download appropriate database files from UCSC Genome Browser Annotation Database using annotate_variation.pl
http://annovar.openbioinformatics.org/en/latest/user-guide/filter

cd /data/home/hfw456/applications/annovar

./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar refGene /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/
./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar knownGene /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/

./annotate_variation.pl -buildver hg38 -downdb cytoBand /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/
./annotate_variation.pl -buildver hg38 -downdb genomicSuperDups /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/ 

./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar esp6500siv2_all /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/
./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar 1000g2015aug /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/
./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar kaviar_20150923 /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/
./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar hrcr1 /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/

./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar exac03 /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/ 
./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar avsnp147 /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/
./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar dbnsfp30a /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/

./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar clinvar_20150629 /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/
./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar cosmic70 /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/
./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar kgXref /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/
./annotate_variation.pl -buildver hg38 -downdb -webfrom annovar nci60 /data/BCI-BioInformatics/jack/genome_annotation/annovar/humandb/



#### Run 'Annovar_annotate_variation.sh' script
#### NOTE: the info from VCF file will be reported only for the first sample (should be TUMOUR)


# Sample 45_1_B vs Sample X16018P001A01 (WGS)
nohup ./Annovar_annotate_variation.sh  45_1_B.merged > 45_1_B.merged.Annovar.log &

# Sample 45_2_C vs Sample X16018P001A01 (WGS)
nohup ./Annovar_annotate_variation.sh  45_2_C.merged > 45_2_C.merged.Annovar.log &

# Sample 45_3_D vs Sample X16018P001A01 (WGS)
nohup ./Annovar_annotate_variation.sh  45_3_D.merged > 45_3_D.merged.Annovar.log &

# Sample 45_4_E vs Sample X16018P001A01 (WGS)
nohup ./Annovar_annotate_variation.sh  45_4_E.merged > 45_4_E.merged.Annovar.log &

# Sample 95_1_A vs Sample X16018P001B01 (WGS)
nohup ./Annovar_annotate_variation.sh  95_1_A.merged > 95_1_A.merged.Annovar.log &

# Sample 95_2_B vs Sample X16018P001B01 (WGS)
nohup ./Annovar_annotate_variation.sh  95_2_B.merged > 95_2_B.merged.Annovar.log &

# Sample 95_3_C vs Sample X16018P001B01 (WGS)
nohup ./Annovar_annotate_variation.sh  95_3_C.merged > 95_3_C.merged.Annovar.log &

# Sample 95_4_D vs Sample X16018P001B01 (WGS)
nohup ./Annovar_annotate_variation.sh  95_4_D.merged > 95_4_D.merged.Annovar.log &



#### Filter the variants to those which PASSed Mutect2 calling

# Sample 45_1_B

egrep PASS 45_1_B.merged.vcf.avinput.exonic_variant_function > 45_1_B.merged.vcf.avinput.exonic_variant_function_PASS


########## SKIP this filtering #########
#### Exclude synonymous and unknown variants (info in second column)
#awk '{ if ( $2 != "synonymous" && $2 != "unknown" ) { print $0; } }' 45_1_B.merged.vcf.avinput.exonic_variant_function_PASS > 45_1_B.merged.vcf.avinput.exonic_variant_function_PASS_nonsyn
########################################


egrep PASS 45_1_B.merged.vcf.avinput.variant_function > 45_1_B.merged.vcf.avinput.variant_function_PASS

#### Include only splicing variants
egrep splicing 45_1_B.merged.vcf.avinput.variant_function_PASS > 45_1_B.merged.vcf.avinput.variant_function_PASS_splice


#### Format Annovar output files

./process_annovar_output_exonic.pl -i 45_1_B.merged.vcf.avinput.exonic_variant_function_PASS

./process_annovar_output_splice.pl -i 45_1_B.merged.vcf.avinput.variant_function_PASS_splice



# Sample 45_2_C

egrep PASS 45_2_C.merged.vcf.avinput.exonic_variant_function > 45_2_C.merged.vcf.avinput.exonic_variant_function_PASS


########## SKIP this filtering #########
#### Exclude synonymous and unknown variants (info in second column)
#awk '{ if ( $2 != "synonymous" && $2 != "unknown" ) { print $0; } }' 45_2_C.merged.vcf.avinput.exonic_variant_function_PASS > 45_2_C.merged.vcf.avinput.exonic_variant_function_PASS_nonsyn
########################################


egrep PASS 45_2_C.merged.vcf.avinput.variant_function > 45_2_C.merged.vcf.avinput.variant_function_PASS

#### Include only splicing variants
egrep splicing 45_2_C.merged.vcf.avinput.variant_function_PASS > 45_2_C.merged.vcf.avinput.variant_function_PASS_splice


#### Format Annovar output files

./process_annovar_output_exonic.pl -i 45_2_C.merged.vcf.avinput.exonic_variant_function_PASS

./process_annovar_output_splice.pl -i 45_2_C.merged.vcf.avinput.variant_function_PASS_splice



# Sample 45_3_D

egrep PASS 45_3_D.merged.vcf.avinput.exonic_variant_function > 45_3_D.merged.vcf.avinput.exonic_variant_function_PASS


########## SKIP this filtering #########
#### Exclude synonymous and unknown variants (info in second column)
#awk '{ if ( $2 != "synonymous" && $2 != "unknown" ) { print $0; } }' 45_3_D.merged.vcf.avinput.exonic_variant_function_PASS > 45_3_D.merged.vcf.avinput.exonic_variant_function_PASS_nonsyn
########################################


egrep PASS 45_3_D.merged.vcf.avinput.variant_function > 45_3_D.merged.vcf.avinput.variant_function_PASS

#### Include only splicing variants
egrep splicing 45_3_D.merged.vcf.avinput.variant_function_PASS > 45_3_D.merged.vcf.avinput.variant_function_PASS_splice


#### Format Annovar output files

./process_annovar_output_exonic.pl -i 45_3_D.merged.vcf.avinput.exonic_variant_function_PASS

./process_annovar_output_splice.pl -i 45_3_D.merged.vcf.avinput.variant_function_PASS_splice



# Sample 45_4_E

egrep PASS 45_4_E.merged.vcf.avinput.exonic_variant_function > 45_4_E.merged.vcf.avinput.exonic_variant_function_PASS


########## SKIP this filtering #########
#### Exclude synonymous and unknown variants (info in second column)
#awk '{ if ( $2 != "synonymous" && $2 != "unknown" ) { print $0; } }' 45_4_E.merged.vcf.avinput.exonic_variant_function_PASS > 45_4_E.merged.vcf.avinput.exonic_variant_function_PASS_nonsyn
########################################


egrep PASS 45_4_E.merged.vcf.avinput.variant_function > 45_4_E.merged.vcf.avinput.variant_function_PASS

#### Include only splicing variants
egrep splicing 45_4_E.merged.vcf.avinput.variant_function_PASS > 45_4_E.merged.vcf.avinput.variant_function_PASS_splice


#### Format Annovar output files

./process_annovar_output_exonic.pl -i 45_4_E.merged.vcf.avinput.exonic_variant_function_PASS

./process_annovar_output_splice.pl -i 45_4_E.merged.vcf.avinput.variant_function_PASS_splice



# Sample 95_1_A

egrep PASS 95_1_A.merged.vcf.avinput.exonic_variant_function > 95_1_A.merged.vcf.avinput.exonic_variant_function_PASS


########## SKIP this filtering #########
#### Exclude synonymous and unknown variants (info in second column)
#awk '{ if ( $2 != "synonymous" && $2 != "unknown" ) { print $0; } }' 95_1_A.merged.vcf.avinput.exonic_variant_function_PASS > 95_1_A.merged.vcf.avinput.exonic_variant_function_PASS_nonsyn
########################################


egrep PASS 95_1_A.merged.vcf.avinput.variant_function > 95_1_A.merged.vcf.avinput.variant_function_PASS

#### Include only splicing variants
egrep splicing 95_1_A.merged.vcf.avinput.variant_function_PASS > 95_1_A.merged.vcf.avinput.variant_function_PASS_splice


#### Format Annovar output files

./process_annovar_output_exonic.pl -i 95_1_A.merged.vcf.avinput.exonic_variant_function_PASS

./process_annovar_output_splice.pl -i 95_1_A.merged.vcf.avinput.variant_function_PASS_splice



# Sample 95_2_B

egrep PASS 95_2_B.merged.vcf.avinput.exonic_variant_function > 95_2_B.merged.vcf.avinput.exonic_variant_function_PASS


########## SKIP this filtering #########
#### Exclude synonymous and unknown variants (info in second column)
#awk '{ if ( $2 != "synonymous" && $2 != "unknown" ) { print $0; } }' 95_2_B.merged.vcf.avinput.exonic_variant_function_PASS > 95_2_B.merged.vcf.avinput.exonic_variant_function_PASS_nonsyn
########################################


egrep PASS 95_2_B.merged.vcf.avinput.variant_function > 95_2_B.merged.vcf.avinput.variant_function_PASS

#### Include only splicing variants
egrep splicing 95_2_B.merged.vcf.avinput.variant_function_PASS > 95_2_B.merged.vcf.avinput.variant_function_PASS_splice


#### Format Annovar output files

./process_annovar_output_exonic.pl -i 95_2_B.merged.vcf.avinput.exonic_variant_function_PASS

./process_annovar_output_splice.pl -i 95_2_B.merged.vcf.avinput.variant_function_PASS_splice



# Sample 95_3_C

egrep PASS 95_3_C.merged.vcf.avinput.exonic_variant_function > 95_3_C.merged.vcf.avinput.exonic_variant_function_PASS


########## SKIP this filtering #########
#### Exclude synonymous and unknown variants (info in second column)
#awk '{ if ( $2 != "synonymous" && $2 != "unknown" ) { print $0; } }' 95_3_C.merged.vcf.avinput.exonic_variant_function_PASS > 95_3_C.merged.vcf.avinput.exonic_variant_function_PASS_nonsyn
########################################


egrep PASS 95_3_C.merged.vcf.avinput.variant_function > 95_3_C.merged.vcf.avinput.variant_function_PASS

#### Include only splicing variants
egrep splicing 95_3_C.merged.vcf.avinput.variant_function_PASS > 95_3_C.merged.vcf.avinput.variant_function_PASS_splice
########################################

#### Format Annovar output files

./process_annovar_output_exonic.pl -i 95_3_C.merged.vcf.avinput.exonic_variant_function_PASS

./process_annovar_output_splice.pl -i 95_3_C.merged.vcf.avinput.variant_function_PASS_splice



# Sample 95_4_D

egrep PASS 95_4_D.merged.vcf.avinput.exonic_variant_function > 95_4_D.merged.vcf.avinput.exonic_variant_function_PASS


########## SKIP this filtering #########
#### Exclude synonymous and unknown variants (info in second column)
#awk '{ if ( $2 != "synonymous" && $2 != "unknown" ) { print $0; } }' 95_4_D.merged.vcf.avinput.exonic_variant_function_PASS > 95_4_D.merged.vcf.avinput.exonic_variant_function_PASS_nonsyn
########################################


egrep PASS 95_4_D.merged.vcf.avinput.variant_function > 95_4_D.merged.vcf.avinput.variant_function_PASS

#### Include only splicing variants
egrep splicing 95_4_D.merged.vcf.avinput.variant_function_PASS > 95_4_D.merged.vcf.avinput.variant_function_PASS_splice


#### Format Annovar output files

./process_annovar_output_exonic.pl -i 95_4_D.merged.vcf.avinput.exonic_variant_function_PASS

./process_annovar_output_splice.pl -i 95_4_D.merged.vcf.avinput.variant_function_PASS_splice





####################################################################################################
#
#### Since we expect little tumour content in the plasma DNA Mutect2 algorithm may be too stringent to call variants in plasma samples. Thus, we try an alternative approach based on reporting any variants, compared to reference genome, across all samples followed by relevant filtering (germline mutations etc.)
#### (see paper by Murtaza M et al, 2013, Non-invasive analysis of acquired resistance to cancer therapy by sequencing of plasma DNA; https://www.ncbi.nlm.nih.gov/pubmed/23563269)
#
########################################################################################
#
#### NOTE: No need to run this part. Run this analysis from WGS directory including all samples (normal tissue + tumour tissue + plasma 1 + plasma 2 + plasma 3 + plasma 4
#
########################################################################################

#### Calling variants based on mpileup approach


#### Run 'Varscan_pileup2cns_3samples.sh' script to call variants (SNPs and indels) using Varscan mpileup2cns for 3 samples


# Sample 45_1_B
nohup ./Varscan_pileup2cns_3samples.sh 45_1_B.merged  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001C01/B01P0045BAA07_tumour.bam  45_1_B.merged.recalib.bam  > 45_1_B.merged.Varscan_pileup2cns_3samples.log &



# Sample 45_2_C
nohup ./Varscan_pileup2cns_3samples.sh 45_2_C.merged  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001C01/B01P0045BAA07_tumour.bam  45_2_C.merged.recalib.bam  > 45_2_C.merged.Varscan_pileup2cns_3samples.log &



# Sample 95_1_A
nohup ./Varscan_pileup2cns_3samples.sh 95_1_A.merged  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001D01/B01P0095AAA03_tumour.bam  95_1_A.merged.recalib.bam  > 95_1_A.merged.Varscan_pileup2cns_3samples.log &




########################################################################################

#### Calling variants based on mpileup approach (as above) for defined regions (interval list)


#### Run 'Varscan_pileup2cns_3samples_interval.sh' script to call variants (SNPs and indels) using Varscan mpileup2cns for 3 samples for defined list of regions (interval list)


# Sample 45_1_B
nohup ./Varscan_pileup2cns_3samples_interval.sh 45_1_B.merged  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001A01/B01P0045_BBC03_normal.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001C01/B01P0045BAA07_tumour.bam  45_1_B.merged.recalib.bam  > 45_1_B.merged.Varscan_pileup2cns_3samples_interval.log &


# Sample 95_1_A
nohup ./Varscan_pileup2cns_3samples_interval.sh 95_1_A.merged  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001B01/B01P0095_ABC03_normal.bam  /data/BCI-BioInformatics/PC_ctDNA/WGS_data/X16018/2016-11-21/X16018P001D01/B01P0095AAA03_tumour.bam  95_1_A.merged.recalib.bam  > 95_1_A.merged.Varscan_pileup2cns_3samples_interval.log &




########################################################################################

#### Annotate somatic variants with Annovar across ALL SAMPLES for defined regions (interval list)

#### Run 'Annovar_annotate_variation_allsample.sh' script


# Sample 45_1_B
nohup ./Annovar_annotate_variation_allsample.sh  45_1_B.merged.cns_exome > 45_1_B.merged.Annovar_allsample.log &


#################### No need to run, run filtering below instead #######################
#### Format Annovar output files
#
#./process_annovar_output_exonic_3samples.pl -i 45_1_B.merged.cns_exome.vcf.avinput.exonic_variant_function
#
./process_annovar_output_splice_3samples.pl -i 45_1_B.merged.cns_exome.vcf.avinput.variant_function
##
######################################################################################## 


#### Filter Annovar output files
# NOTE: for plasma samples set the minimum base Q to 40 as for some reason the qualities in plasma samples are doubled (e.g. 60 instead of 30)

./process_annovar_output_exonic_3samples_filtering.pl -t 4 -p 2 -d 1 -q 40 -i 45_1_B.merged.cns_exome.vcf.avinput.exonic_variant_function



