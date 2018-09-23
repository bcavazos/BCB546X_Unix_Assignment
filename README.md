# BCB546X_Unix_Assignment

## File Inspection

 * I first looked at the top and bottom of the `fang_et_al_genotypes` file to see what types of data I was dealing with. This produced a super gross looking output to I cut the first few columns and used column to line everything up

   `cut -f 1-12 fang_et_al_genotypes.txt | head | column -t`
* I also checked file sizes for both the fang and snp document using `du -h`
```
du -h fang_et_al_genotypes.txt
11M     fang_et_al_genotypes.txt
# ^ pretty big file size
du -h snp_position.txt
84K     snp_position.txt
```

## Preparing Files to join
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
* After that I used the awk transformation code provided in the assignment sheet to transform the data and put the output in a new file called transposed_teosinte_genotypes.txt

   `awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt`

 * I then looked at the output of the file using a combination of cut, head and column

   `cut -f 1-5 transposed_maize_genotypes.txt | head | column -t`

 * So now it looks like each column is an individual genotype. To figure out the number of SNPs, I used `wc -l` and subtracted the number of 'metadata' lines (3)

   ```
   wc -l transposed_teosinte_genotypes.txt
   986 
   #so there are 983 SNPs
   ```

--whoops----------------------------------------------------------------------------

_So_, when I used `grep` in the beginning to pull out maize and teosinte, it got rid of the column names for the fang et al doc. While I am pretty sure that they are in the same order as the snp position, I am going to transpose the fang doc, keep the first column, and cat to the maize and teosinte doc so I'll have a common column to join
```
awk -f transpose.awk fang_et_al_genotypes.txt > transposed_fang_genotypes.txt`
cut -f 1 transposed_fang_genotypes.txt > comcolumn.txt
paste commoncolumn.txt transposed_maize_genotypes.txt | cut -f 1-5 | head | column -t
paste commoncolumn.txt transposed_maize_genotypes.txt > maize_genotypes_forjoin.txt
paste commoncolumn.txt transposed_teosinte_genotypes.txt > teosinte_genotypes_forjoin.txt
```
This makes _way_ more sense now. 


#### *SNP Position*

we only care about SNP_ID, Chromosome number, and position so we can cut out all extra info and have a new doc


## Data Processing
Make sure the dimensions are the same so take out the headers in everything using `awk` and rename file with underscoreNH at the end (no header) 
for example
` awk 'NR>3' teosinte_genotypes_forjoin.txt > teosinte_genotypes_forjoin_NH.txt`
join documents by snp id 

```
join -1 1 -2 1 -t $'\t' snp_position_forjoin_NH.txt maize_genotypes_for_join_NH.txt > joined_maize.txt`
wc joined_maize.txt
cut -f 1-5 joined_maize.txt | head | column -t

join -1 1 -2 1 -t $'\t' snp_position_forjoin_NH.txt
teosinte_genotypes_forjoin_NH.txt > joined_teosinte.txt`
wc joined_teosinte.txt
cut -f 1-5 joined_teosinte.txt | head | column -t

```
now I can reorder everything into the different files

* I used sort to put both the joined_maize.txt and joined_teosinte.txt files in order by first chromosome number, then position
```
sort -k2n,2 -k3,3n joined_teosinte.txt > joined_teosinte_sorted.txt
sort -k2n,2 -k3,3n joined_maize.txt > joined_maize_sorted.txt
```

* Then I used awk to make each unique chromosome into a separate file 
`for i in {1..10}; do awk '$2=='$i'' joined_maize_sorted.txt > chr"$i"_maize_genotypes.txt; done`
* make sure all the sorting worked out okay using `cat` and `head` and `tail`
* did the same loop for teosinte

* to reverse the order of positions, I added an r to in the third column in the sort command 
 `sort -k2n,2 -k3,3nr joined_maize.txt > joined_maize_reversesorted.txt`
 I did this for both maize and teosinte files and followed the same steps as above to make separate files for each chromosome
 

* use `sed` to replace ?/? with -/-
sed 's/\?/\-/g' name > name





