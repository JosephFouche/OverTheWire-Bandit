# OverTheWire-Bandit
Bandit 15
openssl s_client -connect localhost:30001 -quiet 2>/dev/null < /etc/bandit_pass/bandit15

Let me break it down word by word:
openssl s_clientthe program that can make SSL/TLS connections (like a secure telnet)

-connect localhost:30001connect to port 30001 on the same machine

-quietdon’t print all the SSL debug garbage

2>/dev/nullthrow away any remaining error/warning messages

< /etc/bandit_pass/bandit15THIS IS THE KEY: take the content of the password file and feed it directly as input to openssl (including the newline at the end)




