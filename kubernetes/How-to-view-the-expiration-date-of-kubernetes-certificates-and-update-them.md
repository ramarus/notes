### How to check the validity of kubernetes certificates
The validity of kubernetes certificates is limited to 1 year. Run the following command to view the expiration date:
```console
# echo | openssl s_client -servername <server> -connect <server>:6443 2>/dev/null | openssl x509 -noout -dates
notBefore=Mar  4 05:52:31 2021 GMT
notAfter=Mar  4 08:52:33 2022 GMT
```
or
```console
# openssl s_client -connect localhost:6443 -showcerts < /dev/null 2>&1 | openssl x509 -noout -enddate
notAfter=Mar  1 10:29:02 2023 GMT
```

### How to update kubernetes certificates
#### If you use k3s
When using k3s, everything is simple. K3s automatically updates certificates on restart if there are less than 90 days left before the expiration date.

Just restart the k3s service:
```console
# systemctl restart k3s
```
or reboot server :)

But!
Some versions of K3s (ex. 1.18) does not clear the cached certificate. Therefore, the cache needs to be cleared manually.
For resolving the problem take a steps:
1. Backup the TLS dir
```console
# tar -czvf /var/lib/rancher/k3s/server/apphost-cert.tar.gz /var/lib/rancher/k3s/server/tls
```
2. Remove the cached certificates and cert-file:
```console
# rm /var/lib/rancher/k3s/server/tls/dynamic-cert.json
# kubectl --insecure-skip-tls-verify=true delete secret -n kube-system k3s-serving
```
3. Restart K3s to rotate certs:
```console
# systemctl restart k3s
```
Then check the K3s certificates:
```console
# for i in `ls /var/lib/rancher/k3s/server/tls/*.crt`; do echo $i; openssl x509 -enddate -noout -in $i; done
```

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