# Transações

## 6. Atividade prática

### Atividade: simular transferência bancária e concorrência

#### Etapa 1. Criar o banco de teste

```sql
DROP TABLE IF EXISTS contas;

CREATE TABLE contas (
    id INT PRIMARY KEY,
    titular VARCHAR(100),
    saldo DECIMAL(10,2)
);

INSERT INTO contas (id, titular, saldo) VALUES
(1, 'Ana', 1000.00),
(2, 'Bruno', 500.00),
(3, 'Carlos', 300.00);

SELECT * FROM contas;
```

---

#### Etapa 2. Testar COMMIT

Execute:

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 100
WHERE id = 1;

UPDATE contas
SET saldo = saldo + 100
WHERE id = 2;

COMMIT;
```

Depois:

```sql
SELECT * FROM contas;
```

**Pergunta**  
O que aconteceu com os saldos após o `COMMIT`?

---

#### Etapa 3. Testar ROLLBACK

Execute:

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 50
WHERE id = 2;

UPDATE contas
SET saldo = saldo + 50
WHERE id = 3;

ROLLBACK;
```

Depois:

```sql
SELECT * FROM contas;
```

**Pergunta**  
Por que os valores não foram alterados ao final?

---

#### Etapa 4. Testar lock com duas sessões

Abra duas conexões.

### Sessão 1

```sql
START TRANSACTION;

SELECT * FROM contas WHERE id = 1 FOR UPDATE;

UPDATE contas
SET saldo = saldo - 200
WHERE id = 1;
```

Não execute `COMMIT` ainda.

### Sessão 2

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo + 300
WHERE id = 1;
```

**Observe**  
A segunda sessão pode ficar bloqueada esperando a primeira terminar.

Agora volte para a Sessão 1 e faça:

```sql
COMMIT;
```

Depois finalize a Sessão 2.

**Perguntas**

- O que aconteceu com a segunda transação?
- Por que ela precisou esperar?
- Qual a função do `FOR UPDATE`?

---

## 7. Atividade dissertativa

### Questão 1
Explique o que é uma transação em banco de dados.

### Questão 2
Descreva a diferença entre `COMMIT` e `ROLLBACK`.

### Questão 3
Explique por que uma transferência bancária deve ser tratada como transação.

### Questão 4
O que pode acontecer se duas transações alterarem o mesmo dado ao mesmo tempo sem controle de concorrência?

### Questão 5
Qual a relação entre transações e as propriedades ACID?

---

## 8. Atividade prática com enunciado formal

### Enunciado
Uma instituição bancária deseja garantir integridade nas operações de transferência entre contas. Para isso, implemente testes em SQL que demonstrem:

- confirmação de transação com `COMMIT`
- cancelamento com `ROLLBACK`
- bloqueio de registro em acesso concorrente com `FOR UPDATE`