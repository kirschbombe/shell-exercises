---
title: "Finding Things"
teaching: 15
exercises: 0
questions:
- "How can I find files?"
- "How can I find things in files?"
---

> ## Using `grep`
>
> Referring to `haiku.txt`
> presented at the begin of this topic,
> which command would result in the following output:
>
> ~~~
> and the presence of absence:
> ~~~
> {: .output}
>
> 1. `grep "of" haiku.txt`
> 2. `grep -E "of" haiku.txt`
> 3. `grep -w "of" haiku.txt`
> 4. `grep -i "of" haiku.txt`
>
> > ## Solution
> > The correct answer is 3, because the `-w` flag looks only for whole-word matches.
> > The other options will all match "of" when part of another word.
> {: .solution}
{: .challenge}

> ## `find` Pipeline Reading Comprehension
>
> Write a short explanatory comment for the following shell script:
>
> ~~~
> wc -l $(find . -name '*.dat') | sort -n
> ~~~
> {: .bash}
>
> > ## Solution
> > 1. Find all files with a `.dat` extension in the current directory
> > 2. Count the number of lines each of these files contains
> > 3. Sort the output from step 2. numerically
> {: .solution}
{: .challenge}

> ## Matching and Subtracting
>
> The `-v` flag to `grep` inverts pattern matching, so that only lines
> which do *not* match the pattern are printed. Given that, which of
> the following commands will find all files in `/data` whose names
> end in `s.txt` (e.g., `animals.txt` or `planets.txt`), but do
> *not* contain the word `net`?
> Once you have thought about your answer, you can test the commands in the `data-shell`
> directory.
>
> 1.  `find data -name '*s.txt' | grep -v net`
> 2.  `find data -name *s.txt | grep -v net`
> 3.  `grep -v "temp" $(find data -name '*s.txt')`
> 4.  None of the above.
>
> > ## Solution
> > The correct answer is 1. Putting the match expression in quotes prevents the shell
> > expanding it, so it gets passed to the `find` command.
> >
> > Option 2 is incorrect because the shell expands `*s.txt` instead of passing the wildcard
> > expression to `find`.
> >
> > Option 3 is incorrect because it searches the contents of the files for lines which
> > do not match "temp", rather than searching the file names.
> {: .solution}
{: .challenge}

> ## Tracking a Species
>
> Leah has several hundred
> data files saved in one directory, each of which is formatted like this:
>
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> ~~~
> {: .source}
>
> She wants to write a shell script that takes a species as the first command-line argument
> and a directory as the second argument. The script should return one file called `species.txt`
> containing a list of dates and the number of that species seen on each date.
> For example using the data shown above, `rabbits.txt` would contain:
>
> ~~~
> 2013-11-05,22
> 2013-11-06,19
> ~~~
> {: .source}
>
> Put these commands and pipes in the right order to achieve this:
>
> ~~~
> cut -d : -f 2  
> >  
> |  
> grep -w $1 -r $2  
> |  
> $1.txt  
> cut -d , -f 1,3  
> ~~~
> {: .bash}
>
> Hint: use `man grep` to look for how to grep text recursively in a directory
> and `man cut` to select more than one field in a line.
>
> An example of such a file is provided in `data-shell/data/animal-counts/animals.txt`
>
> > ## Solution
> >
> > ```
> > grep -w $1 -r $2 | cut -d : -f 2 | cut -d , -f 1,3  > $1.txt
> > ```
> > {: .source}
> >
> > You would call the script above like this:
> >
> > ```
> > $ bash count-species.sh bear .
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Little Women
>
> You and your friend, having just finished reading *Little Women* by
> Louisa May Alcott, are in an argument.  Of the four sisters in the
> book, Jo, Meg, Beth, and Amy, your friend thinks that Jo was the
> most mentioned.  You, however, are certain it was Amy.  Luckily, you
> have a file `LittleWomen.txt` containing the full text of the novel
> (`data-shell/writing/data/LittleWomen.txt`).
> Using a `for` loop, how would you tabulate the number of times each
> of the four sisters is mentioned?
>
> Hint: one solution might employ
> the commands `grep` and `wc` and a `|`, while another might utilize
> `grep` options.
> There is often more than one way to solve a programming task, so a
> particular solution is usually chosen based on a combination of
> yielding the correct result, elegance, readability, and speed.
>
> > ## Solutions
> > ```
> > for sis in Jo Meg Beth Amy
> > do
> > 	echo $sis:
> >	grep -ow $sis LittleWomen.txt | wc -l
> > done
> > ```
> > {: .source}
> >
> > Alternative, slightly inferior solution:
> > ```
> > for sis in Jo Meg Beth Amy
> > do
> > 	echo $sis:
> >	grep -ocw $sis LittleWomen.txt
> > done
> > ```
> > {: .source}
> >
> > This solution is inferior because `grep -c` only reports the number of lines matched.
> > The total number of matches reported by this method will be lower if there is more
> > than one match per line.
> {: .solution}
{: .challenge}

> ## Finding Files With Different Properties
>
> The `find` command can be given several other criteria known as "tests"
> to locate files with specific attributes, such as creation time, size,
> permissions, or ownership.  Use `man find` to explore these, and then
> write a single command to find all files in or below the current directory
> that were modified by the user `ahmed` in the last 24 hours.
>
> Hint 1: you will need to use three tests: `-type`, `-mtime`, and `-user`.
>
> Hint 2: The value for `-mtime` will need to be negative---why?
>
> > ## Solution
> > Assuming that Nelle’s home is our working directory we type:
> >
> > ~~~
> > $ find ./ -type f -mtime -1 -user ahmed
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}
