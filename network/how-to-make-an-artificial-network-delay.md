#### If you need to make an artificial network delay (ex. for tests):

##### Create a script with a set of rules
```console
$ mkdir scripts
$ cd scripts
$ nano latency.sh
```

##### Copy and edit next text in script
```shell script
#!/bin/bash
echo
echo Please run script with root access: sudo ./latency.sh
echo
echo Curent status:
tc qdisc show dev eth0
echo
echo "Please make a choice:"
OPTIONS="Clean_Value Latency-200ms Latency-500ms Latency-1000ms Packet_Lost-20% Packet_Lost-30%
Latency-200ms_and_Packet_Lost-20% Clean_and_Exit"
select opt in $OPTIONS; do
if [ "$opt" = "Clean_Value" ]; then
tc qdisc del dev eth0 root
echo Cleared
elif [ "$opt" = "Latency-200ms" ]; then
#Remove fusion
tc qdisc del dev eth0 root
tc qdisc add dev eth0 root netem delay 200ms
echo Latency 200ms
tc qdisc show dev eth0
elif [ "$opt" = "Latency-500ms" ]; then
#Remove fusion
tc qdisc del dev eth0 root
tc qdisc add dev eth0 root netem delay 500ms
echo Latency 500ms
tc qdisc show dev eth0
elif [ "$opt" = "Latency-1000ms" ]; then
#Remove fusion
tc qdisc del dev eth0 root
tc qdisc add dev eth0 root netem delay 1000ms
echo Latency 1000ms
tc qdisc show dev eth0
elif [ "$opt" = "Packet_Lost-20%" ]; then
#Remove fusion
tc qdisc del dev eth0 root
tc qdisc add dev eth0 root netem loss 20%
echo Packet Lost 20%
tc qdisc show dev eth0
elif [ "$opt" = "Packet_Lost-30%" ]; then
#Remove fusion
tc qdisc del dev eth0 root
tc qdisc add dev eth0 root netem loss 30%
echo Packet Lost 30%
tc qdisc show dev eth0
elif [ "$opt" = "Latency-200ms_and_Packet_Lost-20%" ]; then
#Remove fusion
tc qdisc del dev eth0 root
tc qdisc add dev eth0 root netem delay 200ms 50ms loss 20%
echo Latency 200ms and Packets Lost 10%
tc qdisc show dev eth0
elif [ "$opt" = "Clean_and_Exit" ]; then
tc qdisc del dev eth0 root
echo Curent status:
tc qdisc show dev eth0
exit
else
clear
echo bad option
fi
done
```
You need to edit your network interface if it isn't **eth0**!

Also you can edit menu and options fo your tasks
##### Make the script executable
```console
$ chmod +x latency.sh
```
##### Run the script (with Root-privileges)
```console
$ sudo ./latency.sh
```

\\\ This script was tested on Ubuntu 20.04