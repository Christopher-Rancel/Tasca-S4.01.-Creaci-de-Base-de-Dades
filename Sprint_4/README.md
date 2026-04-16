# Tasca S4.01 - Creació de base de dades

## Descripció

En aquest sprint es treballa la creació d’una base de dades relacional a partir de diferents arxius CSV utilitzant MySQL.

L’activitat consisteix en dissenyar un model de dades estructurat, crear les taules necessàries, importar les dades i realitzar consultes per extreure informació rellevant.

Es treballa amb una base de dades que conté informació sobre usuaris, transaccions, targetes de crèdit, empreses i productes.

---

## Estructura de la base de dades

El projecte treballa amb les següents taules:

- `data_user` → informació dels usuaris  
- `transaction` → registres de transaccions  
- `credit_card` → informació de targetes de crèdit  
- `company` → informació de les empreses  
- `products` → informació dels productes  
- `transaction_products` → relació entre transaccions i productes  

Les taules estan relacionades mitjançant claus primàries i claus foranes:

- `transaction.user_id → data_user.id`  
- `transaction.card_id → credit_card.id`  
- `transaction.business_id → company.company_id`  
- `credit_card.user_id → data_user.id`  
- `transaction_products.transaction_id → transaction.id`  
- `transaction_products.product_id → products.id`  

---

## Nivell 1

### Exercici 1
Creació de la base de dades, definició de les taules i importació de dades des de fitxers CSV.

### Exercici 2
Consulta per obtenir la mitjana d’import (`amount`) per IBAN de les targetes de crèdit d’una empresa concreta.

---

## Nivell 2

### Exercici 1
Creació d’una taula derivada per determinar l’estat de les targetes de crèdit segons les seves últimes transaccions i càlcul del nombre de targetes actives.

---

## Nivell 3

### Exercici 1
Creació d’una taula intermèdia per relacionar transaccions i productes a partir del camp `product_ids`.

### Exercici 2
Consulta per calcular el nombre de vegades que s’ha venut cada producte.

---

## Objectiu del projecte

L’objectiu d’aquesta activitat és consolidar els coneixements sobre creació i explotació de bases de dades relacionals, incloent:

- Disseny de models de dades  
- Creació de taules (`CREATE TABLE`)  
- Importació de dades (`LOAD DATA INFILE`)  
- Normalització (1NF)  
- Gestió de claus foranes i integritat referencial  
- Consultes amb `JOIN`  
- Subconsultes i funcions d’agregació (`COUNT`, `AVG`)  

Aquest projecte forma part del procés d’aprenentatge en anàlisi de dades i bases de dades relacionals.
