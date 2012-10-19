wallet-update
=============

Założenia:

* Sprawdza z grupy sudo kto ma dostęp.
* Dla każdego z grupy sprawdza:
  1. istnieje katalog $KP_DIR.
		1.1 Jak nie istnieje to zakłada i daje prawa do odczytu i zapisu tylko dla właściciela
	2. Porównuje $KP_DIR z wzorcem.
		2.1 Jeżeli $KP_DIR ma inne sumy kontrolne  i datę modyfikacji do wzorca:
			2.1.1 Dodaj do dziennika czynność z sumą kontrolną, nazwą konta i datą.
		2.2 Jeżeli nie ma pliczku z wzorca- skopiuj i ustaw prawa dla właściciela.
	3. Analizuje dziennik.
		3.1 Jeżeli dany plik ma więcej niż jedną sumę kontrolną- czyli ten sam plik jest zmodyfikowany względem wzorca przez więcej niż jedno konto:
			3.1.1 Powiadom mailem o konflikcie $ADMIN_MAIL.
			3.1.2 Ustaw wszystkim dany plik na tylko do odczytu.
			3.1.3 Zaktualizuj pliczek $KP_DIR/conflicts

		3.2 Jeżeli jest tylko jedna suma kontrolna- czyli tylko jeden plik ma różnicę względem wzorca:
			3.2.1 Zrób xdiffa dla zmienionego pliku względem wzorca.
			3.2.2 Zapisz xdiffa w katalogu wzorca.
				* w katalogu: nazwa_pliku
				* pod nazwą: suma_kontrolna_oryginału
				* sprawdź czy po nałożeniu diffa na oryginał dostaniemy taką samą sumę kontrolną co nowego pliku
			3.2.3 Dodaj wpis do bazy wzorca.
				* Ddata
				* Plik
				* Konto
				* Suma, wielkość, data modyfikacji oryginału
				* Suma, wielkość, data modyfikacji nowego pliku
			3.2.4 Nałóż diffa na wzorzec i sprawdź sumę kontrolną.

			3.2.5 Jeżeli dane zostały zapisane w bazie wzorca i suma w wzorcu jest zgodna:
				* Dla wszystkich kont:
					* Sprawdź czy suma kontrolna pliku jest taka jak dla starego wzorca
					* Przekopiuj nowy plik i nadpisz stary.
