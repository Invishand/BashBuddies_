# Bash [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)
## Points of Consideration 

- Case Sensitivity
- Shares aliases with PowerShell:
  - ls, curl, man, set, sleep, wget, mkdir, mv, echo, cat, cd, cp, diff, history, pwd, rm, sort, tee, type
- [man bash](http://man.he.net/?topic=bash&section=all)

## Variables

Variables must be directly touching the assignment operator `=`:
- `X="Hello World"  `✔️
- `X = "Hello World"`❌
- `X= "Hello World" `❌
- `X ="Hello World" `❌

Variables are invoked using `$`:
- `echo $X`
- `echo ${X}`

## Syntax [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)

### Types

- **Arrays**: Store multiple values.
  - **Indexed arrays**: `arr=(value1 value2 value3)`
  - **Associative arrays** (declare explicitly): `declare -A arr; arr[key]=value`
- **Strings**: Text data that can be manipulated.
- **Integers**: Used for arithmetic operations.

### Bash-isms

- `|` Pass output of one command to another, stringing them together.
- `&` Place at the end of a command to send it to the background.
- `-` Represents STDOUT, returns output to the screen when used with commands like `echo`.
- `>` Sends output to a file.
- `>>` Appends output to a file.
- `:` Evaluates as true.
- `[[]]` Enhances the traditional `[ ]` test command, allowing for more complex expressions and pattern matching.
- `$` Prefix for accessing the value of a variable.
- `/dev/<tcp|udp>/<HOST>/<PORT>` Used for creating TCP/UDP connections directly from the shell.
- `/dev/null` Anything sent here is thrown away.
- `$()` Command substitution, runs the command inside the parentheses and substitutes its output.
- `history` Shows the command history list.
- `!!` Repeats the last command executed.
- `!<NUM>` Replacing number with the history number you wish to execute.
- `~/.bash_history` The default location for logging commands set by the `$HISTFILE` variable.
- ` ` By placing a space before your command, it does not store that command to `.bash_history`.
- `$HISTCONTROL` Variable controls which commands are stored in `.bash_history` (ignorespaces, ignoredups, both).
- `$HISTSIZE` Dictates the size (number of lines) of history to retain.
- `$HISTFILESIZE` Dictates the size in bytes of history to retain.
- `^FIND^REPLACE` Rerun the last command, finding and replacing the text.
- `.bash_profile` or `.profile` The code that is run when you login. Useful to store functions and aliases.

### Conditional Statements

#### if

- **Syntax**: `if [ condition ]; then actions; fi`
- **Example**: `if [[ $X -gt 0 ]]; then echo "X is greater than zero"; fi`
  - **Output**: `X is greater than zero`

### Loops

#### for loop

- **Syntax**: `for var in list; do actions; done`
- **Example**: `for i in {1..5}; do echo "Iteration $i"; done`
  - **Output**:
    - `Iteration 1`
    - `Iteration 2`
    - `Iteration 3`
    - `Iteration 4`
    - `Iteration 5`

#### while loop

- **Syntax**: `while [ condition ]; do actions; done`
- **Example**: `while [[ $X -lt 5 ]]; do echo $X; X=$((X+1)); done`
  - **Output**:
    - `0`
    - `1`
    - `2`
    - `3`
    - `4`
    - `5`

### Brace Expansion

- **Example**:
  - **Command**: `echo {A..C}{1..3}`
  - **Output**: `A1 A2 A3 B1 B2 B3 C1 C2 C3`

# Binaries [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)
## Text Processing and Filtering

### `awk`

- **[man awk](http://man.he.net/?topic=awk&section=all)**
- **Common Flags**: `-F` (field separator), `-v` (variable assignment)
- **Use Cases**: Transforming text data, extracting columns, and complex filtering.
- **Examples**:
  - **Command**: `echo -e "name,age\nAlice,21\nBob,22" | awk -F',' '{print $2}'` 
  - **Outputs**:
    - `age`
    - `21`
    - `22`

### `sed`

- **[man sed](http://man.he.net/?topic=sed&section=all)**
- **[sed handout](https://github.com/mwilco03/BashBuddies/blob/main/handouts/sed_handout.md)**
- **Common Flags**: `-i` (edit files in-place), `-e` (add script)
- **Use Cases**: Find and replace text within files.
- **Example**: 
  - **Command**: `echo "hello world" | sed 's/world/universe/'`
  - **Output**: `hello universe`

### `tr`

- **[man tr](http://man.he.net/?topic=tr&section=all)**
- **Common Flags**: `-d` (delete characters), `-s` (squeeze repeated characters)
- **Use Cases**: Convert lowercase to uppercase, remove or replace characters.
- **Examples**: 
  - **Command**: `echo "hello" | tr '[:lower:]' '[:upper:]'`
  - **Output**: `HELLO`
  - **Command**: `echo "hello\tworld" | tr '\t' ' '`
  - **Output**: `hello world`
        
### `cut`

- **[man cut](http://man.he.net/?topic=cut&section=all)**
- **Common Flags**: `-f` (fields), `-d` (delimiter)
- **Use Cases**: Extract columns from text data.
- **Example**:  
  - **Command**: `echo "name,age" | cut -d',' -f1`
  - **Output**: `name`

### `grep`

- **[man grep](http://man.he.net/?topic=grep&section=all)**
- **Common Flags**: `-i` (ignore case), `-r` (recursive), `-E` (extended regex), `-v` (exclude)
- **Use Cases**: Search for text in files, highlighting matching lines.
- **Examples**:
  - **Command**: `echo "hello" | grep 'Hello'`
  - **Output**: Nothing no match (case sensitivity)
  - **Command**: `echo "hello" | grep -i 'Hello'`
  - **Output**: `hello`
  - **Command**: `echo "hello" | grep -v 'ello'`
  - **Output**: Nothing excluded match

### `uniq`

- **[man uniq](http://man.he.net/?topic=uniq&section=all)**
- **Common Flags**: `-c` (count), `-u` (unique)
- **Use Cases**: Finding or filtering out duplicate entries in a sorted file.
- **Examples**: 
  - **Command**: `echo -e "apple\napple\nbanana" | sort | uniq | sort`
  - **Outputs**:
    - `apple`
    - `banana`
  - **Command**: `echo -e "apple\napple\nbanana\napple" | uniq`
  - **Outputs**:
    - `apple`
    - `banana`
    - `apple`

### `awk '!a[$0]++'`

- **[man awk](http://man.he.net/?topic=awk&section=all)**
- **Use Case**: Similar to `uniq`, but works on unsorted data.
- **Example**:  
  - **Command**: `echo -e "apple\napple\nbanana" | awk '!a[$0]++'`
  - **Outputs**:
    - `apple`
    - `banana`

## File Manipulation / Analysis [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)

### `cat`

- **[man cat](http://man.he.net/?topic=cat&section=all)**
- **Common Flags**: `-n` (number all output lines)
- **Use Cases**: Combine multiple files or display file contents.
- **Example**:  
  - **Command**: `cat file.txt`
  - **Output**: Outputs the contents of `file.txt`

### `tac`

- **[man tac](http://man.he.net/?topic=tac&section=all)**
- **Use Case**: Display file contents starting from the last line.
- **Example**:  
  - **Command**: `echo -e "line1\nline2\nline3" | tac`
  - **Outputs**:
    - `line3`
    - `line2`
    - `line1`

### `lolcat`

- **[man lolcat](http://man.he.net/?topic=lolcat&section=all)**
- **Use Case**: Display file contents with color.
- **Example**:  
  - **Command**: `echo -e "line1\nline2\nline3" | lolcat`
  - **Output**: Colorful display

### `head`

- **[man head](http://man.he.net/?topic=head&section=all)**
- **Common Flags**: `-n` (number of lines)
- **Use Cases**: View the beginning of a file.
- **Example**:  
  - **Command**: `echo -e "line1\nline2\nline3" | head -n 2`
  - **Outputs**:
    - `line1`
    - `line2`

### `tail`

- **[man tail](http://man.he.net/?topic=tail&section=all)**
- **Common Flags**: `-n` (number of lines), `-f` (follow file growth)
- **Use Cases**: Monitor changes in file content dynamically.
- **Example**: 
  - **Command**: `echo -e "line1\nline2\nline3" | tail -n 2`
  - **Outputs**:
    - `line2`
    - `line3`

### `wc`

- **[man wc](http://man.he.net/?topic=wc&section=all)**
- **Common Flags**: `-l` (lines), `-w` (words), `-c` (bytes)
- **Use Cases**: Count lines, words, or characters in a file.
- **Example**:  
  - **Command**: `echo "hello world" | wc -w`
  - **Output**: `2`
  - **Command**: `echo -e "hello\nworld" | wc -l`
  - **Output**: `2`

## Data Manipulation [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)

### `seq`

- **[man seq](http://man.he.net/?topic=seq&section=all)**
- **Common Flags**: `-s` (separator), `-w` (equal width)
- **Use Case**: Generate sequences of numbers for loops and lists.
- **Example**:
  - **Command**: `seq 1 5`
  - **Output**:
    - `1`
    - `2`
    - `3`
    - `4`
    - `5`

### `shuf`

- **[man shuf](http://man.he.net/?topic=shuf&section=all)**
- **Common Flags**: `-n` (output count)
- **Use Case**: Randomly shuffle lines in a file or list.
- **Example**:
  - **Command**: `echo -e "1\n2\n3" | shuf`
  - **Output**: Random order of `1`, `2`, and `3`

### `sort`

- **[man sort](http://man.he.net/?topic=sort&section=all)**
- **Common Flags**: `-n` (numeric sort), `-r` (reverse), `-u` (unique)
- **Use Case**: Sort data in files.
- **Example**:
  - **Command**: `echo -e "3\n1\n2" | sort`
  - **Output**:
    - `1`
    - `2`
    - `3`

## Network Tools [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)

### `curl`

- **[man curl](http://man.he.net/?topic=curl&section=all)**
- **Common Flags**: `-o` (output file), `-L` (follow redirects), `-k` (ignore SSL errors), `-s` (silent), `-f` (fail quietly), `-H` (header), `-A` (user-agent), `-d` (data)
- **Use Case**: Downloading files or querying web services.
- **Example**:
  - **Command**: `curl -o example.html http://example.com`
  - **Output**: Downloads `example.com` into `example.html`.
  - **Command**: `curl -ksL http://example.com`
  - **Output**: Returns HTML content, following links, ignoring certificate errors, no progress bar.

### `wget`

- **[man wget](http://man.he.net/?topic=wget&section=all)**
- **Common Flags**: `-r` (recursive), `-O` (output document)
- **Use Case**: Downloading files from the internet.
- **Example**:
  - **Command**: `wget -O example.html http://example.com`
  - **Output**: Downloads `example.com` into `example.html`

## Search and Locate Commands [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)

### `man`

- **[man man](http://man.he.net/?topic=man&section=all)**
- **Use Case**: Accessing manual pages for other commands.
- **Example**:
  - **Command**: `man ls`
  - **Output**: Displays the manual page for the `ls` command.

### `which`

- **[man which](http://man.he.net/?topic=which&section=all)**
- **Use Case**: Finding the full path of shell commands.
- **Example**:
  - **Command**: `which ls`
  - **Output**: `/bin/ls`

### `locate`

- **[man locate](http://man.he.net/?topic=locate&section=all)**
- **Use Case**: Quickly searching for files by name, using an indexed database.
- **Example**:
  - **Command**: `locate myfile.txt`
  - **Output**: Lists paths containing `myfile.txt`

### `find`

- **[man find](http://man.he.net/?topic=find&section=all)**
- **Common Flags**: `-name` (file name), `-iname` (file case insensitive name), `-type` (type of file directory(d)/file(f)), `-or` (used when searching for multiples), `-exec` (execute command after finding file), `-and` (used when searching for multiples)
- **Use Case**: Finding files and performing actions on them.
- **Example**:
  - **Command**: `find /home -name "example.txt"`
  - **Output**: `/home/user/example.txt`
  - **Command**: `find /home -iname "example.txt"`
  - **Output**: `/home/user/Example.txt`
  - **Command**: `find /home -type d "ec2-user"`
  - **Output**: `/home/user/ec2-user`

## Advanced Text Searching [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)

### `fzf`

- **[man fzf](http://man.he.net/?topic=fzf&section=all)**
- **Use Case**: Search through complex lists and command histories quickly.
- **Example**:
  - **Command**: `echo -e "apple\nbanana\ncarrot" | fzf`
  - **Output**: Interactive search interface for list selection.

## Compressed File Viewing [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)

### `zgrep`

- **[man zgrep](http://man.he.net/?topic=zgrep&section=all)**
- **Use Case**: Searching inside compressed files without explicitly decompressing.
- **Example**:
  - **Command**: `zgrep "pattern" file.gz`
  - **Output**: Lines matching "pattern" in `file.gz`

### `zcat`

- **[man zcat](http://man.he.net/?topic=zcat&section=all)**
- **Use Case**: Viewing the contents of gzipped files without decompressing.
- **Example**:
  - **Command**: `zcat file.gz`
  - **Output**: Displays the contents of `file.gz`

## Editors [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)

### `vi`

- **[man vi](http://man.he.net/?topic=vi&section=all)**
- **Use Case**: Editing text files, programming, script editing, no quitting, no escape.
- **Example**:
  - **Command**: `vi file.txt`
  - **Output**: Opens `file.txt` in the `vi` editor.

### `nano`

- **[man nano](http://man.he.net/?topic=nano&section=all)**
- **Use Case**: Editing text files, programming, script editing, intuitive.
- **Example**:
  - **Command**: `nano file.txt`
  - **Output**: Opens `file.txt` in the `nano` editor.


## Helpers [Back to Index](https://github.com/mwilco03/BashBuddies/edit/main/README.md#index)

### `nohup`

- **[man nohup](http://man.he.net/?topic=nohup&section=all)**
- **Use Case**: Long running command will output to `nohup.out` can continue working in that shell.
- **Examples**:
  - **Command**: `nohup echo "hello world"`
  - **Output**: `nohup: ignoring input and appending output to 'nohup.out'`
  - **Command**: `nohup echo "hello world" &`
  - **Output**: `nohup: ignoring input and appending output to 'nohup.out'`

### `tmux`

- **[man tmux](http://man.he.net/?topic=tmux&section=all)**
- **Use Case**: Long running command will output to a different terminal can split screen.
- **Example**:
  - **Command**: `tmux`
  - **Output**: Allows to split screen. (Browser functionality limited)

### `fg`

- **[man fg](http://man.he.net/?topic=fg&section=all)**
- **Use Case**: Returns a backgrounded process to foreground.
- **Example**:
  - **Command**: `fg`
  - **Output**: Allows all backgrounded processes to be presented.

### `nmap`

- **[man nmap](http://man.he.net/?topic=nmap&section=all)**
- **[nmap handout](https://github.com/mwilco03/BashBuddies/blob/main/handouts/nmap_handout.md)**
- **Use Case**: Network mapping tool.
- **Example**:
  - **Command**: `nmap 10.2.72.0/25`
  - **Output**: Returns information about the open ports on the specified net.

### `jq`

- **[man nmap](http://man.he.net/?topic=jq&section=all)**
- **Use Case**: JSON object parsing tool.
- **Example**:
  - **Command**: `cat file.json|jq .keys`
  - **Output**: Returns information from json keys.
