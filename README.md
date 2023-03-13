# bio_avanzada2023
Texto para clase

```
#!/bin/bash

code="$1"

curl "https://rest.uniprot.org/proteomes/stream?format=json&query=%28$code%29" -o $code".json"

echo $code
```

# -

```
for i in names:
    print('start cmd /c ordenes.sh ' + i)
    os.system('start /wait cmd /c ordenes.sh ' + i) # k = keep, c = close
```
