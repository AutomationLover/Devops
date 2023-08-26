
Mastering Linux commands is crucial for SRE (Site Reliability Engineering) or DevOps roles. 
Here are some examples: 

```
- sed 
    ```
    sed 's/foo/bar/g' filename # This replaces all occurrences of 'foo' with 'bar' in the file
    ```

- awk
    ```
    awk '{print $1}' filename # This prints the first field in each line of the file
    ```

- grep
    ```
    grep 'pattern' filename # This searches for the pattern in the file
    ```

- find
    ```
    find /home/user -name 'file.txt' # This searches for 'file.txt' in '/home/user' directory
    ```

- curl
    ```
    curl https://www.google.com # This fetches the HTML of Google's homepage
    ```

- ps
    ```
    ps aux # This displays the currently active processes
    ```

- netstat
    ```
    netstat -tuln # This displays network connections, listening ports, and the protocol used
    ```

- iptables
    ```
    iptables -L # This lists all the rules in the selected chain
    ```

- ssh
    ```
    ssh user@hostname # This connects to the hostas the specified user
    ```

- scp
    ```
    scp sourcefile user@hostname:destination # This copies the sourcefile to the destination directory on the remote host
    ```

- rsync
    ```
    rsync -av source_directory destination_directory # This syncs the source directory to the destination directory
    ```
```
