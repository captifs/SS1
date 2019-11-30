

Initial am format matricea Hurwitz.


| a(n-1)       | a(n-3)           | a(n-5)  |
| ------------- |:-------------:| -----:|
| a(n)      | a(n-2) | a(n-3) |
| 0      | a(n-1)      |   a(n-3) |

Primind sistemul meu,  am memorat in H matricea Hurwitz:

| 1.5386       | 3.6277       | 0  |
| ------------- |:-------------:| -----:|
| 1      | 3.9354 | 0 |
| 0      |1.5386   | 3.6277 |

Dupa care generez matricile M1 1x1, M2 2x2 din H , iar in variabilele 
det1,det2, det3 memorez determinantii 
matricilor M1,M2 si respectiv H.

```bash
det1 = det(M1) = 1.5386
det2 = det(M2) = 2.4273
det3 = det(M3) = 8.8055
```
Analizam P_tan.den{1} ca este vector linie , deci ca sa punem numitorul 
intr-un vector coloana facem:

`numitor = P_tan.den{1}'` , deci:
numitor =
```bash
    1.0000
    1.5386
    3.9354
    3.6277
```

Aflu polii din vectorul numitor prin : 
`poli = roots(numitor);`

poli =
```bash
  -0.2400 + 1.8357i
  -0.2400 - 1.8357i
  -1.0585 + 0.0000i
```

Observam ca toti polii au partea reala negativa , confirmand din nou stabilitatea 
procesului.

Am apelat impulse pentru P_tan pe intervalul t in vectorul h_poondere, dupa grafic
vedem ca tinde la 0.

`[h_pondere] = impulse(P_tan,t);`

Timpul indicial la intrare treapta: 

`[rasp_trp] = step(P_tan,t);`

Pentru a calcula convolutia intre trp( vectorul coloana treapta unitara de dimensiunea lui t) si h_pondere, am folosit:
```bash
rasp_conv = conv(trp,h_pondere)*0.01;
 rasp_conv = cv(1:length(t));
norma_dif = norm( rasp_trp - rasp_conv,inf);
```

Observam ca ne da 0. 
Facem graficile celor 2 semnale:

```bash
plot(rasp_trp)
hold on 
plot(rasp_conv)
```

Vedem ca graficile se suprapun. 

Creez modelul de spatiu cu comanda: 

`rasp_tot = lsim(ss(P_tan,'min'),trp,t,x0)`

Pentru a calcula raspunsul permanent , 
inmultesc trp cu valoarea lui P_tan evaluat in 0.

`rasp_perm = trp*evalfr(P_tan,0)`


Diferenta intre rasp_tot si rasp_perm l-am memorat in
rasp_tran

```bash
plot(rasp_perm)
figure(2)
plot(rasp_tran)
```

Vedem ca graficul rasp_tran este marginit , iar graficul
rasp_perm tinde asimptotic la 0.

Calculez raspunsul liber in rasp_libr cu initial()
```bash
rasp_libr = initial(ss(P_tan,'min'),x0,t)
rasp_fort = rasp_tot - rasp_libr
```


Grafic , observam ca rasp_fort si rasp_trp sunt suprapuse
```bash
figure(1)
plot(rasp_fort)
hold on
plot(rasp_trp)
```
Apoi observ ca graficul lui rasp_libr tinde asimptotic la 0 
caci P_tan este stabil
`plot(rasp_libr)`

S = stepinfo(P_tan)
 Pentru ca  sa aflu valorile din P_tan pentru:
1. tc1 - timpul de crestere
1. tt1 - timpul tranzitoriu
1. tv1 - timpul de varf
1. sr1 - suprareglajul
