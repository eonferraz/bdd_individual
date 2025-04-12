## Exercício 1 – MAP REDUCE

### Enunciado
Considerando Cluster HDFS contendo 3 slaves.  
Cada 5 linhas serão gravadas como um bloco no cluster.  
Não considerar replicação dos blocos no cluster.

#### SEQ.LANMENTOS
| SEQ | CLIENTE | AG CC | VL LANC |
|-----|---------|--------|---------|
| 1   | Cli001  | c0015  | 100     |
| 2   | Cli002  | c0031  | 1500    |
| 3   | Cli002  | c0032  | 800     |
| 4   | Cli003  | c0059  | 250     |
| 5   | Cli001  | C0015  | 300     |
| 6   | Cli003  | C0059  | 120     |
| 7   | Cli004  | c0012  | 140     |
| 8   | Cli005  | C0044  | 3200    |
| 9   | Cli005  | C0044  | 800     |
| 10  | Cli005  | C0044  | 10      |
| 11  | Cli004  | c0012  | 120     |
| 12  | Cli004  | c0012  | 320     |
| 13  | Cli002  | C0032  | 1200    |
| 14  | Cli001  | C0015  | 300     |
| 15  | Cli003  | c0059  | 400     |

### Distribuição dos blocos no cluster
- **Bloco 1 (Slave 1):** linhas 1–5  
- **Bloco 2 (Slave 2):** linhas 6–10  
- **Bloco 3 (Slave 3):** linhas 11–15

### Etapas do MapReduce – Saldo Médio por Cliente

#### MAP  
Cada linha gera um par chave-valor no formato `(Cliente, Valor)`:

| Seq | Cliente | Valor | Saída MAP        |
|-----|---------|--------|------------------|
| 1   | Cli001  | 100    | (Cli001, 100)    |
| 2   | Cli002  | 1500   | (Cli002, 1500)   |
| 3   | Cli002  | 800    | (Cli002, 800)    |
| 4   | Cli003  | 250    | (Cli003, 250)    |
| 5   | Cli001  | 300    | (Cli001, 300)    |

#### COMBINE (Local – por Slave)

**Bloco 1 - Slave 1**

| Cliente | Soma Parcial | Qtde |
|---------|---------------|------|
| Cli001  | 400           | 2    |
| Cli002  | 2300          | 2    |
| Cli003  | 250           | 1    |

**Bloco 2 - Slave 2**

| Cliente | Soma Parcial | Qtde |
|---------|---------------|------|
| Cli003  | 120           | 1    |
| Cli004  | 140           | 1    |
| Cli005  | 4010          | 3    |

**Bloco 3 - Slave 3**

| Cliente | Soma Parcial | Qtde |
|---------|---------------|------|
| Cli004  | 440           | 2    |
| Cli002  | 1200          | 1    |
| Cli001  | 300           | 1    |
| Cli003  | 400           | 1    |

#### SHUFFLE & SORT (Antes do REDUCE)

| Cliente | Pares Recebidos no Reducer       |
|---------|----------------------------------|
| Cli001  | [(400, 2), (300, 1)]             |
| Cli002  | [(2300, 2), (1200, 1)]           |
| Cli003  | [(250, 1), (120, 1), (400, 1)]   |
| Cli004  | [(140, 1), (440, 2)]             |
| Cli005  | [(4010, 3)]                      |

#### REDUCE

| Cliente | Soma Total | Qtde Total | Saldo Médio       |
|---------|-------------|--------------|-------------------|
| Cli001  | 700         | 3            | 700 / 3 ≈ 233.33  |
| Cli002  | 3500        | 3            | 3500 / 3 ≈ 1166.67|
| Cli003  | 770         | 3            | 770 / 3 ≈ 256.67  |
| Cli004  | 580         | 3            | 580 / 3 ≈ 193.33  |
| Cli005  | 4010        | 3            | 4010 / 3 ≈ 1336.67|
