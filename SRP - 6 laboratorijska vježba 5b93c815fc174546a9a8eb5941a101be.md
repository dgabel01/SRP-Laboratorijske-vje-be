# SRP - 6.laboratorijska vježba

Cilj vježbe: upoznavanje sa postupkom upravljanja korisničkim računima na operacijskom
sustavu Linux, točnije kontrolu pristupa file-ovima.

Vježba je podijeljena u 4 dijela:

# A.Kreiranje novog korisničkog računa

→Prvo smo se spojili na Linux računalo preko Windows terminala naredbom “ wsl”
→Kako bi saznali identitet korisnika korištena je naredba “id” te provjerimo da korisnik
pripada “sudo”(super user do)  grupi(administratorska grupa) naredbom “groups”

→Zatim smo kreirali novog korisnika alice2 naredbom “sudo adduser alice2
→Provjera login-a sa novim korisnikom: “su - alice2”

# B.Standardna prava pristupa datotekama

→Logiramo se kao alice2 te u home direktoriju kreiramo novi direktorij srp

→Unutar direktorija srp kreiramo tekstualnu datoteku “security.txt” sa sadržajem
”Hello world” sa sljedećim naredbama:
”mkdir srp”,   “cd srp”,    “echo “Hello World”>security.txt”,     “cat security.txt”
→Za provjeru prava vezanih za file ili direktorij koristili smo narebu “getfacl”:
”getfacl security.txt” te vidimo tko ima prava na čitanje(r), pisanje(w) i izvršavanje(x)
→U sljedećim koracima smo modificirali prava naredbom “chmod”, kao na primjer:

“chmod u-r security.txt “ oduzmemo pravo čitanja .txt datoteke vlasniku
”chmod u+r security.txt” dodali pravo čitanja .txt datoteke vlasniku

# C.Kontrola pristupa korištenjem Access Control Lists(ACL)

→Za gledanje i promjenu ACL resursa koristimo naredbe “getfacl” i “setfacl”

→Korisniku bob2 kojeg smo kreirali dodamo prava čitanja koristeći
”setfacl -m u:bob2:r security.txt”
→Korisniku bob2 također možemo dati prava čitanja tako da kreiramo grupu kojoj
pridijelimo prava čitanja te će ih imati svaki novi korisnik te grupe
”groupadd alice_reading_group2“
→Za dodavanje u grupu, korisniku bob2 treba ukloniti prethodna prava, dodijeliti
ih grupi te onda korisnika bob2 dodati u grupu koristeći naredbe:

“setfacl -r u:bob2 security.txt”   ,     “setfacl g:alice_reading_group2: r security.txt”   ,        
“usermod -aG alice_reading_group2”

# D.Linux procesi i kontrola pristupa

Svaki proces u Linuxu ima vlasnika(UID) i jedinstveni identifikator procesa(PID).
→Moramo korisnika bob2 ukloniti iz grupe alice_reading_group2 narebom:
”gpasswd -d bob2 alice_reading_group2”
→U trenutnom direktoriju otvorimo Visual studio code te unutar njega napišemo
Python skriptu kojom provjeravamo UID korisnika koji pokreće skriptu te gledamo
možemo li otvoriti datoteku security.txt:

import os

print('Real (R), effective (E) and saved (S) UIDs:')
print(os.getresuid())

with open('/home/alice/srp/security.txt', 'r') as f:
print(f.read())

→Izvršimo skriptu kao administrator:

R:1000, E:1000, S:1000 i možemo otvoriti datoteku iako se ne nalazi u ACL-u koji je vezan
za ovu datoteku
→Izvršimo skriptu kao alice2 možemo otvoriti datoteku, a kao bob2 dobijemo permission
denied(jer nema prava)