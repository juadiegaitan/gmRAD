# gmRAD: an integrated  pipeline to call SNP genotypes across a hybrid population for genetic mapping  with RADseq data
# Introduction
gmRAD is used to call SNP genotypes across a hybrid population with RAD-seq data for genetic linkage mapping. It takes five steps as following to finish the whole work.  
        1. Clustering the first reads of each parent;  
        2. Building the reference sequence within a cluster for each parent;  
        3. Generating the parental SNP catalogs;  
        4. Calling SNP genotypes across the individuals in the whole population;  
        5. Filtering the SNP data by considering segregation ratio and missing data.  
# Usage
To run gmRAD, users should install the three prerequisite packages: [LOCAS](http://ab.inf.uni-tuebingen.de/software/locas/), [BWA](http://bio-bwa.sourceforge.net/) and [SAMtools (with BCFtools)](http://samtools.sourceforge.net/), and prepare a parameter setting file, namely `parameters.ini`. The parameter file contains three parts, i.e., folders, parameters and fastq files. As the first part, 'folders' gives the paths to gmRAD itself, LOCAS, SAMtools, BCFtools and fastq files. The second part 'parameters' includes the number of threads used for parallel computing, the edit distance allowed in clustering and mapping reads, the estimated percent of genome repeat regions, the minimum score of an SNP genotype, the percent of non-missing genotypes required at an SNP and the minimum p-value allowed for testing the segregation ratio of an SNP. The third part 'fastq files' presents the first read files and the second read files for all individuals including the two parents.  A typical parameter file looks as following:  

        [folders]  
        GMRAD_FOLD:/home/tong/gmRAD  
        LOCAS_FOLD:/home/tong/locas_binaries  
        BWA_FOLD:/home/tong/bwa-0.7.5a  
        SAMTOOLS_FOLD:/home/tong/samtools-1.4.1  
        BCFTOOLS_FOLD:/home/tong/bcftools-1.4.1  
        RADDATA_FOLD:/home/tong/exampledata  
          
        [parameters]  
        THREADS: 4  
        EDITDST: 5  
        GENOMEREPEAT: 30  
        GQ: 30  
        MISPCT: 90  
        PVALUE: 0.01  

        [fastq files]  
        FEMALEPARENT: female_1.fq   female_2.fq  
        MALEPARENT: male_1.fq   male_2.fq  
        PROGENY1:  sample01_1.fq  sample01_2.fq  
        PROGENY2:  sample02_1.fq  sample02_2.fq  
        PROGENY3:  sample03_1.fq  sample03_2.fq  
        .......................................  
        PROGENY19:  sample19_1.fq  sample19_2.fq  
        PROGENY20:  sample20_1.fq  sample20_2.fq  
  
  When the required software packages are installed and the parameter file is parepared and saved in a work directory, you can go to the work directory and get started with the command:  
  `perl PathToGmRAD/gmRAD.pl`
    
  You can run with the 'help' option (`perl PathToGmRAD/gmRAD.pl -h`) to show the usage of gmRAD:

        Usage: perl gmRAD.pl [Options]
        
        Options:
                --nocluster  skip the step for clustering the first reads of each parent  
                --nobuild    skip the step for building the parental reference sequences  
                --nocatalog  skip the step for generating parental SNP catalogs  
                --nocall     skip the step for calling SNP genotypes for all progeny
                --nofilter   skip the step for filtering the SNP genotype data  
                --help|h     help  
It can be seen that users can perform the analytical steps independently by adding some options described as above if some prerequisite files are avaiable.  
# Test Data
       
We provide a test data for users to quickly grasp the use of gmRAD. All reads data files can be downloaded in a compressed file as [exampledata.tar.gz](http://www.bioseqdata.com/gmRAD/exampledata.tar.gz), including 2 parents and their 20 progeny samples. With the parameter file [parameters.ini](https://github.com/tongchf/gmRAD/blob/master/parameters.ini) attached at this site, we can perform the analysis process by inputting the command: `perl gmRAD.pl`. When the computing finishes, we can obtain 6 segregation types of SNP genotype files, including aaxab_pct90pv01.txt, aaxbc_pct90pv01.txt, abxaa_pct90pv01.txt, abxab_pct90pv01.txt, abxac_pct90pv01.txt and abxcc_pct90pv01.txt, which contain 130, 34, 136, 28, 14 and 39 SNPs, respectively. In this test example, we cannot find the genotype file "abxcd_pct90pv01.txt", because no SNPs are generated in the segregation type of abxcd.
