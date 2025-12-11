# OverTheWire-Bandit
Bandit 15
openssl s_client -connect localhost:30001 -quiet 2>/dev/null < /etc/bandit_pass/bandit15

Let me break it down word by word:
openssl s_clientthe program that can make SSL/TLS connections (like a secure telnet)

-connect localhost:30001connect to port 30001 on the same machine

-quietdon’t print all the SSL debug garbage

2>/dev/nullthrow away any remaining error/warning messages

< /etc/bandit_pass/bandit15THIS IS THE KEY: take the content of the password file and feed it directly as input to openssl (including the newline at the end)


--Bandit 16
1. Find which ports in 31000-32000 are actually listening
nmap -p 31000-32000 localhost -reason --open
Output (yours showed 5 open ports): 31046, 31518, 31691, 31790, 31960.

2. Loop through ports to send the Bandit 16 password and check responses.text
for p in 31046 31518 31691 31790 31960; do 
  echo -e "\n=== Testing port $p ==="; 
  openssl s_client -connect localhost:$p -quiet 2>/dev/null < /etc/bandit_pass/bandit16 | head -10
done

3. Retrieve the full private key from the correct port:
openssl s_client -connect localhost:31790 -quiet < /etc/bandit_pass/bandit16 > /tmp/key17 2>/dev/null

4. set permissions. chmod 600 /tmp/key17

5. Log out of Bandit 16 and connect to Bandit 17 from your local machine:
cat /tmp/key17

mkdir -p ~/.ssh
cat > ~/.ssh/bandit17_key << 'EOF'
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
EOF

chmod 600 ~/.ssh/bandit17_key


6. Connect. 
ssh -i ~/.ssh/bandit17_key bandit17@bandit.labs.overthewire.org -p 2220

This uses public-key authentication (no password needed).



# Bandit 17
 para conseguir pass comparar 2 archivos de texto identicos excepto una linea cambiada

se usa comando diff entre password.new y passwords.old

# Bandit 18
The password for the next level is stored in a file readme in the homedirectory.
 Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme

Al poner cat readme al final del comando ssh, le estamos diciendo:
“Conéctate con SSH, autentícate, pero NO abras un shell interactivo. En su lugar, ejecuta directamente el comando cat readme y envíame la salida por la conexión SSH.”
Esto se llama ejecución remota de un comando no interactivo.
Consecuencias:

SSH no considera que es un “login interactivo”.
Por lo tanto, NO se cargan .bashrc, .bash_profile, ni ninguno de los archivos de inicio del shell.

# Bandit 19

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

rwsr-x---  1 root bandit19 7296 Apr 23 2018 bandit20-do
   ↑
   esta "s" es el bit setuid

Bit setuid = activado → el programa corre como root, no como tú.

¿Qué hace realmente el programa bandit20-do?
No es un programa mágico que te da root completo. Es un programa muy pequeño y controlado que hace lo siguiente:

Mira quién lo está ejecutando (tú, bandit19).
Dice: “Ok, como soy setuid root, tengo poder de root… pero solo voy a usar ese poder para cambiar temporalmente al usuario bandit20 y ejecutar el comando que me pidas”.

En otras palabras: te permite ejecutar un solo comando como si fueras bandit20.


Entonces usamos el binario setuid para ejecutar cat como bandit20:

./bandit20-do cat /etc/bandit_pass/bandit20

# Bandit 20
There is a setuid binary in the homedirectory that does the following: it makes a connection to
 localhost on the port you specify as a commandline argument.
 It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think


nc -lvkp 50000 <<< "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" & sleep 0.5; ./suconnect 50000

0.000s → nc empieza a escuchar en el puerto 50000
      → ya tiene escrita la contraseña de bandit20 lista para enviar
0.500s → sleep termina
      → se ejecuta ./suconnect 50000
      → suconnect se conecta al puerto
      → lee la línea que le envía nc → comprueba que es correcta
      → escribe la contraseña de bandit21 en la conexión
      → nc la recibe y la imprime en pantalla

Tú abres el canal → dejas el mensaje listo → das un poquito de tiempo → llamas al robot → el robot te da el premio.

# Bandit 21
Level 21 → Level 22 Level Goal A program is running automatically at regular intervals from cron,
 the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

Primero, mira los archivos cron del sistema:

ls -l /etc/cron.d/


Vas a ver varios archivos, incluyendo uno llamado algo como:

cronjob_bandit22


Luego, simplemente ábrelo:

cat /etc/cron.d/cronjob_bandit22


Ese archivo te mostrará qué script se ejecuta, y normalmente apunta a algo como:

/usr/bin/cronjob_bandit22.sh


Entonces inspeccionás el script:

tRae0UfB9v0UzbCdn9cY0gQnds9GF58Qcat /usr/bin/cronjob_bandit22.sh
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
 #!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

# Bandit 22

A program is running automatically at regular intervals from cron, the time-based job scheduler.
 Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill.
 The script for this level is intentionally made easy to read. If you are having problems understanding what it does,
 try executing it to see the debug information it prints.

bandit22@bandit:~$ cd /etc/cron.d
bandit22@bandit:/etc/cron.d$ ls
behemoth4_cleanup  clean_tmp  cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  leviathan5_cleanup  manpage3_resetpw_job  otw-tmp-dir  sysstat
bandit22@bandit:/etc/cron.d$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
 #!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget

Cron copia la contraseña de bandit23 dentro de un archivo en /tmp, cuyo nombre es un hash md5 basado en el texto:

I am user bandit23


Entonces vos solo tenés que generar ese md5 igual que el script.

Hacelo directamente desde bandit22:

echo -n "I am user bandit23" | md5sum | cut -d ' ' -f1

bandit22@bandit:/$ echo -n "I am user bandit23" | md5sum | cut -d ' ' -f1
7dfc5d0348e965fba8b56a01c1508c98


# Bandit 23

Bandit Level 22 → Level 23
Level Goal
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

Look in /etc/cron.d/ for the relevant job file:

cat /etc/cron.d/cronjob_bandit23

Examine the script that's being executed:
cat /usr/bin/cronjob_bandit23.sh

 #!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
cat /etc/bandit_pass/$myname > /tmp/$mytarget

The script runs as bandit23, so myname becomes bandit23.
It computes the MD5 hash of the string "I am user bandit23", takes the first field (the hash itself), and uses that as a filename in /tmp/.
Then it copies the bandit23 password to that file.

Compute the filename yourself (as bandit22):

echo "I am user bandit23" | md5sum | cut -d ' ' -f 1

The cronjob runs every minute and creates/overwrites the file /tmp/8ca319486bfbbc3663ea0fbe81326349 with the bandit23 password.
Read the password:

cat /tmp/8ca319486bfbbc3663ea0fbe81326349

