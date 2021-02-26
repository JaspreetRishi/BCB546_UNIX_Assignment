## Course Assignments

# Data inspection

head -n5 snp_position.txt tail -n5 snp_position.txt head -n 5 fang_et_al_genotypes.txt tail -n 5 fang_et_al_genotypes.txt wc fang_et_al_genotypes.txt wc snp_position.txt

# Sorting Before joining

head -n 1 snp_position.txt > sorted_snp1.txt tail -n+2 snp_position.txt | sort -k1,1 >>sorted_snp1.txt

# Rearranging the columns in the file according to final output

awk 'BEGIN {FS="\t"; OFS="\t"} {print $1, $3, $4, $2, $5,$6,$7,$8,$9,$10,$11,$12,$13,$14,$15}' sorted_snp1.txt > sorted_column_snp1.txt

# Zea Mays

head -n 1 fang_et_al_genotypes.txt > geno_maize.txt grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt >>geno_maize.txt

Transposing the filtered data

awk -f transpose.awk geno_maize.txt > transposed_genotype_maize.txt

# Sorting before joining

head -n 1 transposed_genotype_maize.txt > sorted_maize.txt tail -n+4 transposed_genotype_maize.txt | sort -k1,1 >>sorted_maize.txt

# Joining the two files

join -1 1 -2 1 --header sorted_column_snp1.txt sorted_maize.txt > joined_file.txt sed -i "s/ /\t/g" joined_file.txt 

# Creating 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by'?'
```mkdir maize```
```head -n 1 joined_file.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 >"maize/ordered_chromosomes_"i"_asc.txt"}'```
```tail -n +2 joined_file.txt | sort -k2,2 -k3,3n | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {  print  >>"maize/ordered_"$2"_asc.txt"}}'```

# Creating 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by'?'
```head -n 1 joined_file.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 >"maize/ordered_chromosomes_"i"_des.txt"}'```
```tail -n +2 joined_file.txt | sort -k2,2 -k3,3rn | sed 's/?/-/g' | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {  print  >>"maize/ordered_chromosomes"$2"_des.txt"}}'``

# Creating file for unknown position
```head -n 1 joined_file.txt >maize/unknown_Chromosomes.txt```
```tail -n +2 joined_file.txt |  awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "unknown") {  print  >>"maize/unknown_Chromosomes.txt"}}'```

# Creating file for multiple positions
```head -n 1 joined_file.txt >maize/multiple_Chromosomes.txt```
```tail -n +2 joined_file.txt |  awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "multiple") {  print  >>"maize/multiple_chromosomes.txt"}}'```

# Teosinte datasets
# Filtering teosinte data from genotype dataset
```head -n 1 fang_et_al_genotypes.txt > genotype_teosinte.txt```
```grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt >>genotype_teosinte.txt```

# Transposing the filtered data 
```awk -f transpose.awk genotype_teosinte.txt > transposed_genotype_teosinte.txt```

# Sorting the data before join
```head -n 1 transposed_genotype_teosinte.txt > teosinte_sorted.txt```
```tail -n+4 transposed_genotype_teosinte.txt | sort  -k1,1 >>teosinte_sorted.txt```

# Joining the two files
```join -1 1 -2 1 --header  sorted_column_snp1.txt teosinte_sorted.txt > teosinte_joined.txt```
```sed -i "s/ /\t/g" teosinte_joined.txt```

# Creating 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with the missing data encoded by this symbol: ?
```mkdir teosinte```
```head -n 1 teosinte_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 >"teosinte/Chromosome_"i"_asc.txt"}'```

# Creating 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with the missing data encoded by this symbol: -
```head -n 1 teosinte_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 >"teosinte/Chromosome_"i"_des.txt"}'```
```tail -n +2 teosinte_joined.txt | sort -k2,2 -k3,3rn | sed 's/?/-/g' | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {  print  >>"Chromosome_"$2"_des.txt"}}'```

# Creating file for unknown position
``` head -n 1 teosinte_joined.txt > teosinte/unknown_chromosome.txt```
```tail -n +2 teosinte_joined.txt |  awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "unknown") {  print  >>"teosinte/unknown_chromosome.txt"}}'```

# Creating a file for multiple positions
```head -n 1 teosinte_joined.txt >teosinte/multiple_chromosome.txt```
```tail -n +2 teosinte_joined.txt |  awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "multiple") {  print  >>"teosinte/multiple_chromosome.txt"}}'```

that's all

