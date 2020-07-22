Project Gutenberg Australia word lists
======================================

These are frequency-sorted lists of words seen in Project Gutenberg Australia
books that were available for download in plain text format.  The approach was
as follows:

```
cut -d, -f1-3 catalogue.txt | grep ',txt$' | sed 's.,./.; s/,/./; s,^,http://gutenberg.net.au/,' > urls
wget -i urls
rm -- `fgrep -l pagenotfound [0-9]*.txt`
cat [0-9]*.txt | sed -r 's/--+/ /g' | tr -cs "A-Za-z'-" '\n' | egrep "^([A-Za-z]|[A-Za-z][A-Za-z'-]{0,99}[^-])$" | fgrep -v "''" | sort -S20G | uniq -c | sort -S20G -rn > gutenberg-all-words-with-counts.txt &
cat `grep -l '[^0-9]19[0-9][0-9][^0-9]' [0-9]*.txt` | sed -r 's/--+/ /g' | tr -cs "A-Za-z'-" '\n' | egrep "^([A-Za-z]|[A-Za-z][A-Za-z'-]{0,99}[^-])$" | fgrep -v "''" | sort -S20G | uniq -c | sort -S20G -rn > gutenberg-19xx-words-with-counts.txt
wait
grep -v "[A-Z'-]" gutenberg-all-words-with-counts.txt > gutenberg-all-lowercase-words-with-counts.txt
grep -v "[A-Z'-]" gutenberg-19xx-words-with-counts.txt > gutenberg-19xx-lowercase-words-with-counts.txt
for f in gutenberg-*-words-with-counts.txt; do cut -c9- < $f > ${f/-with-counts}; done
for f in gutenberg-*-words-with-counts.txt; do sed -n '/ 9 /q; p' $f | cut -c9- > ${f/-with-counts/-10plus}; done
for f in gutenberg-*-words-with-counts.txt; do sed -n '/ 99 /q; p' $f | cut -c9- > ${f/-with-counts/-100plus}; done
for f in gutenberg-*-words-with-counts.txt; do sed -n '/ 999 /q; p' $f | cut -c9- > ${f/-with-counts/-1000plus}; done
```

A total of 1962 books were processed resulting in the `gutenberg-all-*` files.

Since Project Gutenberg only includes books where copyright has expired, the
words are biased towards older English.  To partially mitigate this, processed
separately was a subset consisting of 1172 books that include the numbers 19xx
(hopefully were written in those years or an author lived into those years).
This resulted in the `gutenberg-19xx-*` files.

The `*lowercase*` files include only all-lowercase words, and are of pretty
high quality.  They do not include any words with capital letters, nor any
words with dashes nor apostrophes.

The more complete lists (without `*lowercase*` in the filenames) do include
those other categories of words, but as a result are of much lower quality.
For example, there's no reliable way to distinguish a word that is capitalized
because it's the first word in a sentence (in which case we'd ideally convert
it to lowercase and count as such) or also because it's a given name (in which
case we should preserve the capitalization).  The current approach doesn't even
try to distinguish these cases (always keeping the capitalization intact, but
also always excluding such words from the `*lowercase*` files).

Please do not link directly to individual files in this repository as they're
likely to be moved around in future revisions.  Link to the repository instead.
