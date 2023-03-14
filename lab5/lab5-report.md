# Lab Report 5: Lab Report 3 Electric Boogaloo
**`find` is being used in the `docsearch` directory first introduced in Week 4.**

## Part 1: `-type`, finding types of files
**Code format: `find -type c` where `c` stands for a type**

* Using `find -type` to find all directories:
>```bash
>find -type d
>.
>./.git
>./.git/info
>./.git/hooks
>./.git/branches
>./.git/refs
>./.git/refs/heads
>./.git/refs/tags
>./.git/refs/remotes
>./.git/refs/remotes/origin
>./.git/objects
>./.git/objects/pack
>./.git/objects/info
>./.git/logs
>./.git/logs/refs
>./.git/logs/refs/remotes
>./.git/logs/refs/remotes/origin
>./.git/logs/refs/heads
>./lib
>./written_2
>./written_2/non-fiction
>./written_2/non-fiction/OUP
>./written_2/non-fiction/OUP/Abernathy
>./written_2/non-fiction/OUP/Berk
>./written_2/non-fiction/OUP/Castro
>./written_2/non-fiction/OUP/Fletcher
>./written_2/non-fiction/OUP/Kauffman
>./written_2/non-fiction/OUP/Rybczynski
>./written_2/travel_guides
>./written_2/travel_guides/berlitz1
>./written_2/travel_guides/berlitz2
>```

We are starting out in the `docsearch` base directory, so it's really intersting that `find` actually counts that directory in its search. It can also reveal some of the hidden directories that I wouldn't have been able to see otherwise, such as the `./.git` directory. 

* Using `find -type` to find files:
>```bash
>find type -f
>./CH4.txt
>./ch1.txt
>./ch2.txt
>./ch7.txt
>```

At this point, we have gone into the `Berk` directory (mainly to avoid a huge long list of files) and all the files in it are returned. Not only does `find -type` return the file name, it returns the whole path. 

**Source:** `man find` 

## Part 2: `-size`, finding files that are a specific size

**Code format: `find -size n` where n is a type of unit**

* Using `find -size` with an upper bound:

>```bash
>find -size -10M
>.
>./CH4.txt
>./ch1.txt
>./ch2.txt
>./ch7.txt
>```

In this case, `M` stands for megabytes, and the `-` sign indicates a search for files under 10 megabytes. Like the code before, the path names are fully written out. However, -size is interesting because it starts with the current directory and has included the whole directory itself, indicating that the  directory folder is under 10 megabytes.

* Using `find -size` with a lower bound:
>```bash
>find -size +100k
>./CH4.txt
>./ch2.txt
>```

This is very similar to the command above, except we have set a lower bound of file size by using `+` and have changed the unit to kilobytes by using the option `k`. This time, the current directory was not included, which means that it doesn't have more the 100 kilobytes, which is a little weird because from my view (and ChatGPT's), a directory is the sum of its memory as well as the files and subdirectories inside of it. If the files inside are greater than 100 kilobytes, I'm not sure why the directory wouldn't be either. I asked ChatGPT for a while about this, although it didn't have very helpful things to say about why that is. 

**Source:** ChatGPT. I asked it to elaborate on the `-size` command.


## Part 3: `-exec`, executing different commands on the search result

**Code format: `-exec command {}`**


* Deleting files with `-exec rm`
>```bash
>find -name "ch1.txt" -exec rm {} \;
>ls
>CH4.txt  ch2.txt  ch7.txt
>```

We take the file found from `find -name "ch1.txt"` and insert it where the `{}` would go, effectively removing that file from the directory as shown by the list of files there. The `\;` at the end ends the command--I tried it without it and my terminal said there was a missing argument to `-exec`.

* Copying files to another directory with `-exec cp`
>```bash
>find -name "ch2.txt" -exec cp {} Bork/  \;
>cd Bork/
>ls
>ch2.txt
>```

I have created a subdirectory in `Berk` called `Bork` and have copied over the text file `ch2.txt`. The format is really similar to the previous command, with the exception of having to put the directory that I was copying the file into after the `{}`, as that's how the regular `cp` command works. I think it would've been interesting to try this with a directory that wasn't in `Berk`. 

**Sources:** ChatGPT again! I got the base from there, but had to experiment a little before I could get it to work. 


## Part 4: `-empty`, finding empty files or directory

**Code format: `find -empty`**

* Finding empty directories in the whole repository
>```bash
>[cs15lwi23auo@ieng6-203]:docsearch:371$ find -empty
>./.git/branches
>./.git/refs/tags
>./.git/objects/info
>```

Out of all the files and directories, these are the things that are entirely empty. I wonder why they're empty, is it because the repository currently doesn't have branches, tags for the refs, or info for objects? Obviously there's a use to these directories if they exist, but I wonder why nothing is in them for this specific repository. 

* Searching for empty files
>```bash
>find -type f -empty
>```

Nothing was found! So it's good to know that everything that is empty is actually a directory. I also like that we can combine the different options that we've explored for different results. I tried inserting `-f` before and after `-empty`, but that wasn't an option for `-empty`.

**Source:** `man find` for the original command and ChatGPT for the way to restrict searches to files. 
