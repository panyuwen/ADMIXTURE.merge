**ADMIXTURE merge** (by Yuwen Pan)    

To merge ancestral components (admixture proportions across individuals) and adjust their orders for the replicates of ADMIXTURE analysis    

If you use the script **ancestry_file.merge.py** in your research work, please cite at least one of the following paper(s):    

[Genomic diversity and post-admixture adaptation in the Uyghurs](https://doi.org/10.1093/nsr/nwab124) (*National Science Review*, 2021)


### Description: 

This program works in the following way: 

1. Find all the ancestry files of the same number of ancestry clusters (K) under the given directory. And also accept one of the ancestry file as the reference, while others are regarded as targets. The ancestry file is generated by the program [AncestryPainter.pl](https://github.com/Shuhua-Group/AncestryPainter). The rationale behind is that, the results of ADMIXTURE have their biological meaning, so you should choose one replicate that make sense to you as the reference after multiple times of ADMIXTURE run;
2. For each target file, calculate the pairwise pearson correlation coefficients between components in the target and those in the reference. Given an ancestral component in the target file, the program will take one from the reference file as its best match if their correlation coefficient is at least 0.5 higher than others (other components in the reference file). Further, the two replicates (target and reference) would be considered as consensus if every ancestral component had its own best match; 
3. Consistent replicates will be merged by averaging all the ancestral components / admixture proportions to mean in the individual level;
4. The supporting rate for each component will be calculated as the proportion of replicates that matching the reference;
5. The ancestral components in the output will be reordered according to their mean proportions in the population level, which would promise the same relative orders of ancestral components even under different K;
6. If different sample sets are used across all the replicates, only those shared between target and reference would be used for the estimation of correlation coefficient. So you should make sure that enough samples are shared among the different replicates. 

### Input:

1. directory of  the ancestry files, which were generated by [AncestryPainter.pl](https://github.com/Shuhua-Group/AncestryPainter) or in the same format`required` 
2. path to the reference file, and filename should be included `required` 
3. K (number of ancestral group) `required` 
4. population oder list (same as that for AncestryPainter.pl) `optional`
5. output prefix `optional`

### Output:

1. [prefix].merge.ancestry
2. [prefix].logfile
3. [prefix].consensus.filelist
4. [prefix].conflic.filelist
5. [prefix].supporting_ratio.txt

### Usage:

``` bash
python2 ancestry_file.merge.py --h
```

``` bash
python2 ancestry_file.merge.py \
          --path ./ \
          --ref ./rep1/file1.3.ancestry \
          --k 3 \
          --order pop.order \
          --out prefix
```

**Details about the arguments** 

**--path** (required): directory of  the ancestry files that generated by [AncestryPainter.pl](https://github.com/Shuhua-Group/AncestryPainter) or in the same format. Only 2 additional columns to the Q file, `individual ID` `population ID` `columns of ancestral components` , tab/space separated. **Note:** ancestry files should be named in the following format, [ancestryfilenameprefix].{K}.ancestry, {K} should be a number and the same as that indicated by the "--k" option.    

**--ref** (required): path to the reference ancestry file, and filename should be included   

**--k** (required): number of ancestral group    

**--order** (optional): population oder list, one population name for each line (same as that for AncestryPainter.pl)    

**--out** (optional): output prefix, **default:** ./out    

### Tips & Notes:  

The program is developed in python2.7, use other versions of python at your own risk.    

All the packages required are accessible in conda.   

---

By: Yuwen Pan, 2021  
Contact: panyuwen.x@gmail.com
