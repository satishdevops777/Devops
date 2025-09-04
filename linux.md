### Finding Files
**1. find**
- Search for files/directories in a given location.
Syntax:
```bash
find [path] [options] [expression]

find /path/to/search -name "filename" #Find by name
find /home/user -name "notes.txt" #Example
find /path/to/search -iname "filename" #Case-insensitive search

find /path/to/search -type f -name "*.log" #Find by file type
#(-type f = file, -type d = directory)

find /var/log -size +10M #Find by size (Find files larger than 10 MB)
```

**2. locate**
- Search files quickly using a prebuilt index/database.
- Find file by name
```bash
locate filename
```
- Update the database (if new files aren’t found)
```bash
sudo updatedb
```
⚡ locate is much faster than find but may not reflect recent changes unless you run updatedb.

### Wildcards in Linux
- Wildcards are special characters used in shell commands to match patterns in filenames or strings.

**1. * (Asterisk) → Matches zero or more characters**
```bash
ls *.txt
```
👉 Matches all files ending with .txt (e.g., a.txt, notes.txt, test123.txt).

**2. ? (Question Mark) → Matches exactly one character**
```bash
ls file?.txt
```
👉 Matches file1.txt, fileA.txt, but not file10.txt.

**3. [] (Square Brackets) → Matches any one character in the range/class**
```bash
ls file[1-3].txt
```
👉 Matches file1.txt, file2.txt, file3.txt.
```bash
ls file[a-c].txt
```
👉 Matches filea.txt, fileb.txt, filec.txt.

**4. {} (Brace Expansion) → Matches a sequence or set**
```bash
echo file{1..5}.txt
```
👉 Expands to: file1.txt file2.txt file3.txt file4.txt file5.txt
```bash
echo {apple,banana,orange}.txt
```
👉 Expands to: apple.txt banana.txt orange.txt

✅ Summary:
- * → zero or more characters
- ? → exactly one character
- [] → character set/range
- {} → explicit list or sequence

### Linux File Types
- In Linux, everything is treated as a file. You can see file types using:
```bash
ls -l
```
👉 The first character of the output indicates the file type.

**1. - Regular File**
```bash
-rw-r--r--  1 user user  1200 Sep  4  notes.txt
```
**2. d Directory**
```bash
drwxr-xr-x  2 user user  4096 Sep  4  Documents
```
**3. Symbolic Link (Soft Link)**
```bash
lrwxrwxrwx  1 user user    10 Sep  4  linkfile -> notes.txt 
```
**4. c Character Device File**
- Represents hardware devices that transfer data character by character.
- Examples: Keyboard, Mouse, Serial ports (/dev/tty, /dev/random).

**5. b Block Device File**
- Represents devices that handle data in blocks.
- Examples: Hard drives, USB drives (/dev/sda, /dev/sdb).

**6. s Socket File**
- Used for network communication between processes.
- Allows two processes to exchange data.
- Example: /var/run/docker.sock

**7. p Named Pipe (FIFO)**
- Special file for inter-process communication (IPC).
- Works in FIFO (First In, First Out) order.
- Example:
```bash
mkfifo mypipe
echo "hello" > mypipe
cat < mypipe
```
✅ Summary Table
| Symbol | File Type         | Example Use Case                     |
| ------ | ----------------- | ------------------------------------ |
| `-`    | Regular file      | Text, binary, scripts, configs       |
| `d`    | Directory         | Folders                              |
| `l`    | Symbolic link     | Shortcuts/pointers                   |
| `c`    | Char device       | Keyboard, mouse, serial ports        |
| `b`    | Block device      | Hard drives, USB drives              |
| `s`    | Socket            | Process communication (e.g., Docker) |
| `p`    | Named pipe (FIFO) | IPC within system processes          |


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
