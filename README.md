#Program transformujący współrzędne do wybranych układów odniesienia
Program służy do przeliczania współrzędnych geocentrycznych (X,Y,Z) do innych układów, bazując na różnych płaszczyznach odniesienia. Elipsoidami obsługiwanymi w programie są elipsoida WGS 84 oraz elipsoida GRS 80.

##Wymagania programowe
Aby program działa poprawnie potrzebujemy programu obsługującego Pythona w wersji 3.8 lub nowszej.  
Potrzebne są również biblioteki math, numpy oraz argparse, które musimy zaimplementować do naszego programu.
Program został napisany dla systemu windows.

##Przykład użycia

```python
# utworzenie obietku
geo = Transformacje(model="grs80")
# wczytanie danych
parametry = geo.wczytaj_dane("wsp_inp.txt")
# uruchomienie transformacji
geo.uruchom(*parametry)
```

```
Czy chcesz obliczyć odległosc miedzy dwoma punktami?
>> tak
Podaj numer pierwszego punku:
>> 1
Podaj numer drugiego punktu:
>> 3
Chcesz policzyć odległosć 2D czy 3D?
>> 2D
Czy chcesz policzyć jeszcze odległosc 3D?
>> nie
```

##Działanie programu

Program zadaje poniższe pytania użytkownikowi:

- Czy chcesz obliczyć odległosc miedzy dwoma punktami?
  Możliwa odpowiedź: tak
- Podaj numer pierwszego punktu
- Podaj numer drugiego punktu
  Przy tych podajemy dane o które prosi nas program
- Chcesz policzyć odległość 2D czy 3D?
  Możliwe odpowiedzi: 2D, 3D, nie
- Chcesz policzyć jeszcze odległość 3D?
  Możliwe odpowiedzi: tak, nie

Poniżej pokazane są dane wejściowe oraz wyjściowe dla każdej funkcji zawartej w programie.

###XYZ->FLH

```python
Transformacje.xyz2plh(X, Y, Z)
```

funkcja służąca do przeliczenia współrzędnych geocentrycznych XYZ na współrzędne geocentryczne krzywoliniwe (Fi, La, h) przy użyciu algorytmu Hirvonena

INPUT:

- `X` [float] - współrzędna X danego punktu [m]
- `Y` [float] - współrzędna Y danego punktu [m]
- `Z` [float] - współrzędna Z danego punktu [m]

OUT:

- `phi` [float] - szerokosc geodezyjna punktu [rad]
- `lam` [float] - długosć geodezyjna punktu [rad]
- `hel` [float] - wyokosć elipsoidalna punktu [m]

###Długość łuku południka

```python
Transformacje.sigma(phi)
```

funkcja, która służy do obliczenia długości łuku południka, konieczna do przejścia ze współrzędnych krzywoliniowych na płaszczyznę
odwzorowania Gaussa-Krügera

INPUT:

- `phi` [float] – szerokość geodezyjna danego punktu [rad]

OUT:

- `sigma` [float] – długość łuku południka [m]

###PLH -> GK

```python
Transformacje.plh2gk(phi, lam, lam0)
```

funkcja służąca do przeliczenia współrzędnych krzywoliniowych na współrzędne GK

INPUT:

- `phi` [float] - szerokosc geodezyjna punktu przeliczanego[rad]
- `lam` [float] - długosc geodezyjna punktu przeliczane [rad]
- `lam0` [float] - południk odniesienia, wybierany stosowanie do odwzorowania, na które chcemy przejsc później [rad]
- `a` [float] - dłuższa półos elipsoidy [m]
- `e2` [float] - pierwszy mimosród elipsoidy

OUT:

- `XGK` [float] - współrzędna X punktu na płaszczyźnie GK [m]
- `YGK` [float] - współrzędna Y punktu na płaszczyźnie GK [m]

###GK->PL2000

```python
Transformacje.gk2pl2(xgk, ygk, strefa)
```

funkcja służąca do przeliczenia współrzędnych GK do układu PL-2000

INPUT:

- `XGK` [float] - współrzędna X punktu na płaszczyźnie GK [m]
- `YGK` [float] - współrzędna Y punktu na płaszczyźnie GK [m]
- `strefa` [int] - strefa w której znajduję sie punkt w układzie PL-2000 [bez jednostek]

OUT:

- `x2` [float] - współrzędna X puntku w układzie współrzędnych płaskich PL-2000 [m]
- `y2` [float] - współrzędna Y puntku w układzie współrzędnych płaskich PL-2000 [m]

