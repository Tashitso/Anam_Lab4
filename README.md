Anamza_Lab4
=========== 

## Synopsis 
Description of what the project is ....

## Code examples 

```Shell
#! /bin/sh
for species in 'CAST' 'DOM'
do
    wc -l $species
    mkdir -p Anamza/DOM Anamza/CAST

    if [ $species == "DOM" ]; then
        file=`grep -il "domesticus" *.csv`
        echo "The file for $species is $file"
    elif [ $species == "CAST" ]; then
        file=`grep -il "castaneus" *.csv`
        echo "The file for $species is $file"
    fi
    cd Anamza/CAST/
    pwd
    
    awk 'NR == 1' $species > ${species}_header.txt
    ls
    cat ${species}_header.txt
    sort -nk 17,17 $species | uniq > ${species}_lat_uniq.txt
    sort -nk 18 ${species}_lat_uniq.txt | uniq > ${species}_lat_long_uniq.txt
    wc -l ${species}_lat_long_uniq.txt
    grep 'USNM' ${species}_lat_long_uniq.txt | wc -l
    grep 'USNM' ${species}_lat_long_uniq.txt > ${species}_USNM.txt
    cp ${species}_USNM.txt ..
    cd ../..
    cat ${species}_USNM.txt >> Lat_long_USNM_combined.txt

done
```

### Report the results

|filename|Duplicate|Percentage of duplicates removed| 
|:-------|:-------:|---------:|
|CAST|0|0%|
|DOM|91|1%|


## Motivation
