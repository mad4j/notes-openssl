# Ricette per OpenSSL
simple tasks using openssl library

## Verificare la versione di OpenSSL in uso

`$ openssl version`

## Creazione di una coppia di chiavi privata/pubblica

Generare una coppia di chiavi di 2048 bits in formato PEM (non cifrato)

`$ openssl genrsa -out kes.pem 20148`

Generare una coppia di chiavi di 2048 bits in formato PEM (cifrato usando AES256 CBC).
Viene richiesto di inserire una password per la cifratura del file.

`$ openssl genrsa -aes256 -out keys.pem 20148`

Visualizzare opzioni riguardanti la generazione di chiavi private/pubbliche

`$ openssl genrsa help`


## Esporare una chiave pubblica

`openssl rsa -in keys.pem -outform PEM -pubout -out public.pem`

Se il file in input `keys.pem` e' cifrato verra' richiesta la password di decifratura.

Se non viene specificato il parametro `-pubout` allora verra' esportata **in chiaro** la chiave privata:

`openssl rsa -in keys.pem -out private.pem -outform PEM`

