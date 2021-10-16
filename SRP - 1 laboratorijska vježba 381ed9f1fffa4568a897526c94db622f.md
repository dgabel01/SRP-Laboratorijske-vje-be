# SRP - 1.laboratorijska vježba

ARP spoofing je Man in the Middle napad u kojem napadač presreće

komunikaciju između uređaja na lokalnoj mreži(LAN).ARP protokol

prevodi IP adrese u MAC adrese.

Napadač koristi spoofing alat pomoću kojeg izvede da se 2 računala koje napada 

spoje na računalo napadača umjesto jedno sa drugim šaljući maliciozneARP pakete

na LAN switch kako bi postigao da se MAC adresa napadača poistovjetisa adresom 

žrtava. Sav mrežni promet tada ide preko računala napadača koji može vidjeti 

komunikaciju između dviju žrtava napada.

Na vježbama je korištena virtualizirana Docker mreža sa 3 virtualna Docker računala:

- station-1(žrtva 1)
- station -2(žrtva 2)
- evil-station(napadač)

Isječci koda iz terminala:

- git clone (https://github.com/mcagalj/SRP-2021-22) → repozitorij sa spoofing alatom 
    
- $ docker exec -it station-1 bash → pokretanje prvog virtualnog računala
- $ ping station-2 → slanje paketa sa drugog računala, provjera veze
- $ hostaname station-1 ; $ netcat -l -p 9000 ; $ netcat station-2 9000→ spajanje na zajednički port
- $ docker exec -it evil-station bash → pokretanje$t računala koje napada
- $ tcpdump -X host station-1 and not arp → prikazuje napadaču promet između
    2 računala bez vidljivosti ARP paketa