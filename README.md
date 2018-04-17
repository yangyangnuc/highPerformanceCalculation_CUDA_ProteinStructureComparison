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
## CUDA's advantage
Compared with CUBLAS, C PROGRAM, INTEL MKL, FFTW, CUFFT, CUDA's efficiency is 25 times more than CPU.
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

