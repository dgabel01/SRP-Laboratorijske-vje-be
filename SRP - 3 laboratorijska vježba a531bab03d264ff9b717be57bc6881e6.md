# SRP - 3.laboratorijska vježba

Opis vježbe:

- primijeniti simetrični kriptografski mehanizam za zaštitu poruka: MAC(Message Authentication Code) mehanizam
- podijeljeno u 2 izazova: -zaštita integriteta poruke korištenjem MAC algoritma
                                         -utvrditi vremenski ispravnu sekvencu transakcija digitalno
                                           potpisanih naloga koji se nalaze na lokalnoj mreži

Izazov 1
- Python kod:

`from cryptography.hazmat.primitives import hashes, hmac
from cryptography.exceptions import InvalidSignature`

```markdown

def verify_MAC(key, signature, message):   //potvrda MAC-a
    if not isinstance(message, bytes):
        message = message.encode()

    h = hmac.HMAC(key, hashes.SHA256())
    h.update(message)
    try:
        h.verify(signature)
    except InvalidSignature:
        return False
    else:
        return True
def generate_MAC(key, message):     //stvaranje MAC-a
    if not isinstance(message, bytes):
        message = message.encode()

    h = hmac.HMAC(key, hashes.SHA256())
    h.update(message)
    signature = h.finalize()
    return signature

if __name__ == "main":

key = b"velika tajna"
with open("message.txt","rb") as file:
   content = file.read()
mac = generate_MAC(key, content)
with open("message.sig","wb") as file: //u datoteku message.sig smo spremili mac
	file.write(mac)                      //koji smo mijenjali(tekst i hex)kako bi 
is_authentic = verify_MAC(key,mac,content)    //vidjeli radi li funkcija
print(is_authentic)
```

Izazov 2

- Python kod:

```markdown
from cryptography.hazmat.primitives import hashes, hmac
from cryptography.exceptions import InvalidSignature
import os

def verify_MAC(key, signature, message):   
    if not isinstance(message, bytes):
        message = message.encode()
h = hmac.HMAC(key, hashes.SHA256())
h.update(message)
try:
    h.verify(signature)
except InvalidSignature:
    return False
else:
    return True

def generate_MAC(key, message):     
    if not isinstance(message, bytes):
        message = message.encode()

    h = hmac.HMAC(key, hashes.SHA256())
    h.update(message)
    signature = h.finalize()
    return signature

if __name__ == "main":

		key = "gabela_dominik".encode()
    path = "mac_challenge"
    for ctr in range(1, 11):
        msg_filename = f"order_{ctr}.txt"
        with open(os.path.join(path, msg_filename), "rb") as file:
            message = file.read()

        mac = generate_MAC(key, message)

        sig_filename = f"order_{ctr}.sig"
        with open(os.path.join(path, sig_filename), "rb") as file:
            signature = file.read()
        is_authentic = verify_MAC(key, signature, mac)

        print(f'Message {message.decode():>45} {"OK" if is_authentic else "NOK":<6}')
```