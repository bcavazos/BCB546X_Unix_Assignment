# BCB546X_Unix_Assignment

## File Inspection

 * Looking at the top of the file using head

   `head snp.file`

```
head snp.file
tail snp.file
```

1.

2.



## Data Processing

for the fang file

first I cut just the first three columns because I found it easier to play around with to make sure i was doing the right thing

then I used grep to pick out the positions ZZMIL ZMMLR and SMMMR

to make sure the regular expression "(x|y|z)" worked, I used wc -l on each of them separately and then all together to make sure things added up

$ cut -f 1,2,3 fang_et_al_genotypes.txt | grep "ZMMIL" | wc -l
290

$ cut -f 1,2,3 fang_et_al_genotypes.txt | grep "ZMMLR" | wc -l
1256
$ cut -f 1,2,3 fang_et_al_genotypes.txt | grep "ZMMMR" | wc -l
27

$ cut -f 1,2,3 fang_et_al_genotypes.txt | grep -E "(ZMMIL|ZMMLR|ZMMMR)" | wc -l
1573

now I can grep these into a different file and then transpose them 

grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt

