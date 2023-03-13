# bio_avanzada2023
Texto para clase

```
#!/bin/bash

code="$1"

curl "https://rest.uniprot.org/proteomes/stream?format=json&query=%28$code%29" -o $code".json"

echo $code
```
