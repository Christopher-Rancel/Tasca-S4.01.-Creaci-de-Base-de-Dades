# Nivell 1 - Exercici 1

## Enunciat

Descàrrega els arxius CSV, estudia'ls i dissenya una base de dades amb un esquema d'estrella que contingui, almenys 4 taules de les quals puguis realitzar les següents consultes:

---

## Descripció

En aquest exercici he creat una base de dades relacional anomenada `transactions`, definint totes les taules necessàries i carregant les dades des de fitxers CSV.

Finalment, s'ha realitzat una consulta per identificar els usuaris amb més de 80 transaccions.

---

## Creació de les taules

```sql
CREATE TABLE company (
    company_id VARCHAR(20) PRIMARY KEY,
    company_name VARCHAR(100),
    phone VARCHAR(50),
    email VARCHAR(100),
    country VARCHAR(50),
    website VARCHAR(100)
);

CREATE TABLE data_user (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    surname VARCHAR(100),
    phone VARCHAR(50),
    email VARCHAR(100),
    birth_date DATE,
    country VARCHAR(50),
    city VARCHAR(50),
    postal_code VARCHAR(20),
    address VARCHAR(150)
);

CREATE TABLE credit_card (
    id VARCHAR(20) PRIMARY KEY,
    user_id INT,
    iban VARCHAR(50),
    pan VARCHAR(50),
    pin INT,
    cvv INT,
    track1 VARCHAR(150),
    track2 VARCHAR(150),
    expiring_date DATE,
    FOREIGN KEY (user_id) REFERENCES data_user(id)
);

CREATE TABLE products (
    id INT PRIMARY KEY,
    product_name VARCHAR(100),
    price DECIMAL(10,2),
    colour VARCHAR(50),
    weight DECIMAL(10,2),
    warehouse_id INT
);

CREATE TABLE transaction (
    id VARCHAR(50) PRIMARY KEY,
    card_id VARCHAR(20),
    business_id VARCHAR(20),
    timestamp DATETIME,
    amount DECIMAL(10,2),
    declined BOOLEAN,
    product_ids VARCHAR(255),
    user_id INT,
    lat DECIMAL(10,6),
    longitude DECIMAL(10,6),
    FOREIGN KEY (card_id) REFERENCES credit_card(id),
    FOREIGN KEY (business_id) REFERENCES company(company_id),
    FOREIGN KEY (user_id) REFERENCES data_user(id)
);
```

---

## Càrrega de dades

```sql
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/companies.csv'
INTO TABLE company
FIELDS TERMINATED BY ','
IGNORE 1 ROWS
(company_id, company_name, phone, email, country, website);

LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/european_users.csv'
INTO TABLE data_user
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
IGNORE 1 ROWS
(id, name, surname, phone, email, @birth_date, country, city, postal_code, address)
SET birth_date = STR_TO_DATE(@birth_date, '%b %e, %Y');

LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/american_users.csv'
INTO TABLE data_user
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
IGNORE 1 ROWS
(id, name, surname, phone, email, @birth_date, country, city, postal_code, address)
SET birth_date = STR_TO_DATE(@birth_date, '%b %e, %Y');

LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/credit_cards.csv'
INTO TABLE credit_card
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
IGNORE 1 ROWS
(id, user_id, iban, pan, pin, cvv, track1, track2, @expiring_date)
SET expiring_date = STR_TO_DATE(@expiring_date, '%m/%d/%y');

LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/products.csv'
INTO TABLE products
FIELDS TERMINATED BY ','
IGNORE 1 ROWS
(id, product_name, @price, colour, weight, warehouse_id)
SET price = REPLACE(@price, '$', '');

LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/transactions.csv'
INTO TABLE transaction
FIELDS TERMINATED BY ';'
ENCLOSED BY '"'
IGNORE 1 ROWS
(id, card_id, business_id, @timestamp, amount, declined, product_ids, user_id, lat, longitude)
SET timestamp = STR_TO_DATE(@timestamp, '%Y-%m-%d %H:%i:%s');
```

---

## Validació de dades

```sql
SELECT COUNT(*) FROM transaction;
SELECT COUNT(*) FROM data_user;
SELECT COUNT(*) FROM credit_card;
SELECT COUNT(*) FROM products;
SELECT COUNT(*) FROM company;
```

---

## Exercici 1 – Consulta

### Enunciat

Mostra els usuaris amb més de 80 transaccions utilitzant almenys 2 taules.

---

### Resolució

```sql
USE transactions;

SELECT u.id, u.name
FROM data_user u
WHERE u.id IN (
    SELECT t.user_id
    FROM transaction t
    GROUP BY t.user_id
    HAVING COUNT(*) > 80
);
```

---

## Conclusió

S'ha creat correctament la base de dades, s'han carregat les dades des de fitxers CSV i s'ha validat la seva integritat.

Finalment, s'ha aplicat una subconsulta per identificar els usuaris amb més de 80 transaccions.
