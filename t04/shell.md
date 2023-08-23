Let's create a scenario where we have a series of log files and we want to search for a particular keyword and perform certain statistics. 

Let's assume we have 3 log files: log1.txt, log2.txt, log3.txt and they have some random text and also the keyword we are looking for, say "Error".

Log File Contents:
- log1.txt: "Info: System boot, Error: Disk not found, Info: System recovery"
- log2.txt: "Info: System boot, Info: System recovery"
- log3.txt: "Error: Network not found, Error: Disk not found"

1. Find keyword "Error" in log files:

Using `grep` command, we can find the line that contains the keyword "Error".

```bash
grep 'Error' log1.txt log2.txt log3.txt
```

2. Count the number of "Error" occurrences:

We can count the number of times the keyword "Error" occurred using `grep -c`.

```bash
grep -c 'Error' log1.txt log2.txt log3.txt
```

3. Find specific "Error" and count:

To find a specific error like "Disk not found", you can use the following command
```bash
grep -c 'Error: Disk not found' log1.txt log2.txt log3.txt
```

4. Using sed to replace a keyword:

`sed` is a stream editor for filtering and transforming text. Say we want to change "Error" to "Issue" in the log files.

```bash
sed 's/Error/Issue/g' log1.txt log2.txt log3.txt
```
This command will replace all occurrences of "Error" with "Issue" in the log files.

5. Using awk for statistics:

`awk` is a powerful tool for complex text processing. Let's say we want to count the number of "Info" and "Error" messages in all the logs.

```bash
awk '{for(i=1; i<=NF; i++) a[$i]++} END {for(k in a) print k, a[k]}' log1.txt log2.txt log3.txt
```

This command will go through every word in the files and count their occurrences, then print each word and its total count.

Note: These commands scan the files and print the output to the terminal. If you want to permanently change the files or save the output, you'll need to redirect the output to another file. For example:

```bash
# Replace "Error" with "Issue" and save the changes into a new file
sed 's/Error/Issue/g' log1.txt > log1_modified.txt

# Count the number of "Error" occurrences and save the output into a file
grep -c 'Error' log1.txt log2.txt log3.txt > error_counts.txt

# Count the number of "Info" and "Error" messages and save the output into a file
awk '{for(i=1; i<=NF; i++) a[$i]++} END {for(k in a) print k, a[k]}' log1.txt log2.txt log3.txt > stats.txt
```

Remember to replace "log1.txt", "log2.txt", "log3.txt" with your actual log files. Also, the keyword "Error" and "Info" should be replaced with your actual keywords.
