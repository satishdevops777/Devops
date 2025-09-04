### Text Processing Commands
**1. sort**
- The sort command sorts lines of text files
```bash
sort --version
sort --help
sort file #Basic sorting (alphabetical order)
sort -r file #Reverse Order
sort -k2 file #Sort by a specific column (e.g., 2nd column)
```
**2. uniq**
- The uniq command filters out repeated/duplicate adjacent lines.
```bash
uniq file
uniq -c file #Count occurrences of each unique line
```
⚠️ Important: uniq only works on consecutive duplicate lines. That’s why we should always sort first before using uniq.

**3. Combining sort and uniq**
```bash
sort file | uniq #Remove duplicates from file
sort file | uniq -c #Count occurrences of unique lines
```

### File Comparison Commands
**1. diff**
- Compares two files line by line and shows the differences.
```bash
diff file1 file2
```

Example:
file1.txt
```bash
apple
banana
orange
```

file2.txt
```bash
apple
grapes
orange
```

Command:
```bash
diff file1.txt file2.txt
```

Output:
```yaml
2c2
< banana
---
> grapes
```
👉 Meaning: line 2 in file1 is banana, but in file2 it’s grapes.

**2. cmp**
- Compares two files byte by byte (faster, lower-level check).
```bash
cmp file1 file2
```

- If files are identical → no output.
- If files differ → shows first byte & line where difference occurs.

Example:
```bash
cmp file1.txt file2.txt
```

Output:
```bash
file1.txt file2.txt differ: byte 8, line 2
```

✅ When to use what?
- Use diff → when you want to see what the differences are (line-based).
- Use cmp → when you just want to know if two files are identical (byte-level).

### Word/Line/Byte Count – wc
- The wc command prints the number of lines, words, characters, and bytes in a file.
```bash
wc --version
wc --help
wc filename
```
👉 Output (3 columns by default):
```bash
lines   words   bytes   filename
```

✅ Summary:
- wc -l → line count
- wc -w → word count
- wc -c → byte count
- wc -m → character count
- wc Directory

### Compress & Un-compress Commands
**1. tar (Tape Archive)**
- Used to archive multiple files/directories into a single .tar file (without compression).
```bash
tar cvf file.tar . #Create an archive
tar xvf file.tar #Extract an archive
```
  - c → create
  - v → verbose (show files being archived)
  - f → filename
  - x → extract

**2. gzip (GNU Zip)**
- Used to compress files. Works on single files only.
```bash
gzip file.tar #Compress a file
gzip -d file.tar.gz # Un-Compress a file
# or
gunzip file.tar.gz
```
**3. Combining tar + gzip**
- Most common way to create compressed archives:
```bash
#Create compressed archive

tar cvf file.tar dir/      # make tar
gzip file.tar              # compress → file.tar.gz


# Extract compressed archive

gzip -d file.tar.gz        # uncompress → file.tar
tar xvf file.tar           # extract contents

```
✅ Shortcut: Use tar with -z for gzip in one step:
```bash
tar czvf file.tar.gz dir/  # Create tar.gz directly
tar xzvf file.tar.gz       # Extract tar.gz directly
```

### Truncate Command

- The truncate command is used to shrink or extend the size of a file to a specified size.
```bash
truncate -s SIZE filename
truncate -s 10 filename # Shrink file to 10 bytes
truncate -s 0 filename # Set file size to zero (empty the file)
