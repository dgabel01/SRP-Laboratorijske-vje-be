# SRP - 4.laboratorijska vježba

- Dio 3.vježbe - određivanje autentičnosti slike

Zadatak:
→Sa lokalnog servera odrediti koja je od dviju slika autentična

- Slika je poptisana privatnim ključem, a provjeru vršimo kodom:

korištenjem funkcije verify_signature_rsa sa githuba i sa naredbama:

with open("image_1.sig","rb") as file:
      signature = file.read()          #citanje potpisa prve slike, isto i za drugu sliku

with open("image_1.png","rb") as file:
     image = file.read()
is_authentic = verify_signature_rsa(signature, image)  #funkciji šaljemo sliku i

potpis, ona vraća True ili False

- 4.vježba - usporedba vremena izvršavanja kriptografskih hash funkcija

-Korišten kod sa githuba i dodana provjera vremena izvršavanja za 100 iteracija za funkcije:

AES, vrijeme :0.001063s
HASH_MD5, vrijeme: 5.9e-05s
HASH_SHA256, vrijeme:
Linux CRYPTO , 5k iteracija, vrijeme:
Linux CRYPTO , 1M iteracija, vrijeme:2.29792s