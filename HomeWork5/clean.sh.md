# HomeWork5

**Скрипт для чистки логов**
```
#!/bin/bash

var=$(df / | awk '{if (NR!=1) {print $5}}' | sed 's/%//g')
set info=""
# echo $var
freeplace=$(( 100 - $var ))
if (( $freeplace < 60 ))
then
	sudo dd if=/dev/null of=/var/log/syslog
	if (( $? == 0 ))
	then
		info="$info Syslog cleared.";echo "Syslog cleared."
	else
		info="$info Syslog not cleared.";echo "Syslog not cleared"
	fi

	sudo rm /var/log/syslog.*
	if (( $? == 0 ))
	then
		info="$info Syslog's files deleted.";echo "Syslog's files deleted"
	else
		info="$info Syslog's files not deleted.";echo "Syslog's files not deleted"
	fi

	sudo apt-get clean
	if (( $? == 0 ))
	then
		info="$info Apt cache cleaned.";echo "Apt cache cleaned"
	else
		info="$info Apt cache not cleaned.";echo "Apt cache not cleaned"
	fi

fi
echo "$freeplace% свободного места в корневой директории /. $info [$(date)]" >> /var/log/clean_sh.log
```
