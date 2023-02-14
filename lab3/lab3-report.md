# Lab Report 3: `grep`'s Command Line Options

## Part 0: Setting `grep` up for success:

First, I put all of the files from `written_2` into a text file using the command
```
find written_2 > find-results.txt
```
so that grep would be about to search from the paths of the files in `written_2` using a keyword that was part of their path. 

## Part 1: `-n`, printing the line number of output
**Code format: `grep -n search file-pattern`**

* Using `grep -n` with found results:
>```bash
>grep -n "Bahama" find-results.txt
>171:written_2/travel_guides/berlitz2/Bahamas-History.txt
>172:written_2/travel_guides/berlitz2/Bahamas-Intro.txt
>173:written_2/travel_guides/berlitz2/Bahamas-WhatToDo.txt
>174:written_2/travel_guides/berlitz2/Bahamas-WhereToGo.txt
>```

While it doesn't show up with Markdown, the numbers on the left, like `171` and `172`, are colored green. The part in the text file that actually matches the search term, `"Bahama"`, is in red text. The user can see not only see what files match the search term in `written_2`, but can also see the relative position to other files in the directory. 

* Using `grep -n` without any matching results:
>```bash
>grep -n "Egg" find-results.txt
>```

Because there are no results found, like `grep`, `grep -n` doesn't actually return anything. In this way, it can demonstrate to the user that while the command was successfuly operated, there was no found result. 

**Source:** [Vogella](https://www.vogella.com/tutorials/UnixGrep/article.html), found while searching "grep -v command line". 

## Part 2: `--color`, highlights matching text in a color

**Code format: `grep --color search file-pattern`**

* Using `grep --color` with found results:
>```bash
>grep --color "Berk" find-results.txt
>wwritten_2/non-fiction/OUP/Berk
>written_2/non-fiction/OUP/Berk/CH4.txt
>written_2/non-fiction/OUP/Berk/ch1.txt
>written_2/non-fiction/OUP/Berk/ch2.txt
>written_2/non-fiction/OUP/Berk/ch7.txt
>```

Like the code before, the actual colors don't show up on Markdown. In the terminal, the search term `Berk` is in a different color than the regular kind in the terminal, namely, red. This would allow the user to know which part of the path matches their search. This result is very similar to `grep -n`, except the line number of the file isn't printed as well. 

* Changing the color of `grep --color`
>```bash
>export GREP_COLOR='1;32'
>grep --color "Berk" find-results.txt
>wwritten_2/non-fiction/OUP/Berk
>written_2/non-fiction/OUP/Berk/CH4.txt
>written_2/non-fiction/OUP/Berk/ch1.txt
>written_2/non-fiction/OUP/Berk/ch2.txt
>written_2/non-fiction/OUP/Berk/ch7.txt
>```

This is very similar to the command above, except the search term `Berk` is highlighted in green this time, because of the first command. `GREP_COLOR` is an environmental variable, and it's set to `1` to be bold, `32` for the color.

**Source:** ChatGPT. I asked about the different interesting command line options for `grep` and ChatGPT came up with a variety of options. I asked it to elaborate on `--color` and it also told me about how to change the color.


## Part 3: `-w`, matching full words in the file path

**Code format: `grep -w search file-pattern`**


* Exploring what counts as a full word
>```bash
>grep -w "Bahamas" find-results.txt 
>written_2/travel_guides/berlitz2/Bahamas-History.txt
>written_2/travel_guides/berlitz2/Bahamas-Intro.txt
>written_2/travel_guides/berlitz2/Bahamas-WhatToDo.txt
>written_2/travel_guides/berlitz2/Bahamas-WhereToGo.txt
>```

Because we changed the color earlier, `Bahamas` is highlighted in green in each of the search results, just like `--color`. Although each file connects `Bahamas` with a `-` to another word, that counts as a separator from another word. 

* Some more exploration!
>```bash
>grep -w "Berk" find-results.txt 
>written_2/non-fiction/OUP/Berk
>written_2/non-fiction/OUP/Berk/CH4.txt
>written_2/non-fiction/OUP/Berk/ch1.txt
>written_2/non-fiction/OUP/Berk/ch2.txt
>written_2/non-fiction/OUP/Berk/ch7.txt
>```

In this case, a `/` seems to count as a separator too! Combining our knowledge, we know that if there were two different directories that both start with `Berk-` and end with something different, they would both be found using `grep -w`. 

**Sources:** ChatGPT again! It wasn't very helpful in detailing how exactly `-w` worked, as in what counted as a "full word", but I was able to experiment in the `written_2` server. 


## Part 4: `-e`, specifying search patterns

**Code format: `grep -e search file-pattern`**

* Escaping the dash
>```bash
>grep -e -Intro find-results.txt 
>written_2/travel_guides/berlitz2/Algarve-Intro.txt
>written_2/travel_guides/berlitz2/Amsterdam-Intro.txt
>written_2/travel_guides/berlitz2/Athens-Intro.txt
>written_2/travel_guides/berlitz2/Bahamas-Intro.txt
>```

In this case, if we had simply searched `grep -Intro find-results.txt`, the computer may have thought that `-Intro` was what we were trying to input as the command line option. This way, we can in a way "escape" the `-` in the search term. Additionally, the `-Intro` part is also highlighted in green. 

* Searching multiple patterns
>```bash
>grep -e Bahamas -e Athens find-results.txt 
>written_2/travel_guides/berlitz2/Athens-History.txt
>written_2/travel_guides/berlitz2/Athens-Intro.txt
>written_2/travel_guides/berlitz2/Athens-WhatToDo.txt
>written_2/travel_guides/berlitz2/Athens-WhereToGo.txt
>written_2/travel_guides/berlitz2/Bahamas-History.txt
>written_2/travel_guides/berlitz2/Bahamas-Intro.txt
>written_2/travel_guides/berlitz2/Bahamas-WhatToDo.txt
>written_2/travel_guides/berlitz2/Bahamas-WhereToGo.txt
>```

If we had simply input `grep Bahamas Athens find-results.txt`, we would have found the files with `Bahamas` but would not have been able to find anything containing `Athens`. However, we can use `-e` to search for multiple patterns. A file doesn't have to contain both paths, just one-- all paths are output in alphabetical order. 

**Source:** ChatGPT was actually really helpful this time in defining all the terms, "search pattern" and "non-option argument"-- at the very least as helpful as articles I could find online (ex: [phoenixNap](https://phoenixnap.com/kb/grep-multiple-strings)).
