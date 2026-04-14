# Processamento de Transações em Banco de Dados

## 1. Exemplo de transação

Imagine uma tabela de contas bancárias:

```sql
CREATE TABLE contas (
    id INT PRIMARY KEY,
    titular VARCHAR(100),
    saldo DECIMAL(10,2)
);
```

Inserindo dados:

```sql
INSERT INTO contas (id, titular, saldo) VALUES
(1, 'Ana', 1000.00),
(2, 'Bruno', 500.00);
```

Agora uma transferência de R$ 200 da Ana para o Bruno.

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 200
WHERE id = 1;

UPDATE contas
SET saldo = saldo + 200
WHERE id = 2;

COMMIT;
```

Resultado esperado:

- Ana: 800
- Bruno: 700

Consulta:

```sql
SELECT * FROM contas;
```

---

## 2. Exemplo com erro e ROLLBACK

Agora pense que houve erro no meio da operação. A transferência não pode ficar pela metade.

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 300
WHERE id = 1;

-- aqui você decide cancelar
ROLLBACK;
```

Depois:

```sql
SELECT * FROM contas;
```

O saldo volta ao valor anterior.

---

## 3. Exemplo com regra de negócio

Não permitir saldo negativo.

```sql
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 2000
WHERE id = 1;
```

Antes de confirmar, você consulta:

```sql
SELECT * FROM contas WHERE id = 1;
```

Se perceber que ficou negativo, desfaz:

```sql
ROLLBACK;
```

Se estiver correto:

```sql
COMMIT;
```

---

## 4. Exemplo de concorrência

Suponha que duas pessoas tentem alterar o mesmo saldo ao mesmo tempo.

Tabela:

```sql
CREATE TABLE estoque (
    id INT PRIMARY KEY,
    produto VARCHAR(100),
    quantidade INT
);
```

Dados:

```sql
INSERT INTO estoque (id, produto, quantidade) VALUES
(1, 'Notebook', 10);
```

### Sessão 1

```sql
START TRANSACTION;

SELECT * FROM estoque WHERE id = 1 FOR UPDATE;

UPDATE estoque
SET quantidade = quantidade - 2
WHERE id = 1;
```

Sem dar `COMMIT` ainda.

### Sessão 2

```sql
START TRANSACTION;

UPDATE estoque
SET quantidade = quantidade - 3
WHERE id = 1;
```

A segunda sessão pode ficar esperando, dependendo do lock. Isso mostra como o banco controla acesso simultâneo.

Quando a Sessão 1 fizer:

```sql
COMMIT;
```

a Sessão 2 prossegue.

---

## 5. Exemplo de problema sem controle adequado

Considere saldo inicial = 1000.

Duas transações leem o mesmo valor ao mesmo tempo:

- Transação A lê 1000
- Transação B lê 1000

A subtrai 100 e grava 900.  
B subtrai 200 e grava 800.

Resultado final: 800

Mas o correto seria: 700

Isso é um exemplo de **lost update**.

---