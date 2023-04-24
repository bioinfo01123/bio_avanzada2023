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



```
writer = pd.ExcelWriter('Metadata.xlsx')
DataFrame(metadata).to_excel(writer, 'dataframe1', engine='openpyxl', index = None)
writer.save()
```



```
n = 0
bloque = []
for i in range(int(len(lista)/50)):
    diez = lista[n:n+50]
    print(diez)
    bloque.append(diez)
    n += 50
```



# Descargar Blast
https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.8.1/ncbi-blast-2.8.1+-win64.exe



# Plot

import matplotlib as mpl
import matplotlib.pyplot as plt

print('Sequence length =', len(record.seq))
fig = plt.figure(figsize=(20, 4))

ax = fig.add_axes([0, 0, 1, 1])
ax.set_ylim(0, 2)

ax.plot([0, len(record.seq)], [1, 1], color = 'silver', lw = 5)

N = 0
for st, x1, x2 in save:
    if st == 1:
        ax.plot([x1, x2], [1.1, 1.1], color = 'black', linestyle='-', lw = 5)
        ax.arrow(x1, 1.2, x2-x1, 0, head_width = 0.1, head_length = 40, fc ='blue', ec ='blue')
        N += 0.01
M = 0
for st, x1, x2 in save:
    if st == -1:
        ax.plot([x1, x2], [0.9, 0.9], color = 'black', linestyle='-', lw = 5)
        ax.arrow(x2, 0.8, x1-x2, 0, head_width = 0.1, head_length = 50, fc ='red', ec ='red')
        M -= 0.01

plt.close()
fig
