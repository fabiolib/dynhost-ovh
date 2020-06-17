#!/bin/sh

##simple script originally authored by yjajkiew	at GitHub
##tweaked using suggestions from `shellcheck` tool - such as removal of legacy backtick notation

#
# CONFIG
#

HOST=DOMAIN_NAME
LOGIN=USER_NAME
PASSWORD=PASSWORD # be sure to properly escape any special characters in your password

PATH_LOG=/var/log/dynhost
CURRENT_DATE=$(date)

#
# GET IPs
#

HOST_IP=$(dig +short $HOST)
CURRENT_IP=$(curl -4 ifconfig.co)

#
# DO THE WORK
#
if [ -z "$CURRENT_IP" ] || [ -z "$HOST_IP" ]
then
        echo "No IP retrieved" >> $PATH_LOG
else
        if [ "$HOST_IP" != "$CURRENT_IP" ]
        then
                echo "$CURRENT_DATE"": Current IP:" "$CURRENT_IP" "and" "host IP:" "$HOST_IP" "   IP has changed!" >> $PATH_LOG
                RES=$(curl --user "$LOGIN:$PASSWORD" "https://www.ovh.com/nic/update?system=dyndns&hostname=$HOST&myip=$CURRENT_IP")
                echo "Result request dynHost:" >> $PATH_LOG
                echo "$RES" >> $PATH_LOG
        else
                echo "$CURRENT_DATE"": Current IP:" "$CURRENT_IP" "and" "Host IP:" "$HOST_IP" "   IP has not changed" >> $PATH_LOG
        fi
fi

