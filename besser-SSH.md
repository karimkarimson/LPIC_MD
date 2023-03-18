# SSH Vortrag vom CLT
### Key-Verarbeitung
> `ssh-keygen`
> > `ssh-keygen -t ed25519`

> > ed25519

> > `ssh-keygen -l -f test.pub`

> > create/show Fingerprint (why fingerprint?)

> > `ssh-keygen -A`

> > create host keys

> `ssh-keyscan`
> > `ssh-keyscan localhost`

> > zeigt alle vorhandenen Keys an, kann auch über IP-Adressbereiche ausgelöst werde

### Key-Handling in Besser
> `ssh-agent`
> > Passiert eigentlich automatisch
> `ssh-add`

### Key Benutzung
> `ssh/sftp`
> `sshd/sftpd`

### Dokumentation
> `man (5) ssh_config`
> `sshd_config`
> `man ssh`

### SSH-Daemon
> `sudo /usr/sbin/sshd -p 1027 -f /etc/ssh/sshd_config -d`

### Debugging
> `ssh -v ab@login.XYZ`
> > erste Hilfe **-v**
> `ssh -G XYZ`
> > **-G** gibt komplette Konfiguration nach dem parsen der Config aus, wenn eine Verbindung
> > aufgebaut worden wäre.
> > > `ssh -G XYZ | egrep -i '(hostname|jump)'`
_for each parameter, the first obtained value will be used_

### Public Keys & Umgebung
Key besteht aus 3-Teilen:
> + Keytype
> + base64-encoded key *fangen alle gleich an, da Keytype enthalten*
> + comment
`~/.ssh/authorized_keys`
> enthält akzeptierte keys (wer darf sich einloggen)
> jeder Wert besteht aus:
> > + (option)
> > + Keytype
> > + base64encoded key
> > + comment
> kann zum Beispiel vorgeben wie lange man warten (sleep) muss bis man etwas machen kann
> oder, dass man einen Tunnel aufbauen kann(?)

### SSH config
`~/.ssh/config`
> Kann man geschickt nutzen um zu Automatisieren, Stichwort Key-Rollover
> > zB:
> > > `Host`
> > > > `UpdateHostKeys          yes`

### Remote Execution
`ssh root@beispiel -t --systemctl restart stunnel4`
**-t** macht es interaktiv

### Portforwarding
In der Config unter Host XYZ LocalForward einstellen dann,
`ssh -v there`
> #### Portforwarding per Remote
> In der config, unter Host XYZ RemoteForward einstellen
> ### GPG-Agent Forwarding
> `cat /etc/ssh/sshd_config
> `StreamLocalBindUnlink   yes`
> `cat ~/.ssh/config`
> `Host   XYZ`
> >   `Hostname XYZ.at.home`
> >   `RemoteForward /run/user/1234/gnupg/S.gpg-agent /run/user/1234/gnupg/S.gpg-agent.extra`
> `ssh    XYZ`
> `export GPG_TTY$(tty)`
> `git tag --sign ...`

### Jump Host
Um automatisch über mehrere Hosts sich zu verbinden, so lassen sich Verbindungen über mehrere Sub-Netze und Sub-Sub-Netze einfacher herstellen lassen (weniger Tippen)

### Talk to your SSH
wenn verbunden, dann automatisch über folgende Befehle
> + `<enter>~.`    - terminate session
> + `<enter>~?`    - more commands
> + `<enter>~#`    - list forwarded connections

### SSH-Reagent
Keys
> `ssh XYZ -t --screen`
> > `ssh XYZ -t --screen -x`

### SSH & Pipes
`ssh XYZ --dd if=/dev/sda bs=512 count=1 | dd of=mbr-backup`