# Torpedójáték programterv

## A csatamező egy koordinátájának az alábbi tulajdonságai lehetnek:

. = szabad
X  = lövött mező
\# = hajó / találat mező


## A hajók:

"a" változó   elemszám : 1  Így néz ki: #
"b" változó   elemszám : 2  Így néz ki: # # 
"c" változó   elemszám : 2  Így néz ki: # #
"d" változó   elemszám : 3  Így néz ki: # # #
"e" változó   elemszám : 3  Így néz ki: # # #
"f" változó   elemszám : 4  Így néz ki: # # # #


***Minden hajónak azaz listának (a-f) az elemszámával azonos alapértéke van.***
Tehát: 
a = 1
b = 2
c = 2
d = 3
e = 3
f = 4

***Ha a hajót találat érte, akkor csökken az értéke 1-gyel és ha az értéke egyenlő 0, akkor az süllyedt hajó.***

A változók össze értéke: 15

***Ha a változók összes értéke lecsökken 0-ra, akkor azt jelenti, hogy az összes hajó összes elemét eltalálták, a játékos nyert.***

Municio = 50

Ha a municio = 0 és a hajók összes értéke !0 akkor a játékos vesztett.


**A csatamező: matrix, de a mezők értékeit nem mátrix-ban tároljuk majd, hanem könyvtárakban.**

               A   B   C   D   E   F   G   H   I   J

               
            0  .   .   .   .   .   .   .   .   .   .
             
            1  .   .   .   .   .   .   .   .   .   .

            2  .   .   .   .   .   .   .   .   .   .

            3  .   .   .   .   .   .   .   .   .   .

            4  .   .   .   .   .   .   .   .   .   .

            5  .   .   .   .   .   .   .   .   .   .

            6  .   .   .   .   .   .   .   .   .   .

            7  .   .   .   .   .   .   .   .   .   .

            8  .   .   .   .   .   .   .   .   .   .

            9  .   .   .   .   .   .   .   .   .   .

Kezdetben minden mező értéke: .

A csatamezőknek két koordinátája van: x: 0 - 9 és y: 0 -9 
A játék során az y koordinátát betűvel helyettesítjük: A - J (könyvtár)


**feltérképezés()**
    A kiinduló mezőnek két koordintája van, ezeknek az értékeit keressük a csatamezőn. Ha szabadot találunk, akkor beletesszük egy ideiglenes listába, azaz kivesszük a további lehetséges mezők közül, majd ha van elegendő szabad mező, akkor a lista elemeit átadjuk kitöltésre (return)

    0,0 0,1 0,2 0,3 0,4 0,5 0,6 0,7 0,8 0,9
    1,0 1,1 1,2 1,3	1,4	1,5	1,6	1,7	1,8	1,9
    2,0 2,1	2,2	2,3	2,4	2,5	2,6	2,7	2,8	2,9
    3,0 3,1	3,2	3,3	3,4	3,5	3,6	3,7	3,8	3,9
    4,0 4,1	4,2	4,3	4,4	4,5	4,6	4,7	4,8	4,9
    5,0 5,1	5,2	5,3	5,4	5,5	5,6	5,7	5,8	5,9
    6,0 6,1	6,2	6,3	6,4	6,5	6,6	6,7	6,8	6,9
    7,0 7,1	7,2	7,3	7,4	7,5	7,6	7,7	7,8	7,9
    8,0 8,1	8,2	8,3	8,4	8,5	8,6	8,7	8,8	8,9
    9,0 9,1	9,2	9,3	9,4	9,5	9,6	9,7	9,8	9,9


**Ha véletlenszerűen kiválasztunk egy mezőt és egy lépést jobbra haladunk (y+1), akkor a szám eggyel nő, ha balra lépünk, akkor (y-1) eggyel csökken. Ha előrelépünk (x+1) a szám 10-zel nő, ha "hátra" (x-1), akkor 10-zel csökken.**

Tehát
- előre:    mező + 10
- hátra:    mező - 10
- jobbra:   mező + 1
- balra:    mező - 1

**A csatamező széle**

- előre :  mezo_szama + 10   x + 1
- hátra:   mezo_szama - 10   x - 1
- jobbra:  mezo_szama + 1    y + 1
- balra:   mezo_szama - 1    y - 1

Lehetne ekkor forgolódni, hogy másfelé van - e elég hely, vagy a véletleszám generátorhoz visszairányítani a programot, teljesen újrakezdeni az egészet.

**Feltérképezés: szélsőértékek**

- előre: 10 - mezo_szama // 10 + hajó értéke   ha < 0 akkor nincs elég hely
- hátra: mezo_szama // 10 - hajó értéke   ha  < 0 akkor nincs elég hely
- jobbra: 10 - mezo_szama % 10 + hajó értéke   ha < 0 akkor nincs elég hely
- balra: mezo_szama  10 - hajó értéke   ha  < 0 akkor nincs elég hely

**Legyen az, hogy a mező tulajdonságait egy könyvtár típusú adatszerkezetben tároljuk.**

mezo1  { mezo_szama: 0 - 99
         x : 0 - 9
         y : 0 - 9
         mezo_erteke: . # X a - f
}

A mezőket nem egy listäban, hanem egy könyvtárban tároljuk: rendezett, iterálható, változtatható.

_Egyelőre nem tudom, hogy miért jobb ez, a stackoverflow-n találtam egy iterációt könyvtárszerkezetek létrehozására, megpróbáltam létrehozni egy listát könyvtárakból, nem sikerült, de úgy ahogy ott van, könyvtár a könyvtár szerkezetben megy._

<https://stackoverflow.com/questions/16302185/how-do-i-create-multiple-dictionaries-based-on-nested-for-loops>

- Ciklus: a - f hajók létrehozása
- El kell helyezni a csatamezőben a hajókat, amely úgy történik, hogy a csatamező koordinátái közül véletlen generátorral kiválasztunk egy mezőt.
  Szabad a mező? ha igen
Ciklus: annyiszor amennyi a hajó értéke: feltérképezés () | minden rendben akkor a koordináták feltöltése # = hajó / találat mező
-A játék kezdetén kirajzoljuk a csatamezőt.

Üdvözöljük a játékost, elmondjuk, hogy milyen hajók vannak és hogy 50 darab lövése van.

A játék végén, ha a játékos nem nyert kirajzoljuk az el nem talált mezőket is és megkérdezük, hogy akar-e még újra játszani.


