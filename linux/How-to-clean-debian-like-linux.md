### If you want to delete old kernels, except for the current and previous one

##### Run the command:
```console
$ sudo apt-get purge $(dpkg -l 'linux-*' | sed '/^ii/!d;/'"$(uname -r | sed "s/\(.*\)-\([^0-9]\+\)/\1/")"'/d;s/^[^ ]* [^ ]* \([^ ]*\).*/\1/;/[0-9]/!d' | head -n -1)
```
then
```console
$ sudo apt-get autoremove
```

##### Also use next commands:
```console
$ sudo apt-get autoclean
$ sudo apt-get autoremove
```

### If you want to remove old software packages

##### Run
```console
$ dpkg -l | awk '/^rc/ {print $2}' | xargs sudo dpkg --purge
```
