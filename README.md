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

```
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
```



### Proyecto Erick y Alfredo


```
# módulos y funciones

import numpy as np
import re, regex
import warnings
warnings.filterwarnings("ignore")
import xlsxwriter

def time_ini():
    from datetime import datetime
    xx = datetime.now()
    return xx
def time_fin(i = 0):
    from datetime import datetime
    tim = datetime.now() - i
    return 'Time: {}'.format(tim).split('.')[0]


def open_file(file = '', size = 70):
    fas = {}
    with open(file) as fq:
        for line in fq:
            line = line.rstrip()
            if '>' in line:
                try:
                    fas[ent] = s
                except NameError:
                    ent = line.replace('>', '').split(' ')[0]
                N, s, ent = 0, '', line.replace('>', '').split(' ')[0]
            else:
                g = 0
                while g < 1:
                    s += re.sub('[*]', '', line)
                    g += 1
                N += 1
    fas.update({ent: s})
    del s
    for i in list(fas.keys()):
        if len(fas[i]) >= size:
            pass
        else:
            fas.pop(i)
    return fas

```

```
# código 

KMER = '3'
cv_threshold = 0.35
                
# query       
fas1 = open_file(file = 'Bifido1.faa')
# subject
fas2 = open_file(file = 'Bifido1.faa')

CORES = 4
tam = int(len(fas1) / CORES) # numero de procesos
ids = list(fas1.keys())


listas = {}
for e, k in enumerate(range(0, len(ids), tam)):
    if e == (CORES - 1):
        listas[e+1] = ids[k:len(fas1)]
        break
    else:
        listas[e+1] = ids[k:k+tam]

query_sets = {}
for q in fas1:
    que_set = set(regex.findall(r'\w{'+KMER+'}', fas1[q], overlapped=True))
    query_sets[q] = que_set
subject_sets = {}
for s in fas2:
    subject_set = set(regex.findall(r'\w{'+KMER+'}', fas2[s], overlapped=True))
    subject_sets[s] = subject_set

record3 = []
for L in listas:
    init = time_ini()
    record2 = []
    for q in listas[L]:
        for s in list(subject_sets.keys()):
            compartidos = len(query_sets[q] & subject_sets[s])
            if compartidos >= int(len(subject_sets[s]) * 0.15):
                record2.append([q, s, compartidos])
    #-------------------------------------------------------------
    for q, s, c in record2:
        Kqlen, Kslen, Qlen, Slen = len(query_sets[q]), len(subject_sets[s]), len(fas1[q]), len(fas2[s])
        A, B, C, D, E = c, Kqlen, Kslen, (Qlen - int(KMER) + 1), (Slen - int(KMER) + 1)
        # forma 1
        CV = np.std([A, B, C, D, E]) / np.mean([A, B, C, D, E])
        # forma 2
        #CV = pstdev([A, B, C]) / mean([A, B, C])
        
        if CV <= cv_threshold:
            record3.append(tuple([q, s, A, B, C, B + C, D, E, Qlen, Slen, CV, 1 - CV]))
        
        
    print(L, time_fin(i = init))
print('Qacc length=', len(fas1))
print('Sacc length=', len(fas2))

```


```
U = ['qacc', 'sacc', 'shared', 'Kqacc', 'Ksacc', 'Kqacc+Ksacc', 'KEqacc', 'KEsacc', 'qlen', 'slen', 'CV', '1-CV']
D = ['S200', 'S200', object, object, object, object, object, object, object, object, float, float]
dtype2 = list(zip(U, D))


data = []
datos00 = np.array(record3, dtype = object)
for u in ids:
    d = datos00[datos00[:, 0] == u]
    values = [tuple(i) for i in d]
    b = np.array(values, dtype = dtype2)
    c = np.sort(b, order='CV')[::1][:1]
    for x, y, z, n, m, o, p, q, r, s, t, v in c:
        if type(x == np.bytes_):
            x = x.decode()
        if type(y == np.bytes_):
            y = y.decode()
        data.append(tuple([x, y, z, n, m, o, p, q, r, s, t, v]))

ARRAY = np.array(data, dtype = dtype2)
metadata = []
for x, y, z, n, m, o, p, q, r, s, t, v in np.sort(ARRAY, order = 'CV')[::1]:
    if type(x == np.bytes_):
        x = x.decode()
    if type(y == np.bytes_):
        y = y.decode()
    metadata.append([x, y, z, n, m, o, p, q, r, s, t, v])
metadata = np.array(metadata, dtype = object)

## salvar los resultados en una hoja de excel

N = [[i] for i in ['qacc', 'sacc', 'shared', 'Kqacc', 'Ksacc', 'Kqacc+Ksacc', 'KEqacc', 'KEsacc', 'qlen', 'slen', 'CV', '1-CV']]

workbook = xlsxwriter.Workbook('arrays.xlsx')
worksheet = workbook.add_worksheet()


for e, i in enumerate(N):
    worksheet.write_column(0, e, i)

row = 1

for col, data in enumerate(metadata.T):
    worksheet.write_column(row, col, data)

workbook.close()

```
