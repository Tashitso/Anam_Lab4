Anamza_Lab4
=========== 

## Synopsis 
Description of what the project is ....

## Code examples 

```Shell
# the length of the arguments 
len=$*

# write a loop to do these below steps
for i in $len
do
    echo $i
    echo "~~~~~~~~~:"
  
# redirect the header into a file 
    #awk 'NR == 1' $i > ${i%%.*}_header.txt

# count and print after removing the headers
    echo "Counts after removing the headers: " 
    wc -l $i

# sort and remove duplicates based on latitude 
    sort -k 17 $i | uniq > ${i%%.*}_lat_uniq.txt

# use the sorted files to sort it based on longitude and remove duplicates 
    sort -k 18 ${i%%.*}_lat_uniq.txt | uniq > ${i%%.*}_lat_long_uniq.txt

# count number of recoerds after removing the deplicates
    echo "Counts after removing duplicates" 
    wc -l ${i%%.*}_lat_long_uniq.txt

# get the records from USNM and redirect them into a file
    echo "Counts are only from USNM"
    grep 'USNM' ${i%%.*}_lat_long_uniq.txt | wc -l 
    grep 'USNM' ${i%%.*}_lat_long_uniq.txt > ${i%%.*}_USNM.txt 

# produce a file that only has the lat and long coordinates
    awk 'FS="\t" { print $17, $18}' ${i%%.*}_USNM.txt > ${i%%.*}_USNM_lat_long.txt

# remove records with misssing values
    grep -v "^\s*$" ${i%%.*}_USNM_lat_long.txt | grep -v "[a-zA-Z]" > ${i%%.*}_lat_long_cleaned.txt

# count the records after the values are removed
    wc -l ${i%%.*}_lat_long_cleaned.txt
    echo " " 

# combine the two cleaned files cat function with append operator >> 
    cat ${i%%.*}_lat_long_cleaned.txt >> Lat_Long_USNM_combined.txt 
    
     
done
```	
	
Count the number of lines in a file.

      wc -l filename

Find a certian pattern in the file and count the number of its occurence.

     grep 'pattern' filename | wc -l


### Create a table to report the results

|filename|line counts|pattern counts| 
|:-------|:-------:|---------:|
|file1.csv|200|20|
|file2.csv|450|142|


## Motivation