###GK->PL1992

```python
Transformacje.gk2pl92(xgk. ygk. strefa)
```

funkcja służąca przeliczeniu współrzędnych GK do układu PL-1992

INPUT:

- `XGK` [float] - współrzędna X punktu na płaszczyźnie GK [m]
- `YGK` [float] - współrzędna Y punktu na płaszczyźnie GK [m]

OUT:

- `x92` [float] - współrzędna X puntku w układzie współrzędnych płaskich PL-1992 [m]
- `y92` [float] - współrzędna Y puntku w układzie współrzędnych płaskich PL-1992 [m]

###PLH-> XYZ

```python
Transformacje.plh2xyz(phi, lam, hel)
```

funkcja odwrotna do funckcji xyz2plh. Pozwala ona na przeliczenie współrzędnych geocentyrcznych krzywoliniowych na wspoółrzędne geocentryczne prostokątne

INPUT:

- `phi` [float] - szerokosc geodezyjna punku [rad]
- `lam` [float] - długosc geodezyjna punktu [rad]
- `hel` [float] - wysokosc elipsoidalana punktu

OUT:

- `x` [float] - wspołrzędna X punktu [m]
- `y` [float] - współrzedna Y punktu [m]
- `z` [float] - wspoółrzędna Z puntku [m]

###NEU

```python
Transformacje.neu(x, y, z)
```

Funkcja służy do obliczenia współrzednych topocentrycznych, przyumujących jako początek ukłądu współrzędnych, punkt o współrzędnych srednich. Funkcja ta oblicza odpowiadnią macierz obrotu układu, a także oblicza współrzędne punktów

INPUT:

- `x` [array] - tablica zawierająca współrzędne X punktów [m]
- `y` [array] - tablica zawierająca współrzędne Y punktów [m]
- `z` [array] - tablica zawierająca współrzędne Z punktów [m]

OUT:

- `delta_neu` [array] - tablica o trzech kolumnach oraz zmiennej liczbie wierszy zależnej od ilosci punktów, które przeliczamy. W pierszej kolumnie znajdują się wartosci N, w drugiej E, w trzeciej Z [m]

###ODL2D

```python
Transformacje.odl2D(x1, y1, x2, y2)
```

funkcja służąca do do obliczenia odlełosci 2D między dwoma punktami.

INPUT:

- `x1` [float] - współrzędna X punktu pierwszego [m]
- `y1` [float] - współrzedna Y punktu pierwszego [m]
- `x2` [float] - współrzędna X punktu drugiego [m]
- `y2` [float] - współrzedna Y punktu drugiego [m]

OUT:

- `odl` [float] - odlełosc 2D między punktami [m]

###ODL3D

```python
Transformacje.odl3D(x1, y1, z1, x2, y2, z2)
```

funkcja służąca do obliczenia odlełosci 3D między dwoma punktami.

INPUT:

- `x1` [float] - współrzędna X punktu pierwszego [m]
- `y1` [float] - współrzedna Y punktu pierwszego [m]
- `z1` [float] - współrzedna Z punktu pierwszego [m]
- `x2` [float] - współrzędna X punktu drugiego [m]
- `y2` [float] - współrzedna Y punktu drugiego [m]
- `z2` [float] - współrzedna Z punktu drugiego [m]

OUT:

- `odl` [float] - odlełosc 3D między punktami [m]

###Przykładowe wywołanie

```
Czy chcesz obliczyć odległosc miedzy dwoma punktami?
>>> tak

Podaj numer pierwszego punku:
>>> 2

Podaj numer drugiego punktu:
>>> 3

Chcesz policzyć odległosć 2D czy 3D?
>>> 2D

Czy chcesz policzyć jeszcze odległosc 3D?
>>> nie
```

Przy wpisaniu powyższych odpoowiedzi otwrzymujemy plik z danymi wyjściowymi 'wsp_out.txt', a wyniki takiego wywołania prezentują się następująco

