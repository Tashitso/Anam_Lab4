Anamza_Lab4
=========== 

## Synopsis 
This Shell script cleans data from GBIF. It sorts the data based on latitude first and then sort it based on longitude. It removes the duplicate and missing values, it also removes spaces and blank lines. Then it extracts the records that are from a specific museum. The final file only contains the coordinates. This script allows to loop through a given number of sepcies to perform thoese steps. 

## Code examples 

```Shell
#! /bin/sh

for species in 'DOM' #'CAST'
do
   echo "$species"
   wc -l $species
   # make directory for each species 
    mkdir -p Anamza/DOM Anamza/CAST
    
    # check each species correspods to its file 
    if [ $species == "DOM" ]; then
      file=`grep -il "domesticus" *.csv`
        echo "The file for $species is $file"
    elif [ $species == "CAST" ]; then
       file=`grep -il "castaneus" *.csv`
       echo "The file for $species is $file"
    fi
    cd Anamza/DOM/
    pwd

   # get the header and save it  
   awk 'NR == 1' $species > ${species}_header.txt
   
   # sort based on latitude and then longgitude
   sort -nk 17,17 $species | uniq > ${species}_lat_uniq.txt
   sort -nk 18,18 ${species}_lat_uniq.txt | uniq > ${species}_lat_long_uniq.txt
   wc -l ${species}_lat_long_uniq.txt

   # extract the records that are only from USNM 
    grep 'USNM' ${species}_lat_long_uniq.txt | wc -l
    grep 'USNM' ${species}_lat_long_uniq.txt > ${species}_USNM.txt

    # Extract only the coordinates 
    awk 'FS="\t" { print $17, $18}' ${species}_USNM.txt > ${species}_USNM_lat_long.txt

   # remove the missing values
    grep -v "^\s*$" ${species}_USNM_lat_long.txt > ${species}_lat_long_cleaned.txt
    wc -l ${species}_lat_long_cleaned.txt

    # copy the the final file to parent directory 
    cp ${species}_lat_long_cleaned.txt ..
   # change directory to parent directory 
    cd ..

    # combined the final 2 files into one 
    cat *.txt >> Lat_long_USNM_combined.txt
    
    pwd
 
done

```

### Report the results

|filename|Duplicates|Percentage of duplicates removed| 
|:-------|:-------:|---------:|
|CAST|0|0%|
|DOM|91|1%|


## Motivation
