
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
`for f in `find ~/transcripts_textgrid/ -name "*.txt"`; do grep -oP 'text = \".*?\"' $f | cut -f2- -d= > ~/temp/`basename $f .TextGrid.txt`.txt; done`

grab text withing `<>` and find uique \
`find temp/ -name "*.txt" | xargs cat | grep -oP '\<(.*?)\>'| sort -u`

grab text with `<NIS>` but do not have `<NIS/>` and `</NIS>` \
`grep -rn -P "\<NIS\>" temp/ | grep -P -v "\<NIS\/\>" | grep -P -v "\<\/NIS\>"`

replace `<sdf sdf>` with `<sdfsdf>` \
`find . -name "*.txt" | xargs sed -i -e 's/<INAUDIBLE\ SPEECH>/<INAUDIBLESPEECH>/g' `
