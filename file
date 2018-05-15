
#!/bin/bash -       
#title           	:server_info.sh
#description     	:This script create from bash, info_server.sh for using
#author		 		:BoniW | SysAdmin | DevOps
#date            	:20171212
#==============================================================================




# Get hostname
hostname=`hostname` 2> /dev/null
 
# Get distro
if [ -f "/etc/system-release" ]; then
    distro=`cat /etc/system-release`
else
    distro=`python -c 'import platform; print platform.linux_distribution()[0] + " " + platform.linux_distribution()[1]'` 2> /dev/null
fi
 
# Get uptime
if [ -f "/proc/uptime" ]; then
    uptime=`cat /proc/uptime`
    uptime=${uptime%%.*}
    seconds=$(( uptime%60 ))
    minutes=$(( uptime/60%60 ))
    hours=$(( uptime/60/60%24 ))
    days=$(( uptime/60/60/24 ))
    uptime="$days"d", $hours"h", $minutes"m", $seconds"s""
else
    uptime=""
fi
 
# Get cpus
if [ -f "/proc/cpuinfo" ]; then
    cpus=`grep -c processor /proc/cpuinfo` 2> /dev/null
else
    cpus=""
fi
 
# Get load averages
loadavg=`uptime | awk -F'load average:' '{ print $2 }'` 2> /dev/null
 
# Remove leading whitespace from load averages
loadavg=`echo $loadavg | sed 's/^ *//g'`
 
# Get total memory
if [ -f "/proc/meminfo" ]; then
    memory=`cat /proc/meminfo | grep 'MemTotal:' | awk {'print $2}'` 2> /dev/null
else
    memory=""
fi
 
# Get ip addresses
ips=`ifconfig | awk -F "[: ]+" '/inet addr:/ { if ($4 != "127.0.0.1") print $4 }'` 2> /dev/null
 
# ips is empty, let's try and get ip addresses with python instead
if [ -z "${ips}" ]; then
    ips=`python -c 'import socket; print socket.gethostbyname(socket.gethostname())'` 2> /dev/null
fi
 
echo -n '{"hostname": "'$hostname'", "distro": "'$distro'", "uptime": "'$uptime'", "cpus": '$cpus', "loadavg": "'$loadavg'", "memory": '$memory', "ips": "'$ips'"}'
