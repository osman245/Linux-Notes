# Essential Commands

## SSH & Remote Access

**Summary:** To access the console, you can either log in locally or remotely. To access the console remotely, you need a secure method so your password information is not breached. This must be done through SSH. SSH encrypts your password when you are logging into a remote console, so the password data does not travel in transit as plain text and is not vulnerable to eavesdropping and other cyber attacks.

**Notable Commands:**

- **CMD** — Open the console
- **ssh -V** — Get the version of the SSH program currently installed on the PC
- **ssh username@node** — SSH into a remote server
- **sudo -v ssh@alex** — Get verbose information on the command; used for troubleshooting

---

## System Documentation

**Summary:** We use system documentation to learn how to use commands. For instance, we can use the `--help` flag, and for more complex situations we can access the manual page using the `man` command. Use `man -k` to search for key terms in the manual page database, and `mandb` to update the manual page index. Use `sudo mandb` if you need to update the cache system-wide.

**Notable Commands:**

- **man -k / apropos** — Search for key terms
- **mandb** — Update/create cache of manual pages you have permissions to as a user
- **sudo mandb** — Update the cache of system-wide manual pages
- **sudo hostnamectl set-hostname web-server-01** — Change the hostname to web-server-01

---

## Files & Directories

**Summary:** In Linux, we can create, delete, copy, and move files and directories using various commands. Use `touch` or `vim` to create files, `mkdir` to create directories, `rm` to remove files, and `cp` to copy files. Use `mv` to rename files and directories.

**Notable Commands:**

