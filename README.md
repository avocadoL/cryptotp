# TP3: OpenSSL - Gestion de certificats, chiffrement et hachage de données
**Nom:** Lefghih
**Prénom:** Mohamed Elmoctar
**Filière:** IDLD

## Fichiers de vérification
Les fichiers suivants sont fournis pour la vérification du TP:
- `fichier_enc.txt` - Fichier d'entrée original
- `cle_privee.pem` - Clé privée RSA générée
- `cle_publique.pem` - Clé publique extraite
- `certificat.pem` - Certificat auto-signé
- `fichier_chiffre.enc` - Fichier chiffré avec AES-256-CBC
- `fichier_dechiffre.txt` - Fichier déchiffré
- `signature.bin` - Signature numérique du fichier

## Exercices réalisés

### Exercice 1: Génération de clés et de certificats

1. Génération d'une clé privée RSA:
```bash
openssl genrsa -out cle_privee.pem 2048
```

2. Extraction de la clé publique:
```bash
openssl rsa -in cle_privee.pem -pubout -out cle_publique.pem
```

3. Vérification de la clé privée:
```bash
openssl rsa -check -in cle_privee.pem
```

4. Génération d'une demande de certificat (CSR):
```bash
openssl req -new -key cle_privee.pem -out demande_certificat.csr
```
Informations fournies:
- Pays (C): MA
- Région (ST): Rabat-Salé-Kénitra
- Ville (L): Rabat
- Organisation (O): Université Mohammed V
- Unité organisationnelle (OU): Faculté des Sciences
- Nom commun (CN): www.exemple.ma
- Email: admin@exemple.ma

5. Auto-signature du certificat:
```bash
openssl x509 -req -days 365 -in demande_certificat.csr -signkey cle_privee.pem -out certificat.pem
```

6. Vérification du certificat:
```bash
openssl x509 -in certificat.pem -text -noout
```

### Exercice 2: Chiffrement et déchiffrement de fichiers

1. Chiffrement du fichier avec AES-256-CBC:
```bash
openssl enc -aes-256-cbc -salt -pbkdf2 -in fichier_enc.txt -out fichier_chiffre.enc -pass pass:motdepasse
```

2. Déchiffrement du fichier:
```bash
openssl enc -d -aes-256-cbc -pbkdf2 -in fichier_chiffre.enc -out fichier_dechiffre.txt -pass pass:motdepasse
```

### Exercice 3: Hachage de fichiers

1. Calcul du haché SHA-256:
```bash
openssl dgst -sha256 fichier_enc.txt
```

2. Comparaison des hachés:
```bash
openssl dgst -sha256 fichier_enc.txt
openssl dgst -sha256 fichier_dechiffre.txt
```

### Exercice 4: Signature Numérique

1. Signature du fichier:
```bash
openssl dgst -sha256 -sign cle_privee.pem -out signature.bin fichier_enc.txt
```

2. Vérification de la signature:
```bash
openssl dgst -sha256 -verify cle_publique.pem -signature signature.bin fichier_enc.txt
``` 