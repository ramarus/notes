### How to check the validity of kubernetes certificates
The validity of kubernetes certificates is limited to 1 year. Run the following command to view the expiration date:
```console
# echo | openssl s_client -servername <server> -connect <server>:6443 2>/dev/null | openssl x509 -noout -dates
notBefore=Mar  4 05:52:31 2021 GMT
notAfter=Mar  4 08:52:33 2022 GMT
```

### How to update kubernetes certificates
#### If you use k3s
When using k3s, everything is simple. K3s automatically updates certificates on restart if there are less than 90 days left before the expiration date.

Just restart the k3s service:
```console
# systemctl restart k3s
```
or reboot server :)

#### If you use k8s
// In progress...

#### If you missed the certificate renewal deadline...
...simply disable system time synchronization with NTP servers and set the date a few days earlier)))

For example:
```console
# systemctl stop openntpd
# timedatectl set-time "yyyy-mm-dd hh:mm:ss"
# systemctl restart k3s
# systemctl start openntpd
# ntpd -s -d
```