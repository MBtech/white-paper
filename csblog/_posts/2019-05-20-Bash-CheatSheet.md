---
layout: post
title: "Bash Cheatsheet"
categories:
  - guide
  - csblog
tags:
  - guide
---
Updated: 09/12/2019
Here a set of Bash one-liners that I have used in past or I regularly use:

**Starting ssh-agent in the background:**

```bash
eval "$(ssh-agent -s)"
```

**Adding ssh key to ssh-agent:**

```bash
ssh-add -K <ssh private key>
```

**Process using a particular port**
```bash
sudo netstat -pna | grep
```
**Run shell script when a file or directory changes:**

Install entr using `apt install entr`

**For changes to file:**
```bash
ls -d *.c | entr sh -c 'make && make test'
```

**For changes to directories:**

```bash
while true; do find path/ | entr -d echo Changed; sleep 10; done
```
or
```bash
while true; do ls path/* | entr -pd echo Changed; sleep 10; done
```

**Generting public key from private key**
```bash
ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub
```

**Starting a remote ssh command and shifting it to background and getting the PID of the process:**
```bash
ssh -n me@example.com "nohup myscript.sh >/dev/null 2>&1 &"
SERV_PID=$(ssh me@example.com 'echo $!')
```

**Adding numbers to the beginning of every line:**
```bash
awk '{printf("%010d %s\n", NR, $0)}' example.txt
```

**Local and remote variable inside an SSH command:**
```bash
A=3;
ssh host@name "B=3; echo $A; echo \$B;"
```

**Remove the first line of a file:**
```bash
tail -n +2 "$FILE"
```

**Increment a variable:**
```bash
var=$((var+1))
((var=var+1))
((var+=1))
((var++))
```

**Splitting string by the first occurrence of a delimiter:**
```bash
id="$( cut -d ';' -f 1 <<< "$s" )";
```

**Splitting string into an array:**
```Bash
IFS=',' read -r -a array <<< "$string"
for element in "${array[@]}"
do
    echo "$element"
done
```

**Disable password authentication:**

Add the following to `/etc/ssh/sshd_config`
```bash
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```
Then restart ssh using `sudo systemctl restart ssh`

**Changing java version:**

```bash
sudo update-alternatives --config java
```

**Reporting all devices connected in a network**

You need to provide the subnet mask IP as follows

`nmap -sP 192.168.100.0/24`

**Send POST Request with data from a file via curl**
```bash
curl -i -X POST host:port/post-file -H "Content-Type: text/xml" --data-binary "@path/to/file"
```
`@` tells curl to read from the file

**Send POST Request form with file via curl**
```bash
curl -X POST -i -F parametername=@filename host:port/post-file
```

**Measuring time**

One way to measure time is to simply use the `time` utility
```bash
time yourscript.sh
```
Or you can do this if `time` isn't an option
```bash
start=`date +%s`
stuff
end=`date +%s`

runtime=$((end-start))
```

**Source bash variables from a script and have global effect**

If you run it using `bash myscript.sh` or `./script.sh` then a subshell is created where the environment variables are imported and as soon as the script finishes, the subshell and the side-effects vanish too. For more information check this [stackoverflow question](https://stackoverflow.com/questions/3274397/reload-profile-in-bash-shell-script-in-unix)
You have to use this instead:

`.myscript.sh`

**Suppress the output of a command**

You can use the shell redirection to suppress output and redirect it to /dev/null
```bash
your_command >/dev/null 2>&1
```

**Bash script within a script**

The technical terminology for this is [HereDoc](https://linuxhint.com/bash-heredoc-tutorial/). It is a block of code or text which can be redirected to command script and interactive program.

If you want to avoid variable substitution you have to use single quotes or backslash before the delimiter. Otherwise you don't have to. I found that information in these answers ([1](https://stackoverflow.com/questions/22697688/how-to-cat-eof-a-file-containing-code), [2](https://stackoverflow.com/questions/39563955/how-create-a-bash-script-with-another-bash-script))
```bash
cat > script.sh  <<'EOF'
#Your script code
EOF
```

**Self deletion**
Adding a command to make the script delete itself ([SO Answer](https://stackoverflow.com/questions/8981164/self-deleting-shell-script))

```bash
rm -- "$0"
```

**Forcing prompt to always ask for sudo password**
```bash
sudo -k ...
```

**Summarized size of hidden directories**
```bash
du -hs .[^.]*
```

## awk one-liners
Specifying the output field separator
```
cat file | awk -v OFS='\t' '{print $5, $1}'
```

## Useful CLI tools
### TC tool for traffic control:
TC can only limit egress bandwidth (and do a bunch of other things like delays, packet loss and packet corruption). We can use the following command to limit egress bandwidth
```bash
tc qdisc add dev <interface> root tbf rate 1mbit latency 1us
```

You can remove the above limit using:
```bash
tc qdisc remove dev <interface> root tbf rate 1mbit latency 1us
```

### IPERF for testing network performance:
On server side use `iperf -s`

On client side use `iperf -c <serverip>`

**References:**
- [Splitting String into Array in Bash](https://stackoverflow.com/questions/10586153/split-string-into-an-array-in-bash)
- https://stackoverflow.com/questions/13200965/bash-start-remote-python-application-through-ssh-and-get-its-pid

- https://stackoverflow.com/questions/8206370/add-numbers-to-the-beginning-of-every-line-in-a-file

- https://stackoverflow.com/questions/13826149/how-have-both-local-and-remote-variable-inside-an-ssh-command
