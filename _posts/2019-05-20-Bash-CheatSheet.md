---
layout: post
title: "Bash Cheatsheet"
categories:
  - guide
tags:
  - guide
---
Here a set of Bash one-liners that I have used in past

**Starting ssh-agent in the background:**

`$ eval "$(ssh-agent -s)"`

**Adding ssh key to ssh-agent:**

`$ ssh-add -K <ssh private key>`

**Run shell script when a file or directory changes:**

Install entr using `apt install entr`

**For changes to file:**
```Bash
ls -d *.c | entr sh -c 'make && make test'
```

**For changes to directories:**

```Bash
while true; do find path/ | entr -d echo Changed; sleep 10; done
```
or
```Bash
while true; do ls path/* | entr -pd echo Changed; sleep 10; done
```

**Starting a remote ssh command and shifting it to background and getting the PID of the process:**
```Bash
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

**Disable password authentication:**

Add the following to `/etc/ssh/sshd_config`
```bash
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```
Then restart ssh using `sudo systemctl restart ssh`

**Changing java version:**

`sudo update-alternatives --config java`

**Reporting all devices connected in a network**
You need to provide the subnet mask IP as follows

`nmap -sP 192.168.100.0/24`

**Send POST Request with data from a file via curl**
```
curl -i -X POST host:port/post-file -H "Content-Type: text/xml" --data-binary "@path/to/file"
```
`@` tells curl to read from the file

**Send POST Request form with file via curl**
```
curl -X POST -i -F parametername=@filename host:port/post-file
```

**Measuring time**
One way to measure time is to simply use the `time` utility
```
time yourscript.sh
```
Or you can do this if `time` isn't an option
```
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
```
your_command >/dev/null 2>&1
```
## awk one-liners
Specifying the output field separator
```
cat file | awk -v OFS='\t' '{print $5, $1}'
```

## Useful CLI tools
### TC tool for traffic control:
TC can only limit egress bandwidth (and do a bunch of other things like delays, packet loss and packet corruption). We can use the following command to limit egress bandwidth
```Bash
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

- https://stackoverflow.com/questions/13200965/bash-start-remote-python-application-through-ssh-and-get-its-pid

- https://stackoverflow.com/questions/8206370/add-numbers-to-the-beginning-of-every-line-in-a-file

- https://stackoverflow.com/questions/13826149/how-have-both-local-and-remote-variable-inside-an-ssh-command
