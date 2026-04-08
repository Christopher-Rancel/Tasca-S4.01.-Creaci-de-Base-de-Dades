# Nivell 2 - Exercici  1

## Creació de la taula

### Enunciat

Crea una nova taula que reflecteixi l'estat de les targetes de crèdit basat en si les tres últimes transaccions han estat declinades aleshores és inactiu, si almenys una no és rebutjada aleshores és actiu.

---

### Descripció

En aquesta part es crea una taula anomenada `card_status` que permet determinar l’estat de cada targeta de crèdit en funció de les seves últimes tres transaccions.

Per fer-ho, es fa ús de funcions de finestra (`ROW_NUMBER()`) per ordenar les transaccions per cada targeta segons la data i seleccionar les més recents.

A partir d’aquestes dades, es classifica cada targeta com a `ACTIVE` o `INACTIVE`.

---

### Resolució

```sql
CREATE TABLE card_status AS
WITH ranked_transactions AS (
    SELECT
        t.card_id,
        t.declined,
        ROW_NUMBER() OVER (
            PARTITION BY t.card_id
            ORDER BY t.timestamp DESC
        ) AS rn
    FROM transaction t
)
SELECT
    card_id,
    CASE
        WHEN SUM(declined) = 3 THEN 'INACTIVE'
        ELSE 'ACTIVE'
    END AS status
FROM ranked_transactions
WHERE rn <= 3
GROUP BY card_id;
```

---

## Exercici 1

### Enunciat

Quantes targetes estan actives?

---

### Descripció

En aquest exercici es fa una consulta sobre la taula `card_status` per comptar el nombre de targetes que tenen estat `ACTIVE`.

---

### Resolució

```sql
USE transactions;

SELECT COUNT(*) AS active_cards
FROM card_status
WHERE status = 'ACTIVE';
```

---

## Conclusió

S’ha creat una taula derivada que permet classificar les targetes segons el seu comportament recent i s’ha pogut obtenir el nombre de targetes actives de forma senzilla.
