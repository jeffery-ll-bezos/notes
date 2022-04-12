# Attack Process

## Initial Enumeration

Nmap: 

```bash
sudo nmap -sC -sV -n $TARGET; sudo nmap -sU --max-retries 1 -v -n $TARGET;
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
- 

## Solidify and Escalate From Shell

- whoami
- If SSH is available, place your public key in ~/.ssh/authorized_keys
- sudo -l
