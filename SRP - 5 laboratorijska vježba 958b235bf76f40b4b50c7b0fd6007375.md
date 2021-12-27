# SRP - 5.laboratorijska vježba

- Opis vježbe: demonstracija online i offline password guessing napada

- Online password guessing napad:

-Uspostava komunikacije sa serverom `a507-server.local` na kojemu su lozinke
-Korištenjem alanta nmap smo ograničili komunikaciju na uređaje u laboratirju A507
 -naredbom `nmap -v 10.0.15.0/28`  smo ograničili komunkaciju na 16 uređaja tj 
  IP adresa
-Sa servera pročitamo korisiničko ime i IP adresu: gabela_dominik ;  10.0.15.11
-Pomoću ssh klijenta se sa prethodnim podacima spajamo na server
-Server traži lozinku, koja nije poznata te je zadatak saznati je

Znamo da se lozinka sastoji od malih slova(engleske abecede) dužine 4 do 6 slova

S obzirom da postoji 2^(30) kombinacija lozinke:dobiveno 26^4 * 26^5 * 26^6 **≈ 2^30**

tada bi brute force napad trajao 2^5 godina te zbog toga vršimo pre computed dictionary napad.

Dictionary je preuzet sa servera naredbom:`wget -r -nH -np --reject "index.html*" http://a507-server.local:8080/dictionary/g2/`

Napad je izvršen pomoću alata hydra:

→`hydra -l gabela_dominik -x 4:6:a 10.0.15.11 -V -t 1 ssh`
   4:6:a označava duljinu od 4 do 6 znakova, :a označava lowercase slova

→`hydra -l gabela_dominik -P dictionary/g4/dictionary_online.txt 10.0.15.11 -V -t 4 ssh`  → ponovljeni napad sa rangeom dicitonary-a,  dobivena lozinka je bila altybl te je sada 

moguće spajanje na server sa korisničkim imenom gabela_dominik i lozinkom altybl.

- Offline password guessing napad
    
    -Ovaj napad se izvodi korištenjem hasheva koji su pohranjeni na Linux OS-u u folderu
    kojem pristup ima samo administrator
    -Pomoću naredbe `sudo cat /etc/shadow` smo pristupili listi korisnika i hasheva
    -Odabranu hash vrijednost(korisnika Jean Doe) kopiramo u hash.txt 
    

Napad smo izveli pomoću hashcat alata, a duljina lozinke je 6 znakova, lowercase slova.
Također je korišten dictionary(dicitonary_offline.txt) te smo hash saznali pomoću naredbe

`hashcat --force -m 1800 -a 0 hash.txt dictionary/g4/dictionary_offline.txt --status --status-timer 10`

Rezultat napada je bila lozinka sfonth te smo se uspjeli logirati sa naredbom

`ssh jean_doe@10.0.15.11`