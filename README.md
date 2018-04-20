> 本文参考华中科技大学，陈明龙 "基于CUDA蛋白质结构比对算法的优化研究"  仅供研究使用  
> This research is referred to Minglong Chen, 'Comparision Algorithm Optimization Research of Protein's structure based on CUDA'. For research use only.
# highPerformanceCalculation_CUDA_ProteinStructureComparison
## 2 points for this research
* Algorithms for protein structure comparison
* Optimizations for original algorithm
## Goal of ProteinStructureComparison
Build 1 or more similiar 3D structure of 1 Protein based on its shape and 3-level structure
## Evaluation of comparison results
Test structure vs Standard structure. Through structure comparison algorithm, we obtain a set of atom coordinates & RMSD of the 2 structures. A translation vector **t** and rotation vector **u**. And by inverse translation and rotation, test protein will be compared to standard protein.
## CUDA's advantage and Programming Model
### Compared with CUBLAS, C PROGRAM, INTEL MKL, FFTW, CUFFT, CUDA's efficiency is 25 times more than CPU.
* The Key optimization of CPU focuses on serial operation, but that of GPU focuses on parallel-operation.
* CPU focus on improving frequency, parallel-level and numbers of the cores.
* Typical application of 'CPU+GPU' Heterogeneous parallel processing is real-time image rendering. 
* GPU is mostly used in high computation desity and simple logic condition.
### GPU's programming model has only 1 host(CPU), GPU is working as the device of the Host.
* memories fo GPU and CPU are independent, namely host's memory and device's memory.
* Program running in GPU is called 'kernel function' and must be defined using "\_global_". And kernel functions only will be called by Host's program. 
## Application Bioinformatics status
CUDA is already used for BLAST sequence comparison. To fulfill bio-sequence comparison's accuracy.
Smith-Waterman algorithm is proposed to use CUDA's compacity by diamond-shape data layout. And tree-shape algorithm is introduced to calculate maximum match value. The performance is improved by 120 times.
But TM-Align still need to be implemented on CUDA.
## Protein structure comparison
Annotation of similar amino acid residue of protein sequences. If there is a gap, use '-' to indicate. The key of the problem is those long, variant and human-labor incapable sequences. So human's knowledge is used to construct algorithms not to duplicate and boring work. Protein sequence comparison is an important part of Strutural Informatics, aiming at prediction of protein's sequence and functional correlation analysis. For example, we can classify proteins with similar strutures and investigate homology of proteins. And also helping to understand the structural differences between proteins and how these differences affect the functions of proteins. 
2 types of algorithms to compare protein sequences
* Clustering based method. Comparing the distances of amino acid residues within the sequences.
* Superposition based method. To Minimize the distance of relative amino acid residues after rotation and translation.
TM-align used amino acid residue based method and 2-level comparision method simultaneously.

## TM-align
There is a demo here https://zhanglab.ccmb.med.umich.edu/TM-align/

* Result of the demo
Name of Chain_1: A404296                                           
Name of Chain_2: B404296                                           
Length of Chain_1:  154 residues
Length of Chain_2:  146 residues

Aligned length=  143, RMSD=   1.83, Seq_ID=n_identical/n_aligned= 0.245
TM-score= 0.81453 (if normalized by length of Chain_1)
TM-score= 0.85377 (if normalized by length of Chain_2)
(You should use TM-score normalized by length of the reference protein)

(":" denotes aligned residue pairs of d < 5.0 A, "." denotes other aligned residues)
MVLSEGEWQLVLHVWAKVEADVAGHGQDILIRLFKSHPETLEKFDRVKHLKTEAEMKASEDLKKHGVTVLTALGAILKKK--G-HHEAELKPLAQSHATKHKIPIKYLEFISEAIIHVLHSRHPGNFGADAQGAMNKALELFRKDIAAKYKELGYQG
 :::::::::::::::::::::::::::::::::::::::::::::::::: ::::::::::::::::::::::::::::  : :::::::::::::: :::::::::::::::::::::::::  :::::::::::::::::::::: :     ::
-SLSAAEADLAGKSWAPVFANKNANGLDFLVALFEKFPDSANFFADFKGKS-VADIKASPKLRDVSSRIFTRLNEFVNNAANAGKMSAMLSQFAKEHV-GFGVGSAQFENVRSMFPGFVASVAA--PPAGADAAWTKLFGLIIDALKA-A-----GA
![](result.png?raw=true)


* TM-Score produce the homology（同源性） and correlation（相关性） of Proteins. The result is between (0, 1], <0.17 means no match from PDB.
* TM-Align adopts dynamic programming and rotation matrix of TM-score. Its result is between (0,1], <0.2 means no similarity between the structures, >0.5 means the structures share some same foldings.
* Differences between TM-align and TM-score  
TM-score is used to compare 2 models with the same amino acid residues and sequences.
TM-align is devoted to commpare 2 structures with different sequeces or unknown equivalence and output the best-match residues's TM-score. Here TM-score has the same definition of previous TM-score.
* 2 amino acid dehydration synthesis, -COOH（carboxyl）   H2N(azyl)
H2N-CHR1-COOH  +   H2N-CHR2-COOH     ----->   H2N-CHR1-COOH  +   H2N-CHR2-COOH
TM-align only take alpha(first)-carbon chain into account. At the beginning, TM-align will find a better rotation matrix **U** and shift vector **t**. Using these 2 parameters to registrate the test structure to a better matching position. Then align the test structure and source structure based on distances of amino acid residues.
## Formula of TM-score
![](tmscore-formula.png?raw=true)
