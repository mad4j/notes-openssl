# Ricette per OpenSSL
simple tasks using openssl library

## Verificare la versione di OpenSSL in uso

`$ openssl version`

## Creazione di una coppia di chiavi privata/pubblica

Generare una coppia di chiavi di 2048 bits in formato PEM (non cifrato)

`$ openssl genrsa -out kes.pem 20148`

Generare una coppia di chiavi di 2048 bits in formato PEM (cifrato usando AES256 CBC).
Viene richiesto di inserire una password per la cifratura del file.

`$ openssl genrsa -aes256 -out kes.pem 20148`

Visualizzare opzioni riguardanti la generazione di chiavi private/pubbliche

`$ openssl genrsa help`
