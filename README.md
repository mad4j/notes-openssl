# Ricette per OpenSSL
simple tasks using openssl library

## Verificare la versione di OpenSSL in uso

Per ottenere una infomrazione sintetica sulla versione attualmente installata:

`$ openssl version`

Per ottenere informazionei piu' dettagliate:

`$ openssl version -a`

## Creazione di una coppia di chiavi privata/pubblica

Generare una coppia di chiavi di 2048 bits in formato PEM (non cifrato):

`$ openssl genrsa -out kes.pem 20148`

Generare una coppia di chiavi di 2048 bits in formato PEM (cifrato usando AES256 CBC):

`$ openssl genrsa -aes256 -out keys.pem 20148`

viene richiesto di inserire una password per la cifratura del file.

Visualizzare opzioni riguardanti la generazione di chiavi private/pubbliche

`$ openssl genrsa help`


## Esportare una chiave pubblica

`openssl rsa -in keys.pem -outform PEM -pubout -out public.pem`

Se il file in input `keys.pem` e' cifrato verra' richiesta la password di decifratura.

Se non viene specificato il parametro `-pubout` allora verra' esportata **in chiaro** la chiave privata:

`openssl rsa -in keys.pem -out private.pem -outform PEM`

