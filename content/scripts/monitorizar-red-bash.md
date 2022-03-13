---
title: "Monitorizar Red Bash"
date: 2021-12-08T19:21:31+01:00
lastmod: 2021-12-08T19:21:31+01:00
publishdate: 2021-12-08T19:21:31+01:00
description: ""
weight: 10
---

```bash
#!/bin/bash
LOG=/tmp/mylog.log SECONDS=3600
EMAIL=mi@email 
for i in $@ ;do
echo "$i-UP!" > $LOG.$i 
done
while true; do 
    for i in $@; do
    ping -c 1 $i > /dev/null 
    if [ $? -ne 0 ]; then
        STATUS=$(cat $LOG.$i)
if [ $STATUS != "$i-DOWN!" ];
then
     echo "`date`: ping failed, $i
host is down!" |
mail -s "$i host is down!" $EMAIL
fi
else
STATUS=$(cat $LOG.$i)
if [ $STATUS != "$i-UP!" ]; then
     echo "`date`: ping OK, $i host
is up!" |
mail -s "$i host is up!" $EMAIL
fi
echo "$i-UP!" > $LOG.$i
fi done
sleep $SECONDS done
```