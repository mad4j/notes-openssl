# Appunti sull'utilizzo di OpenSSL
----
daniele.olmlisani@gmail.com

* [Verificare la versione di OpenSSL in uso](#verificare-la-versione-di-openssl-in-uso)
* [Creazione di una coppia di chiavi privata/pubblica](#creazione-di-una-coppia-di-chiavi-privatapubblica)
* [Numeri casuali](#lavorare-con-i-numeri-casuali)

----

## Verificare la cassetta degli attrezzi

### Controllare la versione di OpenSSL in uso

#### Giusto un assaggio

Per ottenere una informazione sintetica sulla versione attualmente in uso:

`$ openssl version`

con il seguente risultato:

    OpenSSL 1.1.0h  27 Mar 2018

#### Qualcosa di piu' dettagliato

Per ottenere informazioni più dettagliate:

`$ openssl version -a`

con il seguente risultato:

    OpenSSL 1.0.0 29 Mar 2010
    built on: Wed Apr 14 18:16:40 EDT 2010
    platform: MSys
    options:  bn(64,32) rc4(4x,int) des(ptr,risc1,16,long) blowfish(idx)
    compiler: gcc -D_WINDLL -DOPENSSL_PIC -DOPENSSL_THREADS  -DDSO_DLFCN -DHAVE_DLFCN_H -DTERMIOS -DL_ENDIAN -D__CYGWIN__ -fomit-frame-pointer -O3 -march=i386 -mtune=i686 -Wall -DOPENSSL_BN_ASM_PART_WORDS -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT-DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DMD5_ASM -DRMD160_ASM -DAES_ASM -DWHIRLPOOL_ASM
    OPENSSLDIR: "/var/ssl"
    engines:  dynamic
    
Utilizzare il comando:

`$ openssl version -help`

per ottenere la lista di opzioni utilizzabili con il comando `version`:

    Usage: version [options]
    Valid options are:
     -help  Display this summary
     -a     Show all data
     -b     Show build date 
     -d     Show configuration directory
     -e     Show engines directory
     -f     Show compiler flags used
     -o     Show some internal datatype options
     -p     Show target build platform
     -v     Show library version
     
### Come ottenere aiuto

Richiedere l'help su linea di comando:

`$ openssl help`

Per ottenere il seguente risultato

```
Standard commands
asn1parse         ca                ciphers           cms
crl               crl2pkcs7         dgst              dhparam
dsa               dsaparam          ec                ecparam
enc               engine            errstr            exit
gendsa            genpkey           genrsa            help
list              nseq              ocsp              passwd
pkcs12            pkcs7             pkcs8             pkey
pkeyparam         pkeyutl           prime             rand
rehash            req               rsa               rsautl
s_client          s_server          s_time            sess_id
smime             speed             spkac             srp
ts                verify            version           x509

Message Digest commands (see the `dgst' command for more details)
blake2b512        blake2s256        gost              md4
md5               rmd160            sha1              sha224
sha256            sha384            sha512

Cipher commands (see the `enc' command for more details)
aes-128-cbc       aes-128-ecb       aes-192-cbc       aes-192-ecb
aes-256-cbc       aes-256-ecb       base64            bf
bf-cbc            bf-cfb            bf-ecb            bf-ofb
camellia-128-cbc  camellia-128-ecb  camellia-192-cbc  camellia-192-ecb
camellia-256-cbc  camellia-256-ecb  cast              cast-cbc
cast5-cbc         cast5-cfb         cast5-ecb         cast5-ofb
des               des-cbc           des-cfb           des-ecb
des-ede           des-ede-cbc       des-ede-cfb       des-ede-ofb
des-ede3          des-ede3-cbc      des-ede3-cfb      des-ede3-ofb
des-ofb           des3              desx              rc2
rc2-40-cbc        rc2-64-cbc        rc2-cbc           rc2-cfb
rc2-ecb           rc2-ofb           rc4               rc4-40
seed              seed-cbc          seed-cfb          seed-ecb
seed-ofb
```

### Indagare su cosa è possibile fare

Utilizzare la seguente sequenza di comandi per esplorare cosa sia possibile fare con la libreria

* `openssl -help` - ottenere informazioni di carattere generale
* `openssl list -help` - ottenere informazioni su quali sono gli argomenti di dettaglio
* `openssl list -commands` - ottenere la lista degli argomenti di dettaglio (es. *commands*)
* `openssl seed -help` - ottenere informazioni dettagliate su uno specifico argomento (es. comando *seed*)

Di seguito si possono vedere in dettagloio i risultati di queste operazioni.

Per ottenere la lista dei comandi disponibili:

`$ openssl list -help`

```
Usage: list [options]
Valid options are:
 -help                   Display this summary
 -commands               List of standard commands
 -digest-commands        List of message digest commands
 -digest-algorithms      List of message digest algorithms
 -cipher-commands        List of cipher commands
 -cipher-algorithms      List of cipher algorithms
 -public-key-algorithms  List of public key algorithms
 -disabled               List of disabled features
```

Per esempio, ottenere la lista dei comandi più comuni:

`$ openssl list -commands`
```
asn1parse         ca                ciphers           cms
crl               crl2pkcs7         dgst              dhparam
dsa               dsaparam          ec                ecparam
enc               engine            errstr            exit
gendsa            genpkey           genrsa            help
list              nseq              ocsp              passwd
pkcs12            pkcs7             pkcs8             pkey
pkeyparam         pkeyutl           prime             rand
rehash            req               rsa               rsautl
s_client          s_server          s_time            sess_id
smime             speed             spkac             srp
ts                verify            version           x509
```

Per avere informazioni più dettagliate su un singolo comando utilizzare (esempio per il comando help):

`$ openssl seed -help`

```
Usage: seed [options]
Valid options are:
 -help          Display this summary
 -ciphers       List ciphers
 -in infile     Input file
 -out outfile   Output file
 -pass val      Passphrase source
 -e             Encrypt
 -d             Decrypt
 -p             Print the iv/key
 -P             Print the iv/key and exit
 -v             Verbose output
 -nopad         Disable standard block padding
 -salt          Use salt in the KDF (default)
 -nosalt        Do not use salt in the KDF
 -debug         Print debug info
 -a             Base64 encode/decode, depending on encryption flag
 -base64        Same as option -a
 -A             Used with -[base64|a] to specify base64 buffer as a single line
 -bufsize val   Buffer size
 -k val         Passphrase
 -kfile infile  Read passphrase from file
 -K val         Raw key, in hex
 -S val         Salt, in hex
 -iv val        IV in hex
 -md val        Use specified digest to create a key from the passphrase
 -none          Don't encrypt
 -*             Any supported cipher
 -engine val    Use engine, possibly a hardware device
```

## Cifratura a chiave pubblica/privata con RSA

Questo capitolo mostra alcune delle operazioni più comuni che possono essere eseguite con l'algoritmo RSA.

## Creazione di una coppia di chiavi pubblica/privata per algoritmo RSA

Generare una coppia di chiavi di 2048 bits in formato PEM non cifrato:

`$ openssl genrsa -out keys.pem 2048`

```
Generating RSA private key, 2048 bit long modulus
.+++
..........................+++
e is 65537 (0x010001)
```

Questo è il contenuto del file generaro.

```
$ cat keys.pem

-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAy3zK+fEFMOMF2BkrBhxkVScjPuIxkEf7Ezi16t8DFPM6dbWX
xrVg2qqnCDiEhmjcn6wkFBPhZ6NqJ/4dNBUfAt3K4LVQvqMo5lfgiBtJqRuJMuDm
g20xtBiFSR4T7CWhh5uJLCaadBvimp7tKEvhivBMpMJgxchO6JW9xsFumbO8/0xt
DBBsq2+dZZFcNPF9MnkhkC9qgqfVgEu8iMsFyrXRMI8oE/rvwMmDlKJ+B25kf5Hi
9UpYbYdsglTWvye+C2q8rFZ1Or3iinXlaziLhNSxRXTRTa9AbfIRICkfUhPfD3K4
utzEFmwjm+UjFb58dzzUgtjeEg4urw5Yo6evaQIDAQABAoIBAAb/ncOG5JTP2g2a
n/4vz8uV8wJgqS+7Kgl5M9iGHwcDbolJ25R7/H7Iy8Hen8A3rw7Wzs7Z+DCmUCpb
9QkriMuCcU3VLpe/6NIzR4em2Ju2VLupPIRcpw74oOzo2eqPSMTvNoKMOVew9dQ3
jxSJt5IdvaUVAlsLDpu1OrexQ2qJ0kwC5ZGRby+QeWaFgoZ5Bjmi4iyAX9tuNQDx
Pp0le4q60kwplo/39IvBKZWBr8phW9pM6PV/jsA3EbtTr/nKP3lfVtydX4iaQ8/6
nOnS2auKAJlR9LlbwBGcbULdCnLR3chbrRcyi4DdjsNAly0Ty5t6XJOmxoolCU/D
fwNFOpkCgYEA6Z9XrG9TGOK9g9RgY8I+36Z2NDZWeKPUZN2fB2Dp5xNy7gyS8u0V
jSiT6wwjP7PQ7cPHzsLgIPckTPyaS4rZWKTdvtwtxfopkOpKnFEdTjkudE33u5vk
9En4HFvV08uRqQgHURJ81r/kH7LM04u9tO9dJLNxdiCM8rJfwc3LAD8CgYEA3vqC
xRo9lRcaBpm4O+oA50vUJ8x/LdxR6gf0w+BQzOrIkB5RwvHpMDMo81FhyycwobQ0
7KBMNE7iG2N+p6ha+Rei/6Kzr0FIoVQGYrh1P7/8/pO0u12wsb/5LPQ4V5SOaKiI
FrFGaBEhKGPUkiHbcuK87SPAM13ng60WTzIO5lcCgYBCRpH4uRw50xkwbO9rXY+4
ouClr0SYtOFsO/MBhNzWUQ4puid2Aww6H2jXJXpaeAitav0kCuTKdVI3BPJAvAMs
wpilrJDPuUhRdCX2cox9xHsJQ5UkA/XP16wrX2Ip03ZfHYf11+jSg12UIIU6/kmz
KrSh1MY7XMCfmTrRIDCWwQKBgQC+6H70IK5fKsNWqWAqtAQIBE1lsdmluz/K/vra
hgDIIbCMa9kW7qX3ZaYHBUOUez5RBIhXnDsCghNm3b2/8A3LvSgKdRQg1eVIczdw
aHvClC8ZC/+ud93jofjGO2bN3Vw8UScLPsaILTpVaXvDjgdkiqq04moZ0Kl6fMRg
E/zbywKBgCU3xAaBeb1uLt7hc5zwCyer4x/X4RBq3kPtJ+e+PnIBjZVLhLJRWfwB
oSGQQZcVYUeVlQkkKpunMlw8LSf5YnbwO+MQS30y9/kGCKTsx+TFihwON8jJDydS
03MkCMx7+V1ARQKH/DSPNPlVDIGbsAZH9FhtO8SH91zDnvuA2xik
-----END RSA PRIVATE KEY-----
```

Generare una coppia di chiavi di 2048 bits in formato PEM (cifrato usando AES-256 CBC):

`$ openssl genrsa -aes256 -out keys.pem 2048`

```
Generating RSA private key, 2048 bit long modul
..............................................+++
......+++
e is 65537 (0x010001)
Enter pass phrase for keys.pem:
Verifying - Enter pass phrase for keys.pem:
```

Questo è il contenuto del file generato.

```
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-256-CBC,68F49455F4CB3D02925F696168E95317

Mxw9iwkH64kvu03UfMJDquNCsJ+EtAGhDCbWLcrvP6rkPPkBKiXzD0PxopMMPoR8
lu7MHIDm2yIwLpZc102vTSwPu0ELMP05zYu+Dbt+PkzlV5h9osFru9y4yyRYqqKB
8pLXGrPCqgKHRgWr607U0Omisc3OTY3GsqT1RFUUJKxdyCE9256UYD9DobQFSbAs
MiTdgV9UZVyLMdT53Jb+J6jf3IjwAvPkt8He95sI2RXfUPXJ9YO1lafqvnrTlp5/
MhNffb9Y837CWYaBbCgA5JYvlfPw6J4+biCZspaSraf1YbZHqzcuN2IcAleto7lK
myJEIx6b9gYOB88D9H+cx6/beXMzslcB1kn9C6N0wafvH3eYX54WFjZbIZy/BA0m
RAhqpcCE2QNXOdtd3aLKGwRrd9QmZVdpjTiBDGICDeRdr9pbfhMxKoMGCg/6RIDu
yOA58sbFDTq9XB3wZZ+z6vmMpTsTSycKnfrtTHsGjM1N4AA79ZQFrmEpp5NqSJnv
aMXg6I3dI4R/YDou0FWdrLHlOXDq1ZZLCfK1tTrCrIZkdCz7eFrQT+qI5mel+90U
VTbF6IzjUNKH9Y+xITy7N7cqRZPkXhZC9l0I4me7/kLu28ZVgUH9D3uzxhbaA5Vq
p00zxLVbOKb29BIWZ80JJFiEcLzIg7Eh/0Pm6LAzbcBiZdgWH7Kb1nSWfEjXCAvm
Z65mFXWwtd8aFyZLm4p9fgRE20yGrFK/De1Z3f2it5PVoa+2H6371nfqYrJ6e6oO
QaE5xUjsZHF/StgM6j7neJ4NKNY9s52KqR3slXd5cApU1Yuae7LXwhGc4Rn6cvw/
TtQ7xQ2kMARMuFnkdjw5SkU01RvVZHUJvi02ZwG7h6boOPIq4Hasd3E8jkA5YOCQ
2gFYbrYIVfjeGTN6yCrWICUHgv+1Tpu/0Ykv13ES8MjvA/PATfD3p2r3bUiEy3Yy
NKuDUXWYuZeprTcMMn/yuHXUaLGwjbz+6isUT6Rh6bBN90l0rE7zQM0gSZtJTnEM
oAWoiNnAZwGys+V1rMzjRUn3jWuPft+/EvelHfQjvRuocORyJ9DeGhxFNJFHpn9y
pAuNjS2LGK/d8xOCaA/LxYCUW0lmuITaoqAkiUrU3URvgw/YCsRqyKR3AM0UF4S4
wsd+TjLi/aYTvOgxlfPtyo6Y7GFbYgj02pS3cMjwKJP+iUzxe5FpMRamyF7MBoV1
ZhSs3sKkUhBPfh96ATGBlPrkSqjG6+rGNMHXUMbQyiDHAs7izyyAdsXg6Dlndh8r
AZ/Vj0qzWm0zSIf39XkyIkTlqPbm1o8mV08gDbcdO6GZjACF+D34uR3YcOYTFIFP
a+j1Ds5ZucY34xBaSjrmQ8I51m0PDmo9SX2fNGdYBle9QREkpyml1eTzt2daayuc
NWDDNRBn9xoeLln4krOhvBFJkHJ/RTx5+zbv7lpCWIzerr9WcCRNAUPCMNgxxBgL
3HSr69rFtICZV98t1pOvh59U0s1oldQUGgdGA0wQxS/19iM9mCCoeC7BcuYss7ag
2sjtLPAKQOLuiu+f6UvCJVIGza0gC2/RGP2zlRT+fOzQSYP3mj2KiII3117T0c6x
-----END RSA PRIVATE KEY-----
```

viene richiesto di inserire una password per la cifratura del file.

Visualizzare opzioni riguardanti la generazione di chiavi private/pubbliche

`$ openssl genrsa -h`


## Esportare una chiave pubblica

`openssl rsa -in keys.pem -outform PEM -pubout -out public.pem`

Se il file in input `keys.pem` e' cifrato verra' richiesta la password di decifratura.

Se non viene specificato il parametro `-pubout` allora verra' esportata **in chiaro** la chiave privata:

`openssl rsa -in keys.pem -out private.pem -outform PEM`


## Generazione/Verifica firme digitali con algoritmi EC (Elliptic Curves)

Questa sezione descrive come generare e verificare una firma digitale utilizzando algoritmi EC.


### Generazione di una chiave privata

Generazione di una chiave privata **in chiaro** utilizzando una delle curve predefinite `prime256v1`:

`$ openssl ecparam -out key.pem -name prime256v1 -genkey`

```
$ cat key.pem
-----BEGIN EC PARAMETERS-----
BggqhkjOPQMBBw==
-----END EC PARAMETERS-----
-----BEGIN EC PRIVATE KEY-----
MHcCAQEEIJcavj9RiSReqoSvb8Flf1WaFpoYVQWCJEBYY4NcMh2QoAoGCCqGSM49
AwEHoUQDQgAEzaBK/N8W3mKbOAV2FiZnIDArLfixEKMdAZZ+qIKgaoNVKJwO+q7p
/MCuCLv9cnu/7OxIQX31lR4ZA/fBxB1How==
-----END EC PRIVATE KEY-----
```

E' possibile avere la lista delle curve predefinite attraverso questo comando:

`$ openssl ecparam -list_curves`

```
  secp112r1 : SECG/WTLS curve over a 112 bit prime field
  secp112r2 : SECG curve over a 112 bit prime field
  secp128r1 : SECG curve over a 128 bit prime field
  secp128r2 : SECG curve over a 128 bit prime field
  secp160k1 : SECG curve over a 160 bit prime field
  secp160r1 : SECG curve over a 160 bit prime field
  secp160r2 : SECG/WTLS curve over a 160 bit prime field
  secp192k1 : SECG curve over a 192 bit prime field
  secp224k1 : SECG curve over a 224 bit prime field
  secp224r1 : NIST/SECG curve over a 224 bit prime field
  secp256k1 : SECG curve over a 256 bit prime field
  secp384r1 : NIST/SECG curve over a 384 bit prime field
  secp521r1 : NIST/SECG curve over a 521 bit prime field
  prime192v1: NIST/X9.62/SECG curve over a 192 bit prime field
  prime192v2: X9.62 curve over a 192 bit prime field
  prime192v3: X9.62 curve over a 192 bit prime field
 ...
 ```
 
### Esportazione della chiave pubblica

Da un certifcato contentene una chiave privata e' possibile estrarre solo la chiave pubblica utilizzando il seguente comando:

`$ openssl ec -in key.pem -pubout -out pubkey.pem`

Questo è il contenuto del file generato.

```
cat pubkey.pem
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEzaBK/N8W3mKbOAV2FiZnIDArLfix
EKMdAZZ+qIKgaoNVKJwO+q7p/MCuCLv9cnu/7OxIQX31lR4ZA/fBxB1How==
-----END PUBLIC KEY-----
```


### Firma di un file tramite chiava privata

E' possibile generare la firma di un file di dati partendo da una chiave privata usando il seguente comando:

`$ openssl dgst -sha256 -sign key.pem -out tobesigned.sign tobesigned.txt`


### Verifica di una firma tramite chiave pubblica

E' possibile verificare la firma associata ad un file di dati utilizzando una chiave pubblica usando il seguente comando:

`$ openssl dgst -sha256 -verify pubkey.pem -signature tobesigned.sign tobesigned.txt`


## Lavorare con i numeri casuali

### Generare un numero casuale

E' possibile generare un numero casuale di byte (es. 20 bytes) e leggere il risultato direttamente sull'output della console:

`$ openssl rand -hex 20`

    3c45bd4c3046df2f3bd1d143fae3b04006d31e90

Nello stesso modo e' possibile generare un numero, ma facendo codificare il risultato in base64:

`openssl rand -base64 20`

    RX2WbgHpXNkb4OnGwN1wTdufF/U=

E' possibile indicare il nome del file dove memorizzare il numero salvato. Molto utile nel caso si voglia memorizzare le cifre in fomrato binario (e.g. senza specificare i parametri `-hex` oppure `-base64`):

`$ openssl rand -out rand.txt 20`

Il numero generato e' contenuto nel file `rand.txt`:

    $ cat rand.txt
    ♀/öìÈ±9ÌÐ8À↕Xk%ºÔ¶Ã"

### Ottenere informazioni sul comando RAND

`$ openssl rand -help`

    Usage: rand [options] num


### Risolvere l'errore "unable to write 'random state'"

Fare riferimento a:
* https://www.openssl.org/docs/faq.html#USER1
