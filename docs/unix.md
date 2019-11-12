# A (Very Brief) Introduction to (Very Simple) UNIX

[Return to the main page](../index.html)
<br><br>

The following exercises use this [test directory](../UNIX/test_directory.zip).  The directory contains four files:

```
test_fasta_file.fna
test_gene_list_1.txt
test_gene_list_2.txt
test_gene_list_3.txt
```


### Simple Exercises

1. Moving to a directory ('folder'), back again, and figuring out what directory you are in:
   ```
   cd test_directory
   cd ..
   pwd
   
   cd test_directory
   pwd
   ```

   Directories are hierarchical. You can move multiple directories in one go by using ```cd first_directory/second_directory```. You can step backwards with the command ```cd ..```. The ```pwd``` ('print working directory') command tells you what directory you are currently in.

2. Seeing what files are available:
   ```
   ls
   ```

3. Looking at the contents of a file:
   ```
   cat test_fasta_file.fna
   ```
   
   This command shows the entire file. That is fine for a small 8 KB file, but you would not want to use this command on a 50 GB file!

4. Looking at the file in stages:
   ```
   less test_fasta_file.fna
   ```
   
   You can use the space bar to move down through the file, or use the up and down arrows to move – unsurprisingly – up and down through the file.  Press *q* to quit.


5. Looking at the beginning and end of a file:
   ```
   head -n 8 test_fasta_file.fna
   tail -n 8 test_fasta_file.fna
   ```

6. Counting the number of lines in a file:
   ```
   wc -l test_fasta_file.fna
   ```

7. Finding and counting lines in a file that contain a certain pattern (here, the > symbol):
   ```
   grep ">" test_fasta_file.fna
   grep ">" test_fasta_file.fna -c
   ```

8. Sending the output of one command to another command ('piping'):
   ```
   head -n 50 test_fasta_file.fna | tail -n 10
   ```

9. Redirecting output to a new file, or appending to an existing file, instead of writing to the screen:
   ```
   head -n 10 test_fasta_file.fna > output.txt
   cat output.txt
   
   head -n 10 test_fasta_file.fna >> output.txt
   cat output.txt
   ```


### Extended Exercises

1. Extracting a column of gene counts:
   ```
   cat test_gene_list_1.txt
   cat test_gene_list_2.txt
   
   cut -f 2 test_gene_list_1.txt
   ```

2. Sorting a numeric column (here, column 2) by gene counts (and then returning the reverse order):
   ```
   sort -k 2 -n test_gene_list_1.txt
   sort -k 2 -n -r test_gene_list_1.txt
   ```
   
   > Note that different ‘flags’ are used to indicate columns in different programs. ```cut``` uses ```-f```, but  ```sort``` uses ```-k```. Different programs were written by different people at differnt times and so use different flags.

3. Joining files ‘vertically’ and ‘horizontally’:
   ```
   cat test_gene_list_1.txt test_gene_list_2.txt
   join test_gene_list_1.txt test_gene_list_2.txt
   ```
   
4. Extracting only the unique lines in a file:
   ```
   cat test_gene_list_3.txt
   sort test_gene_list_3.txt | uniq
   sort test_gene_list_3.txt | uniq -c
   ```
   
   Compare the following command – what is it doing differently?
   ```
   uniq test_gene_list_3.txt
   ```


### UNIX Cheat Sheet

Task | Command
:---- | :----
Find information about a command | ```man command_name```
Change directory | ```cd directory_name```
Print working directory | ```pwd```
List files | ```ls```
Remove files | ```rm file_names```
Look at the contents of a file | ```cat file_names```
Look at the contents of a file in stages  | ```less file_name```
Look at the beginning of a file | ```head [-n integer] file_names```
Look at the end of a file | ```tail [-n integer] file_names```
Count the number of lines in a file | ```wc -l file_names```
Pipe output between commands | ```command_one | command_two```
Redirect output to a file | ```command > file_name```
Append output to a file | ```command >> file_name```
Find instances of a pattern in a file | ```grep "pattern" file_names```
Count instances of a pattern in a file | ```grep "pattern" file_names -c```
Extract a column from a file | ```cut -f integer file_name```
Sort a file on a column | ```sort -k integer file_name```
Sort a file numerically on a column | ```sort -k integer -n file_name```
Sort a file numerically in reverse order | ```sort -k integer -n -r file_name```
Join files vertically | ```cat file_names```
Join files horizontally | ```join file_names```
Find unique sequential entries in a file | ```uniq file_name```
Find unique entries in a file | ```sort file_name | uniq```
Count unique entries in a file | ```sort file_name | uniq -c```


### Pearls of Wisdom

1. To save time and avoid mistakes, use tab completion when typing
   * Names of Unix commands if they are more than 2–3 characters long
   * Names of scripts
   * Names of files and directories that are arguments to commands<br>

2. Pay attention when copying and pasting commands from Word, PDFs or similar files. They often contain ‘hidden’ characters that look similar, but are actually different (e,g., '–' instead of '-').

3. Use Unix tools like ```cat```, ```head```, ```tail``` and ```less``` for viewing text files. Try not to use Word or Excel – however tempting – because these can accidentally modify the file (or for big files, crash your machine by running out of memory).

4. If you have problems using TextEdit or similar graphical interfaces for writing data files, try using ```nano``` or ```pico``` or more advanced equivalents. For writing and modifying code, use a plain-text editor like TextEdit or TextWrangler or …

5. Use long descriptive names for your data files and scripts (avoid white spaces; use underscores instead). You will make lots of files and appreciate obvious naming later!

<br>
[Return to the main page](../index.html)

