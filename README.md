# RESTORE_norm
Perform RESTORE normalization on multiplexed imaging data.
This project is for implementing the RESTORE normalization and background subtraction method proposed by Burlingame, EA (1) and Chang, YH (2).  

Intended Usage on a HPC compute node:
```
~$ for i in {0..<number of markers - 1>}; do 
      python calculate_threshold.py \
      --CSV <"intensity csv file"> \
      --mfname <"mutually exclusive marker csv file"> \
      --pos_idx $i \
      --save_dir <"path to save results"> \
      --floor <minimal intensity> &
   done
   wait
```
and then
```
~$ python normalize_v.py \
      --CSV <"the same intensity csv file as for calculate_threshold.py"> \
      --mfname <"the same mutually exclusive marker csv file as for calculate_threshold.py"> \
      --save_dir <"the same results path as for calculate_threshold.py">
```
The intensity files must in a .csv file with marker names in the header and the rest of signal intensities for each cell. It must have a column called 'scene', which contain the identifiers of each image/scene. It can be the same identifier if you gave only one scene in a file.  
The marker file must in a .csv file. The header contains the markers to be normalized. They must match exactly with the markers in the data file header. The rest of the rows are the mutaully exclusive markers for the header marker. One can also normalize a sutset of markers.  
The floor argument wil be different drastically depedent upon the platforms. For cyclic immunofluorescence platforms, the orignal papers from Burlingame, EA (1) and Chang, YH (2) used 50 and 100. For Hyperion platform one can try 0.001. Users should test and find the optimal value for their own dataset  

The normalized data are stored in csv files in the output folder. Marker plots are in the img folder. After normalization, a gate/filter should be applied to remove values below 1 for each marker in each cell.

REFERENCES:  
 
1. Toward reproducible, scalable, and robust data analysis across multiplex tissue imaging platforms.  
   Burlingame EA, Eng J, Thibault G, Chin K, Gray JW, Chang YH.  
   Cell Rep Methods. 2021 Aug 23;1(4):100053. doi: 10.1016/j.crmeth.2021.100053. Epub 2021 Jul 23.
   PMID: 34485971  
2. RESTORE: Robust intEnSiTy nORmalization mEthod for multiplexed imaging.  
   Chang YH, Chin K, Thibault G, Eng J, Burlingame E, Gray JW.  
   Commun Biol. 2020 Mar 9;3(1):111. doi: 10.1038/s42003-020-0828-1.
   PMID: 32152447
