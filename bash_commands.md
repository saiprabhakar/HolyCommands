
Renaming with confirmation: \
`for i in $(grep -rl --include \*.py "ml_basicprocessing"); do vim -c "%s/ml_basicprocessing/basicprocessing/gc" -c "wq" "$i"; done`

Concat all text from stm files \
`find stm_small/ -name *.txt | xargs cat | awk '/ / {a=""; for(i=7; i<=NF; i++) a=a$i" "; print a}' | tr '\n' ' ' | fold -w1 | sort -u`

grab all near by text near `:` character \
`find presido_stm_dirs/stm_raw/ -name *.txt | xargs cat | grep -o -P ".{0,13}\:.{0,14}"`

grab text with or within `[` \
`grep -oP -r '\[\K[^\]]+' stm_raw/ | cut -d":" -f2 | sort -u > ../non_vocab_all.txt`
`grep -oP -r '\[(.*?)\]' stm_raw/ | cut -d":" -f2 | sort -u > ../non_vocab_complete.txt`

grab text from textgrid and make new file \
```for f in `find ~/transcripts_textgrid/ -name "*.txt"`; do grep -oP 'text = \".*?\"' $f | cut -f2- -d= > ~/temp/`basename $f .TextGrid.txt`.txt; done```

grab text withing `<>` and find uique \
`find temp/ -name "*.txt" | xargs cat | grep -oP '\<(.*?)\>'| sort -u`

grab text with `<NIS>` but do not have `<NIS/>` and `</NIS>` \
`grep -rn -P "\<NIS\>" temp/ | grep -P -v "\<NIS\/\>" | grep -P -v "\<\/NIS\>"`

replace `<sdf sdf>` with `<sdfsdf>` \
`find . -name "*.txt" | xargs sed -i -e 's/<INAUDIBLE\ SPEECH>/<INAUDIBLESPEECH>/g' `

for all the file (*.stm) remove lines begining with `;` then modify the 3rd field (`ss_dd_ee` -> `ee`) \
`for f in *.stm; do  grep -v "^;" $f | awk '{n=split($3, a, "_"); $3=a[n]; print}' > $f"_"; done`

Rename all files `*.stm_` -> `*.stm` \
`rename 's/.stm_/.stm/' *.stm_` \
```for f in AA*_upmc*.TextGrid; do a=`cut -d"_" -f3-6 <<< $f`; mv $f $a.TextGrid; done```

Script for renaming files in one dir with files in another (sorted by ls) \
```
src=$1
namesrc=$2
dest=$3
for f in `ls $namesrc/*.docx`; do
        f11=`ls $src/*.docx`;
        f1=`echo $f11 | cut -d" " -f1`
        newname=`basename $f`;
        oldname=`basename $f1`;
        mv $f1 $dest/$newname;
done
```

Get sorted frequency of a column in a file \
`cut -d":" -f3 < file1.txt | sort | uniq -cn | sort -rn > sorted_freq.txt`

Compare two sorted filelists (use `sort` without flags) and get only items that are different \
`comm f1.txt f2.txt -3`


fast grepping 
```
time find . -name "*.stm" | parallel -k -j1000% -n 1000 -m grep -H -n '\\[lay'
# for bonus speed use grep's -F and export LC_ALL=C (mainly for large files)
```

screening: append logs to logfile1-- c-a d to detach \
```screen -L logfile1 <cmd>``` 

screening: output logs to logfile1 --- automatically exists \
```screen -dmS w1; screen -S w1 -X stuff $'<cmd> &> logfile1\n exit\n'``` 

screen running a bash script
```screen -L logsep4_2 -dmS <name> ./run_full_pipeline_on_transcripts.sh 0 1```

word list file manipulation
```cat frequent_word_lists_fiction_60_category | sed 's/^[ \t]*//;s/[ \t]*$//;s/  +/ /g' | sed -r '/^\s*$/d' | cut -d" " -f1 > frequent_word_lists_fiction_60_category_clean```
