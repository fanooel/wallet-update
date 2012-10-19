wallet-update
=============

Założenia:

* Sprawdza z grupy sudo kto ma dostęp.
* Dla każdego z grupy sprawdza:
	1. istnieje katalog $KP_DIR.
		1. Jak nie istnieje to zakłada i daje prawa do odczytu i zapisu tylko dla właściciela
	1. Porównuje $KP_DIR z wzorcem.
		1. Jeżeli $KP_DIR ma inne sumy kontrolne  i datę modyfikacji do wzorca:
			1. Dodaj do dziennika czynność z sumą kontrolną, nazwą konta i datą.
		1. Jeżeli nie ma pliczku z wzorca- skopiuj i ustaw prawa dla właściciela.
	1. Analizuje dziennik.
		1. Jeżeli dany plik ma więcej niż jedną sumę kontrolną- czyli ten sam plik jest zmodyfikowany względem wzorca przez więcej niż jedno konto:
		1. Powiadom mailem o konflikcie $ADMIN_MAIL.
		1. Ustaw wszystkim dany plik na tylko do odczytu.
		1. Zaktualizuj pliczek $KP_DIR/conflicts
	1. Jeżeli jest tylko jedna suma kontrolna- czyli tylko jeden plik ma różnicę względem wzorca:
		1. Zrób xdiffa dla zmienionego pliku względem wzorca.
		1. Zapisz xdiffa w katalogu wzorca.
			* w katalogu: nazwa_pliku
			* pod nazwą: suma_kontrolna_oryginału
			* sprawdź czy po nałożeniu diffa na oryginał dostaniemy taką samą sumę kontrolną co nowego pliku
		1. Dodaj wpis do bazy wzorca.
			* Ddata
			* Plik
			* Konto
			* Suma, wielkość, data modyfikacji oryginału
			* Suma, wielkość, data modyfikacji nowego pliku
		1. Nałóż diffa na wzorzec i sprawdź sumę kontrolną.
		1. Jeżeli dane zostały zapisane w bazie wzorca i suma w wzorcu jest zgodna:
			* Dla wszystkich kont:
			* Sprawdź czy suma kontrolna pliku jest taka jak dla starego wzorca
			* Przekopiuj nowy plik i nadpisz stary.
