# Nivell 3 - Exercici 1 

## Creació de la taula

### Enunciat

Crea una taula amb la qual puguem unir les dades del nou arxiu products.csv amb la base de dades creada, tenint en compte que des de transaction tens product_ids.

---

### Descripció

En aquesta part es crea una taula intermèdia anomenada `transaction_products` que permet relacionar les transaccions amb els productes.

Aquesta taula resol la relació de molts a molts (N:M) entre `transaction` i `products`, ja que una transacció pot tenir múltiples productes.

---

### Resolució

```sql
CREATE TABLE transaction_products (
    transaction_id VARCHAR(50),
    product_id INT,
    PRIMARY KEY (transaction_id, product_id),
    FOREIGN KEY (transaction_id) REFERENCES transaction(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

---

## Inserció de dades

```sql
USE transactions;

INSERT INTO transaction_products (transaction_id, product_id) VALUES
('00043A49-2949-494B-A5DD-A5BAE3BB19DD', 16),
('00043A49-2949-494B-A5DD-A5BAE3BB19DD', 26),
('00043A49-2949-494B-A5DD-A5BAE3BB19DD', 97),
('00043A49-2949-494B-A5DD-A5BAE3BB19DD', 87),
('000447FE-B650-4DCF-85DE-C7ED0EE1CAAD', 66),
('000447FE-B650-4DCF-85DE-C7ED0EE1CAAD', 69),
('000447FE-B650-4DCF-85DE-C7ED0EE1CAAD', 87),
('000481C3-1C26-4FEF-83A0-4CD0EB004BBD', 72),
('00051AA4-9CBE-4268-B070-C38062A1B3E2', 18);
```

---

## Exercici 1

### Enunciat

Necessitem conèixer el nombre de vegades que s'ha venut cada producte.

---

### Descripció

En aquest exercici es calcula el nombre de vegades que s'ha venut cada producte utilitzant la taula intermèdia `transaction_products`.

Es relaciona amb la taula `products` per obtenir el nom del producte i es fa un recompte del nombre de vendes.

---

### Resolució

```sql
USE transactions;

SELECT
    p.id,
    p.product_name,
    COUNT(*) AS vegades_venut
FROM transaction_products tp
JOIN products p ON tp.product_id = p.id
GROUP BY p.id, p.product_name;
```

---

## Conclusió

S’ha creat una taula intermèdia per gestionar correctament la relació entre transaccions i productes, i s’ha pogut calcular el nombre de vendes de cada producte de forma eficient.