```
Transformacje współrzędnych
Autor:Natalia Przygoda
Poniżej zestawienie współrzędnych podanych punktów
============================================================
|X		|Y		|Z		|
|3664940.500000 |1409153.590000 |5009571.170000 |
|3664940.510000 |1409153.580000 |5009571.167000 |
|3664940.520000 |1409153.570000 |5009571.167000 |
|3664940.530000 |1409153.560000 |5009571.168000 |
|3664940.520000 |1409153.590000 |5009571.170000 |
|3664940.514000 |1409153.584000 |5009571.166000 |
|3664940.525000 |1409153.575000 |5009571.166000 |
|3664940.533000 |1409153.564000 |5009571.169000 |
|3664940.515000 |1409153.590000 |5009571.170000 |
|3664940.514000 |1409153.584000 |5009571.169000 |
|3664940.515000 |1409153.595000 |5009571.169000 |
|3664940.513000 |1409153.584000 |5009571.171000 |
============================================================
	Jednostką są stopnie dziesiętne
|phi		|lam		|hel		|
|52.097272	|21.031533 	|141.399	|
|52.097272	|21.031533 	|141.400	|
|52.097272	|21.031533 	|141.403	|
|52.097272	|21.031533 	|141.408	|
|52.097272	|21.031533 	|141.410	|
|52.097272	|21.031533 	|141.402	|
|52.097272	|21.031533 	|141.407	|
|52.097272	|21.031533 	|141.411	|
|52.097272	|21.031533 	|141.407	|
|52.097272	|21.031533 	|141.405	|
|52.097272	|21.031533 	|141.408	|
|52.097272	|21.031533 	|141.406	|
============================================================
	Układ PL2000
|X		|Y		|hel		|
|5773722.721	|7502160.783 	|141.399	|
|5773722.715	|7502160.770 	|141.400	|
|5773722.710	|7502160.757 	|141.403	|
|5773722.706	|7502160.744 	|141.408	|
|5773722.706	|7502160.776 	|141.410	|
|5773722.710	|7502160.773 	|141.402	|
|5773722.704	|7502160.760 	|141.407	|
|5773722.703	|7502160.747 	|141.411	|
|5773722.710	|7502160.778 	|141.407	|
|5773722.712	|7502160.773 	|141.405	|
|5773722.708	|7502160.783 	|141.408	|
|5773722.714	|7502160.773 	|141.406	|
============================================================
	Układ PL1992
|X		|Y		|hel		|
|470125.415	|502159.437 	|141.399	|
|470125.408	|502159.424 	|141.400	|
|470125.404	|502159.411 	|141.403	|
|470125.400	|502159.398 	|141.408	|
|470125.400	|502159.430 	|141.410	|
|470125.404	|502159.426 	|141.402	|
|470125.398	|502159.414 	|141.407	|
|470125.397	|502159.401 	|141.411	|
|470125.404	|502159.432 	|141.407	|
|470125.405	|502159.426 	|141.405	|
|470125.402	|502159.436 	|141.408	|
|470125.407	|502159.427 	|141.406	|
============================================================
	Układ NEU dla układu o początku w punkcie
X = 3664940.517 Y = 1409153.581 Z = 5009571.168
|N		|E		|U		|
|0.01028	|0.01413 	|-0.00952	|
|0.00478	|0.00130 	|-0.00574	|
|0.00101	|-0.01086 	|0.00040	|
|-0.00220	|-0.02281 	|0.00734	|
|-0.00445	|0.00847 	|0.00277	|
|-0.00017	|0.00368 	|-0.00407	|
|-0.00504	|-0.00783 	|0.00269	|
|-0.00527	|-0.01970 	|0.00997	|
|-0.00077	|0.00988 	|-0.00030	|
|0.00155	|0.00434 	|-0.00170	|
|-0.00314	|0.01433 	|-0.00109	|
|0.00343	|0.00507 	|-0.00074	|
============================================================
Odległosc 2D miedzy punktami 2, a 3 wynosi: 0.014m.
```

## Napotkane trudnosci/błędy

Jedną z wad programu jest wykonanie transformacji bl do układów PL-1992 oraz PL-2000.
Aby tego dokonać transformację współrzędnych bl do układu PL-1992 należy skończystać kolejno z funkcji:
plh2gk, a następnie uzyskany wynik podstawić do funkcji gk2pl92. Do wykonania transformacji bl do układu
PL-2000 należy skorzystać najpierw z funkcji plh2gk, a następnie z podstawić uzyskany wynik do funkcji gk2pl2.

Kolejną wadą programu jest to iż współrzędne do stransformowania
trzeba zapisać w załączonym pliku wsp_inp.txt, ponieważ to z niego program odczytuje współrzędne,
a następnie przelicza i zapisuje w nowym pliku tekstowym o nazwie wsp_out.txt.
