# BCB546X_Unix_Assignment

## File Inspection

 * I first looked at the top and bottom of the `fang_et_al_genotypes` file to see what types of data I was dealing with. This produced a super gross looking output to I cut the first few columns and used column to line everything up

   `cut -f 1-12 fang_et_al_genotypes.txt | head | column -t`

#### *Maize Group*
 * Before transforming the data, I wanted to pull out the maize groups (ZMMIL, ZMMLR, and ZMMMR) using `grep` and put that output in a new file called `maize_genotypes.txt`

 * To make sure the expression within grep worked correctly, I used `grep` with each one separately and then used `wc -l` to make sure that to line numbers of each one separately added up to the expression

   ```
   grep "ZMMIL" fang_et_al_genotypes.txt | wc -l
   290
   grep "ZMMLR" fang_et_al_genotypes.txt | wc -l
   125
   grep "ZMMMR" fang_et_al_genotypes.txt | wc -l
   27
   grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt | wc -l
   1573
   
   grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt
   ```

 * After that I used the awk transformation code provided in the assignment sheet to transform the data and put the output in a new file called transposed_maize_genotypes.txt

   `awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt`

 * I then looked at the output of the file using a combination of cut, head and column

   `cut -f 1-5 transposed_maize_genotypes.txt | head | column -t`

 * So now it looks like each column is an individual genotype. To figure out the number of SNPs, I used `wc -l` and subtracted the number of 'metadata' lines (3)

   ```
   wc -l transposed_maize_genotypes.txt
   986 
   #so there are 983 SNPs
   ```

 * 
#### *Teosinte Group*
 * I followed the same general trend to extract the teosinte individuals

- Before transforming the data, I wanted to pulled out the teosinte groups (ZMPBA, ZMPIL, and ZMPJA) using `grep` and put that output in a new file called `teosinte_genotypes.txt`

- To make sure the expression within grep worked correctly, I used `grep` with each one separately and then used `wc -l` to make sure that to line numbers of each one separately added up to the expression

  ```
  grep "ZMPBA" fang_et_al_genotypes.txt | wc -l
  900
  grep "ZMPIL" fang_et_al_genotypes.txt | wc -l
  41
  grep "ZMPJA" fang_et_al_genotypes.txt | wc -l
  34
  grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt | wc -l
  975
  
  grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > teosinte_genotypes.txt
  ```

- 

1.

2.

ls and du

## Data Processing