- **ls -lah** — Show the extended listing (`l`), including hidden files (`a`), with human-readable sizes (`h`)
- **pwd** — Print the current directory
- **cd ~ / cd ../** — Change to the home directory / change to the directory one level up
- **mkdir lfcs** — Create a directory called `lfcs`
- **touch lfcs.txt** — Create a file called `lfcs.txt`
- **cp -ar /tmp/invoice /home/bob** — Recursively copy all child directories and the current directory from `/tmp/invoice` into `/home/bob/`, preserving its attributes
- **rm -rf /home/bob/lfcs** — Recursively remove directories and their child directories, forcing it to happen
- **mkdir -p /1/2/3/4** — Create a directory and all its parent directories if they do not exist
- **ls --full-time** — Get the last time a file was modified

---

## Hard Links & Soft Links

**Summary:** For data efficiency and reliability, we create hard links. For crossing filesystems and linking directories, we create soft links. When you create a file, that file points to a spot on the hard drive. When you create a hard link between two files, both files point to that same spot on the hard drive. When you create a soft link between two files, the second file is an alias to the original file.

**Good explanation on soft and hard links:** https://www.youtube.com/watch?v=lW_V8oFxQgA&t=18s

**Notable Commands:**

- **ln -s /tmp /home/bob/link_to_tmp** — Soft link `/tmp` to `/home/bob/link_to_tmp`, acting as an alias
- **ln /tmp /home/bob/link_to_tmp** — Hard link `/tmp` to `/home/bob/link_to_tmp`, both pointing to the same spot on the hard drive

---

## File Permissions

**Summary:** Every file has a set of permissions. To change the permissions of a file at the owner, group, and other level, use the `chmod` command. First pick the set of permissions you want to change (read, write, and/or execute), then pick the target to change (user, group, other). Read = 4, Write = 2, Execute = 1. The format is `000`: owner, group, other.

**Notable Commands:**

- **chmod 644 filename** — Give read/write access to the owner and read-only access to group members and others
- **chown -R alice:developers /path/to/directory** — Set a new owner and new group for a directory
- **chmod go+r filename** — Add read permissions to the file for group and others

---

## Special Permissions

**Summary:** We add an extra bit for SUID, SGID, and/or Sticky Bit for special permissions. **SUID** is a special permission that allows users to run an executable with the permissions of the executable's owner. **SGID** is a similar permission but applies to both executables and directories. **Sticky Bit** is a special permission that can be set on directories; it restricts file deletion within that directory.

**Notable Commands:**

- **chmod u+s,g+s,o+t /home/bob/datadir** — Set the SUID bit for the user, SGID bit for the group, and Sticky Bit for others

---

## Finding Files

**Summary:** When you want to find files and directories that match certain criteria — such as type, size, or permissions — use the `find` command.

**Notable Commands:**

- **find -name file1.txt** — Find a file named `file1.txt` in the current directory
- **find -mmin 5** — Find files modified exactly 5 minutes ago
- **find -mmin -5** — Find files modified less than 5 minutes ago
- **find -mmin +5** — Find files modified more than 5 minutes ago
- **find -name "f*" -size +512k** — Find files whose name starts with `f` and that are at least 512K in size
- **find -name "f*" -o -size 512k** — Find files whose name starts with `f` or that are 512K in size
- **sudo find /usr -type f -size +5M -size -10M > /home/bob/size.txt** — Find files greater than 5MB and less than 10MB
- **find . -perm 664** — Find files in the current directory with exactly 664 permissions
- **find -mtime 2** — Find files modified exactly 2 days ago (in the 2–3 days ago window, not within the last 2 days)

---

## Text Processing

**Summary:** The `head` and `tail` commands let you quickly view a portion of a file — `head` from the beginning, `tail` from the end. The `sed` command (stream editor) is used to find and replace text. The `cut` command reads each line and cuts out parts of it based on characters, fields (columns separated by a delimiter), or byte positions.

**Good video on sed:** https://www.youtube.com/watch?v=nXLnx8ncZyE

**Notable Commands:**

- **head -5 myfile** — Get the first 5 lines of a file
- **tail -n 10 /var/log/apt/term.log** — Show the last 10 lines of the file
- **head -n 20 /var/log/apt/term.log** — Show the first 20 lines of the file
- **sed 's/canda/canada/g' userinfo.txt** — Substitute "canda" with "canada" globally throughout the file
- **cut -d ' ' -f 1 userinfo.txt** — Use a space delimiter and return the first field from `userinfo.txt`
- **cut -d ',' -f 3 userinfo.txt > countries.txt** — Use a comma delimiter and return the third field, saving output to `countries.txt`
- **uniq countries.txt** — Remove repeating consecutive lines
- **diff -c file1 file2** — Show contextual differences between two files
- **diff -y file1 file2** — Show differences side by side

---

## Grep & Regex

**Summary:** Use `grep` to search for specific text and patterns within files.

**Notable Commands:**

- **grep -rwi 'password' /etc/ssh/sshd_config** — Find the keyword "password" in this directory recursively, with case-insensitive and whole-word matching

**Summary:** Basic regular expressions are the default regex type used by tools like `grep`. Regex makes it easier to filter content without needing extra flags.

- **grep -w '7' /etc/login.defs** — Find lines containing `7` as a whole word
- **grep -r 'c.t' /etc/** — Find patterns matching any single character between `c` and `t`
- **grep -r '/.*/' /etc/** — Find patterns that begin with `/`, have zero or more characters in between, and end with `/`
- **egrep -r '0+' /etc/** — Recursively find files in `/etc` with patterns containing one or more zeros
- **egrep -r '0{3,}' /etc/** — Show three or more consecutive zeros
- **egrep -r '/dev/[a-z]*' /etc/** — Show all files in `/etc` matching the pattern `/dev/` followed by zero or more lowercase letters
- **egrep -r '/[^a-z]' /etc/** — Find a `/` followed by a character that is NOT a lowercase letter

---

## Archiving & Compression

**Summary:** We can archive files into a single manageable container and compress them by removing redundant data using the `tar` command. We use `tar` to list the contents of an archive, create archives, and extract archives to the current directory. We can also compress tar archives and create zip archives.

- **tar -tf archive.tar** — List the contents of the archive
- **tar cf archive.tar file1** — Create a tar archive named `archive.tar` containing `file1`
- **tar -xf archive.tar --directory /tmp/** — Extract the contents of `archive.tar` into the `/tmp` directory
- **zip -r archive.zip Pictures/** — Create a ZIP archive of the entire `Pictures/` directory. Use `zip` over `tar` when you want to archive and compress in one step
- **unzip -l archive.zip** — List the contents of a zip archive
- **tar czf archive.tar.gz file1** — Bundle `file1` into an archive and compress it with gzip
- **tar cjf archive.tar.bz2 file1** — Create a bzip2-compressed tar archive containing `file1`
- **-P** — Preserve the absolute path when extracting
- **tar xf /home/bob/archive.tar.gz -C /tmp** — Change to `/tmp` directory before extracting
- **gzip file1** — Compress `file1` using GNU zip, producing `file1.gz`
- **bzip2 file2** — Compress `file2` using the bzip2 algorithm, replacing the original with `file2.bz2`
- **xz file3** — Compress `file3` using the xz (LZMA2) algorithm, replacing the original with `file3.xz`
- **gunzip file1.gz** (equivalent to `tar xzf archive.tar.gz`) — Decompress the gzip-compressed file and restore the original

---

## Remote File Transfer

**Summary:** Use `rsync` for file copying between remote and local consoles, or for disk imaging by specifying a partition device path.

- **rsync -a Pictures/ aaron@9.9.9.9:/home/aaron/Pictures/** — Recursively copy and synchronize the contents of the local `Pictures/` directory to the remote machine, preserving file attributes
- **rsync -a aaron@9.9.9.9:/home/aaron/Pictures/ .** — Sync a remote directory to the local console

---

## Redirection

**Summary:** We use redirection to control where data flows. We can redirect and append data into files. Standard output is redirected with `1>` and standard error with `2>`.

- **cat file.txt** — Print the contents of `file.txt`
- **sort file.txt** — Sort the file numerically or alphabetically
- **date > file.txt** — Write the current date into `file.txt`
- **stdin** — `<`
- **stdout** — `1>`
- **stderr** — `2>`
- **grep -r '^The' /etc/ 2>/dev/null** — Recursively search for lines starting with "The", suppressing error messages by sending them to `/dev/null` (the trash bin for data streams)

---

## SSL/TLS & OpenSSL

**Summary:** We use SSL/TLS to provide security and trust for data packets travelling over the internet.

- Run **man openssl** to see everything you can do with the `openssl` command
- **openssl req -newkey rsa:2048 -keyout key.pem -out req.pem** — Create a new 2048-bit RSA private key, save it to `key.pem`, and save the CSR (Certificate Signing Request) to `req.pem`
- **openssl req -x509 -noenc -newkey rsa:4096 -days 365** — Create a self-signed certificate (for development) instead of a default CSR, with a new 4096-bit private key, valid for 365 days

---

## Git & Version Control

**Summary:** We use version control software so a team can modify the same codebase without conflicts. The VCS we use is Git. In Git, you add the changes you want to track to the staging area. We create different branches so team members can work on the code simultaneously. We can commit locally and push code remotely to GitHub, which is the remote repository platform Git uses.

- **sudo apt install git** — Install Git
- **git config user.name "Warsame"** — Set your Git username
- **git config user.email "warsame@gmail.com"** — Set your Git email
- **cat .git/config** — See the current config showing which Git account the console is connected to
- **git init** — Initialize the local directory as a Git repository
- **git add** — Add new changes to the Git staging area
- **git reset file2** — Remove `file2` from the staging area
- **git branch 1.1-testing** — Create a branch called `1.1-testing`
- **git branch --delete branch** — Delete a branch
- **git branch --list** — List all branches
- **git status** — See what is staged and what still needs to be committed
- **git commit -m "changes"** — Commit and add a mandatory message describing your changes
- **git switch 1.1-testing** — Switch from the main branch to the `1.1-testing` branch
- **git checkout master** — Switch branches and restore working files
- **git merge 1.1-testing** — Merge the commits from `1.1-testing` into the current branch
- **git remote -v** — Show all remote repositories linked to your local Git repo and their URLs
- **git remote add origin git@github.com:warsame-osman/my-project.git** — Link the local repo to `my-project` on GitHub so you can push and pull via SSH
- **ssh-keygen** — Generate an SSH key pair to authenticate securely to services like GitHub without typing a password each time
- **git log** — Show the commit history of the repository

---

## Vim

**Summary:** Vim is a text editor where you alternate between **Normal**, **Insert**, and **Visual** modes to navigate and edit text.

**Notable Commands:**

- Press **i** (before cursor) or **a** (after cursor) to enter Insert mode; press **ESC** to return to Normal mode
- **:w** — Save the file
- **:q** — Quit
- **:wq** — Save and quit
- **dd** — Delete a line (Normal mode)
- **yy** — Yank (copy) a line
- **p** — Paste
- **u** — Undo
- **Ctrl + R** — Redo
- **/** — Search (follow with your search term)
- **v** — Enter Visual mode to select text for yanking, deleting, or bulk manipulation
