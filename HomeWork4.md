# HomeWork4

**Calculate**

```
#!/bin/bash

if [ $# -gt 1 ]

then
	let " result1 = $(cat $1) "
	let " result2 = $(cat $2) "

	if [ $result1 -ge $result2 ]
	then
		echo $(cat $1) \> $(cat $2)
		echo $result1 \> $result2
	else
		echo $(cat $1) \< $(cat $2)
		echo $result1 \< $result2
	fi
else
	echo "Недостаточно параметров"
fi
```