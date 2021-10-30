# SRP - 2.laboratorijska vježba

Opis vježbe : 

- rješavanje crypto izazova, tj. dešifriranje odgovarajućeg cipertexta u kontekstu
simetrične kriptografije
- svaki izazov je personaliziran, pohranjen u obliku datoteke na internom serveru adrese [http://a507-server.local](http://a507-server.local/) , ime datotetke enkriptirano(sa hash funkcijom) u oblku prezime_ime
- cilj je nakon pronalska vlastite enkriptirane datoteke korištenjem Fernet sustava za simetričnu
enkripciju dobiti plaintext odnosno .PNG sliku metodom brute force-a

![20211025_165747.jpg](SRP%20-%202%20laboratorijska%20vjez%CC%8Cba%206c31d23a6c4a44c1bf9650588ecbd597/20211025_165747.jpg)

                                                 Izgled personalizirane datoteke
   

Dijelovi koda za rješavanje izazova:

1. Prvo moramo saznati koja je naša datoteka sa mreže:
from cryprography.hazmat.primitives import hashes —> korištene biblioteke
def hash(input): → definiramo funkciju koja vraća unos u obliku hash-a
if _ _ name == "_ _main_ *"
     hash('gabela_dominik')*

2. Provjeravamo je li traženi format .PNG :
def test_png(header):
    if header.startswith(b'211PNG\r\n\032\n'):
          return True

3. Definiramo funkciju brute_force() kojom ćemo pronaći naš ključ :
from crytography.hazmat.primitives import hashes
from cryprography.fernet import Fernet —> sustav za simetričnu enkripciju
import base64—> biblioteka za pretvaranje binarnog podatka u ASCII i obrnuto

def brute_force():
   filename = "naše prezime_ime hashirano"
   with open(filename, "rb") as file:             
         cipertext ) file.read()                         //čitamo iz datoteke
   ctr = 0
   while True:
       key_bytes = ctr.to_bytes(32,"big")
       key = base64.urlsafe_b64encode(key_bytes)        //iteriranje kroz ključeve
       if not(ctr+1)%1000
          print(f"[*] Keys tested: {ctr+1:,}", end = "\r")

      try:           //provjera ključa

        plaintext = Fernet(key).decrypt(cipertext)
        header = plaintext[:32]
        if test_png(header):
            print(f"[+] KEY FOUND: {key}")
            with open("BINGO.png", "wb") as file:  
                  file.write(plaintext)
                  break
       except Exception:
                pass

      ctr+=1
        

**