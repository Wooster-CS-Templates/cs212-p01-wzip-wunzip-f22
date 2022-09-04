# Project #1 - Unix Utilities wzip and wunzip

Accept the Project 1 GitHub Classroom assignment posted to Moodle.

Summary of what gets turned in:
* Commited changes to the .c files for each of the utilities below:
  * **wzip.c**
  * **wunzip.c**.
* Each should compile successfully when compiled with the **-Wall** and **-Werror** flags.
* Each should pass the tests supplied to you.

## wzip and wunzip

**wzip** is a file compression tool, and **wunzip** is a file decompression tool. 

The type of compression used here is a simple form of compression called *run-length encoding* (*RLE*). RLE is quite simple: when you encounter **n** characters of the same type in a row, the compression tool (**wzip**) will turn that into the number **n** and a single instance of the character.

Thus, if we had a file with the following contents:
```
aaaaaaaaaabbbb
```
the tool would turn it (logically) into:
```
10a4b
```

However, the exact format of the compressed file is quite important; here, you will write out a 4-byte integer in binary format followed by the single character in ASCII. Thus, a compressed file will consist of some number of 5-byte entries, each of which is comprised of a 4-byte integer (the run length) and the single character. 

To write out an integer in binary format (not ASCII), you should use **fwrite()**. Read the man page for more details. For **wzip**, all output should be written to standard output (the **stdout** file stream, which, as with **stdin**, is already open when the program starts running).

Note that typical usage of the **wzip** tool would thus use shell redirection in order to write the compressed output to a file. For example, to compress the file **file.txt** into a (hopefully smaller) **file.z**, you would type:

```
prompt> ./wzip file.txt > file.z
```
The "greater than" sign is a UNIX shell redirection; in this case, it ensures that the output from **wzip** is written to the file **file.z** (instead of being printed to the screen). You'll learn more about how this works a little later in the course. Note that since the output will be binary data, if you open the output file `file.z` in a text editor, you will most likely only see the characters and not the numbers.
For example, the input file with contents: 
```
aaaaaaaaaabbbb
```
may look like this in a text editor:
```
ab
```
But when viewed with a program that can display a binary file like the terminal program `xxd` you can see:
```
prompt> xxd -b out.z
00000000: 00001010 00000000 00000000 00000000 01100001 00000100  ....a.
00000006: 00000000 00000000 00000000 01100010                    ...b
```
The content after the colon (`:`) and before the text representations (starting with `....a`) is the correct binary data. The `xxd` program displays binary data in hexidecimal format if you remove the `-b` option. Note that `00001010` is 10 in deciaml, and `01100001` is 97 which is the ASCII for lower case `a`. If you are wondering why the binary `00001010 00000000 00000000 00000000` means 10 is due to the use of little-endian byte order (you can read more about this [here](https://www.section.io/engineering-education/what-is-little-endian-and-big-endian/)). The important thing to remember is that the groups of 4 bytes (8 bits each) representing your number portion of the data are automatically stored in reverse order. So:
```
00001010 00000000 00000000 00000000
```
should actually be read as:
```
00000000 00000000 00000000 00001010
```
which is 10. Notice how the order of the bits didn't change, just the ordering of the entire bytes. You don't have to worry about this when considering the ASCII characters because their data is always contained within one byte. See if you can find the 4 and the letter b in the rest of the data above.

The **wunzip** tool simply does the reverse of the **wzip** tool, taking in a compressed file and writing (to standard output again) the uncompressed results. For example, to see the contents of **file.txt**, you would type:

```
prompt> ./wunzip file.z
```
and the output for the example above should be:
```
aaaaaaaaaabbbb
```

**wunzip** should read in the compressed file (likely using **fread()**) and print out the uncompressed output to standard output using **printf()**.

**Details**

* Correct invocation should pass one or more files via the command line to the program; if no files are specified, the program should exit with return code 1 and print "wzip: file1 [file2 ...]" (followed by a newline) or "wunzip: file1 [file2 ...]" (followed by a newline) for **wzip** and **wunzip** respectively. 
* The format of the compressed file must match the description above exactly (a 4-byte integer followed by a character for each run).
* Do note that if multiple files are passed to **wzip**, they are compressed into a single compressed output, and when unzipped, will turn into a single uncompressed stream of text (thus, the information that multiple files were originally input into **wzip** is lost). The same thing holds for **wunzip**.

## Grading 18 points

* 6 points - All wzip tests pass
* 6 points - All wunzip tests pass
* 6 points - Code follows the [class style guidelines](http://csweb.wooster.edu/dguarnera/cs212/resources/guides/style-guidelines.html)
