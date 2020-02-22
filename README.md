# JSON Web Token (JWT) Demo

## JWT mit Python erzeugen

1. Um in Python mit JWTs zu experimentieren, kann man die Bibliothek `pyjwt` verwenden. Für die kryptographischen Funktionen wird `cryptography` benötigt.

```
!pip install pyjwt cryptography
```

2. Die Claims werden im Dictionary `payload` abgelegt. Der Secret key ist im ersten Beispiel eine zufällige Zeichenfolge. 

Über die Funktion `jwt.decode(payload, secret_key, algorithms='HS256')` wird das Token mit HMAC SHA-2 signniert. Der Nachteil dieser Methode ist, dass sowohl die signierende als auch die verifizierende Seite das *Secret* kennen muss.

[](https://de.wikipedia.org/wiki/Keyed-Hash_Message_Authentication_Code)

```
import jwt
import time
payload = {'user_id': 12345, 'exp': time.time()+60*60}
secret_key = '314254353 3f<fxf<7f697<df 7d89sf798a7sf sdfff'
token = jwt.encode(payload, secret_key, algorithm='HS256')
print(token)
```

3. Um das oben beschriebene Problem zu umgehen, kann ein asymmetrisches Verschlüsselungsverfahren wie RSA verwendet werden.
Dazu wird zunächst eine RSA-Schlüsselpaar, z.B. mit `ssh-keygen -t rsa -b 4096` erzeugt und im Python Code importiert.

```
private_key = open('dummy_key').read()
public_key = open('dummy_key.pub').read()
```

4. Das Token berechnet man dann aus den Claims und dem Privaten Schlüssel mit dem Algorithmus `RS256`.

```
token = jwt.encode(payload, private_key, algorithm='RS256').decode('utf-8')
```

5. Zum Verifizieren des Tokens genügt dann der Public Key.

```
jwt.decode(token, public_key, algorithms=['RS256'])
```



## Online JWT Debugger

Auf [jwt.io](https://jwt.io/) findet man ein kleines Web-Tool mit dem binäre JWT Tokens generiert und verifiziert werden können.
![ajwt.io](https://github.com/fh-swf-hgi/jwt-demo/raw/master/jwt_io.png)
