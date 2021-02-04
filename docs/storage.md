## Storage Usage Commands

| Syntax      | Description                       |
| ----------- | -----------                       |
| df -h       | mount file usage and availability |
| ncdu        | storage usage visualization       |

Sorted biggest 10 file in the current dir sub-system <br>
`du -ah . | sort -n -r | head -n 10`

Sorted biggest 10 dirs of one dir below current dir sub-system <br>
`du -h . --max-depth=2 | sort -n -r | head -n 10`


## Unlinked File Storage Hogging

Display pid of resources that processes are holding onto even if the origin is deleted<br>
`lsof | grep deleted | awk '{print $2}'`

Applications using unlinked files<br>
`lsof +L1`

Removing those files using their ids and killing the processes holding them
```
lsof | grep deleted | awk '{print $2}' > ids

#!/bin/bash
input="./ids"
while IFS= read -r line
do
  kill -9 "$line"
done < "$input"
```