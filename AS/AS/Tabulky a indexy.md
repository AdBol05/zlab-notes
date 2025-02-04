- ## Rychlejší vyhledávání

- **Vytvoření indexu na tabulce**
```SQL
CREATE INDEX members_name_i ON members(last_name);
```

- **Smazání indexu**
```sql
DROP INDEX members_last_name_i;
```


# UNIQUE index
- ## Unikátní index
```sql
CREATE UNIQUE INDEX members_email_i ON members(email);

CREATE UNIQUE INDEX unq_idx_demo_ab_i ON unq_idx_demo(a,b);
```

