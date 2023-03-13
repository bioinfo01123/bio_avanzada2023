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

# -

```
metadata = []
summary = {}
M = 1
for n in names:
    f = open(n+'.json')
    data = json.load(f)
    N = 1
    for i in data['results']:
        if ('redundantProteomes' in list(i.keys())) or ('redundantTo' in list(i.keys())):
            print(M, N, up, taxid, protcount, ' '.join(org.split(' ')[:2]), orden)
            pass
        else:
            up = i['id']
            taxid = i['taxonomy']['taxonId']
            protcount = i['proteinCount']
            org = i['taxonomy']['scientificName']
            orden = [list(j.values())[0] for j in i['taxonLineage'] if 'order' in list(j.values())][0]
            if 'sp.' in org:
                pass
            else:
                if int(protcount) >= 7000:
                    # en esta parte se quedan solo los que pasan los filtros
                    #print(M, N, up, taxid, protcount, ' '.join(org.split(' ')[:2]), orden)
                    summary[n] = N
                    metadata.append([M, # indice
                                     N,
                                     up, # numero UP
                                     ' '.join(org.split(' ')[:2]), # organismo
                                     taxid, # numero taxonomico
                                     protcount, # proteome size
                                     orden])  # orden
                    N += 1
                    M += 1
```
