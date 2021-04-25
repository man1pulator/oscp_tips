# Oscp Unique and Cool Preparation Tips

### 1. Tmux - Settings Variables to Use across Tmux Session

Tmux is a terminal multiplexer. It lets you switch easily between several programs in one terminal.
Its highly recommended to use tmux for your daily life task and keeping your work organized.

Talking about CTFs machine, we usually came across with different machines having different IP. Many feel lazy to remember the exact IP and usually writes or copy/paste the machine’s IP again and again to launch different scans. 

You can set variables in tmux session that will be accessed throughout the given tmux session. Make sure to declare variables in the first pane so the child panes, created afterwards, can use declared variables. Kindly refer to the *video* below
<br/>

**Syntax**
```
IP=10.0.2.33
tmux setenv IP $IP
export IP $IP	
```

**Video POC**

https://user-images.githubusercontent.com/82765761/115953112-fe4e6400-a502-11eb-8a70-2c1312b44fd5.mp4

<br/>
<br/>

## 2 Analyzing Traffic with Tcpdump

Tcpdump is a valuable command line utility that allows you to capture and analyze network traffic going through your system. I am going to suggest you an approach that would help you debug and understand network protocols.

I would highly recommend to use this tcpdump command to analyze or debug the traffic. I frequently use this and avoided many stupid mistakes for e.g providing wrong port number or tool not working as expected.

```
tcpdump -i tun0 -nqtX host $IP and port 21 and 'tcp[13] & 8!=0'
```

**Breakdown**

`-n`      Don't convert addresses (i.e., host addresses, port numbers, etc.) to names.

`-q`     Quick (quiet?) output.  Print less protocol information so output lines are shorter.

`-t`     Don't print a timestamp on each dump line.

`-X`     Print the data of each packet in hex and ASCII.  This is very handy for analysing new protocols.


`'tcp[13] & 8 != 0'`     Show all PSH Packets (Packets containing actual data)
<br/>

**Video POC**


https://user-images.githubusercontent.com/82765761/115953147-1cb45f80-a503-11eb-9425-f5cb906f40a0.mp4


<br/>
<br/>

## 3. Dirsearch 

Dirsearch is a mature command-line tool designed to brute force directories and files in webservers.

I highly recommend to use dirsearch for finding hidden directories and files. The key features of dirsearch includes pausing and resuming scan, providing multiple files as input and performing recursive brute forcing. Below is the command that has never let me down throughout my OSCP journey.

**Syntax**
```
wordlists='db/dicc.txt,/usr/share/dirb/wordlists/common.txt,/usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt'
python3 dirsearch.py -u $url -e php,html,htm,txt -w $wordlists --random-user-agent -b -t 20 
```

**Note**: dirsearch doesn't append extensions to every word, if you don't use the -f flag. By default, only the %EXT% keyword in the wordlist will be replaced with extensions (-e <extensions>).

<br/>
<br/>

## 4. Bruteforcing with ffuf

Since Burp suite professional isn’t allowed in OSCP exam and using hydra for brute forcing web forms usually takes a bit headache to configure. We can use ffuf raw request  to brute force http web forms with minimum configurations.

**Syntax**
```
ffuf --request raw.req --request-proto http -w /tmp/user.list:W1,/tmp/password.list:W2 -c -ac -t 20
```

**Video POC**

https://user-images.githubusercontent.com/82765761/115960695-55b4fa00-a52c-11eb-9a55-6fddb06025d7.mp4
