
## Exercício 4 – Ponto Eletrônico

### Enunciado
O projeto final tem como objetivo a estruturação e segmentação de arquivos de ponto eletrônico, conforme os requisitos definidos pela Portaria 1510/2009 do Ministério do Trabalho e Emprego. A partir dos arquivos fornecidos (AFDT, ACJEF e ADS), foi realizada uma abordagem completa de engenharia de dados, envolvendo a extração, interpretação e normalização das informações. O resultado é a modelagem em um banco de dados relacional, adequada para suportar análises posteriores, auditorias trabalhistas e integrações com outros sistemas corporativos.

---

### Arquivos Utilizados

- `AFDT_ADS_2015-07-01_2015-10-31.txt` – Registros de ponto eletrônico no formato AFDT (Arquivo Fonte de Dados Tratados), conforme especificação da Portaria 1510/2009.
- `ACJEF_ADS_01072015_31102015.txt` – Arquivo de Controle de Jornada para Efeitos Fiscais (ACJEF), contendo a jornada efetiva dos colaboradores.
- `ADS.txt` – Arquivo mestre com dados cadastrais dos funcionários da empresa.

---

### Metodologia de Engenharia de Dados

O processo de estruturação dos dados iniciou-se com a segmentação dos arquivos por tipo de registro, conforme layout de posições fixas definido no anexo técnico da Portaria 1510/2009. Cada tipo de dado foi analisado, interpretado e modelado respeitando os princípios de normalização relacional e integridade referencial.

A modelagem resultou na criação de três tabelas principais, organizadas para garantir a consistência e a rastreabilidade dos dados. As estruturas foram otimizadas para permitir análise de jornadas, validação de marcações e integração com dados cadastrais.

---

### Estrutura das Tabelas Criadas

#### Tabela: `afdt_registros`

Armazena os registros brutos de marcação de ponto conforme extraído do arquivo AFDT.

```sql
CREATE TABLE afdt_registros (
    id SERIAL PRIMARY KEY,
    seq_registro CHAR(9),
    tipo_registro CHAR(1),
    data_marcacao DATE,
    hora_marcacao TIME,
    pis CHAR(11),
    num_rep CHAR(17),
    tipo_marcacao CHAR(1),
    sequencial_marcacao CHAR(2),
    tipo_informacao CHAR(1),
    motivo TEXT
);
```

---

#### Tabela: `acjef_jornada`

Contém o detalhamento da jornada efetiva por funcionário, com cálculo de horas extras, faltas e compensações.

```sql
CREATE TABLE acjef_jornada (
    id SERIAL PRIMARY KEY,
    seq_registro CHAR(9),
    tipo_registro CHAR(1),
    pis CHAR(11),
    data_jornada DATE,
    hora_entrada TIME,
    codigo_horario CHAR(4),
    horas_diurnas TIME,
    horas_noturnas TIME,
    horas_extras1 TIME,
    perc_adicional1 CHAR(4),
    modalidade_extra1 CHAR(1),
    horas_extras2 TIME,
    perc_adicional2 CHAR(4),
    modalidade_extra2 CHAR(1),
    horas_extras3 TIME,
    perc_adicional3 CHAR(4),
    modalidade_extra3 CHAR(1),
    horas_extras4 TIME,
    perc_adicional4 CHAR(4),
    modalidade_extra4 CHAR(1),
    horas_falta_atraso TIME,
    sinal_compensacao CHAR(1),
    saldo_compensacao CHAR(4)
);
```

---

#### Tabela: `ads_funcionarios`

Tabela com dados cadastrais dos funcionários, incluindo o PIS como identificador de chave.

```sql
CREATE TABLE ads_funcionarios (
    id SERIAL PRIMARY KEY,
    pis CHAR(11) UNIQUE NOT NULL,
    nome TEXT NOT NULL,
    codigo_departamento TEXT NOT NULL,
    data_admissao DATE NOT NULL
);
```


# Dicionário de Dados

## Tabela: `afdt_registros`
Registros brutos de marcação de ponto conforme extraído do arquivo AFDT.

| Campo                | Tipo     | Tamanho | Obrigatório | Descrição                                                           |
|----------------------|----------|---------|-------------|----------------------------------------------------------------------|
| id                   | SERIAL   | -       | Sim         | Identificador único da linha (chave primária)                        |
| seq_registro         | CHAR     | 9       | Sim         | Sequência do registro no arquivo                                     |
| tipo_registro        | CHAR     | 1       | Sim         | Tipo do registro (conforme layout da Portaria 1510)                  |
| data_marcacao        | DATE     | -       | Sim         | Data da marcação de ponto                                            |
| hora_marcacao        | TIME     | -       | Sim         | Hora da marcação de ponto                                            |
| pis                  | CHAR     | 11      | Sim         | Número do PIS do funcionário                                         |
| num_rep              | CHAR     | 17      | Não         | Número de série do Registrador Eletrônico de Ponto (REP)            |
| tipo_marcacao        | CHAR     | 1       | Não         | Indica o tipo da marcação (entrada, saída etc.)                      |
| sequencial_marcacao  | CHAR     | 2       | Não         | Número sequencial da marcação no dia                                 |
| tipo_informacao      | CHAR     | 1       | Não         | Tipo de informação complementar                                      |
| motivo               | TEXT     | -       | Não         | Observações ou motivo da marcação, se aplicável                      |

---

## Tabela: `acjef_jornada`
Detalhamento da jornada efetiva por funcionário.

| Campo                | Tipo     | Tamanho | Obrigatório | Descrição                                                            |
|----------------------|----------|---------|-------------|----------------------------------------------------------------------|
| id                   | SERIAL   | -       | Sim         | Identificador único da linha (chave primária)                        |
| seq_registro         | CHAR     | 9       | Sim         | Sequência do registro no arquivo                                     |
| tipo_registro        | CHAR     | 1       | Sim         | Tipo do registro                                                     |
| pis                  | CHAR     | 11      | Sim         | Número do PIS do funcionário                                         |
| data_jornada         | DATE     | -       | Sim         | Data da jornada de trabalho                                          |
| hora_entrada         | TIME     | -       | Não         | Hora de entrada registrada                                           |
| codigo_horario       | CHAR     | 4       | Não         | Código do horário de trabalho                                        |
| horas_diurnas        | TIME     | -       | Não         | Total de horas trabalhadas no período diurno                         |
| horas_noturnas       | TIME     | -       | Não         | Total de horas no período noturno                                    |
| horas_extras1        | TIME     | -       | Não         | Horas extras tipo 1                                                  |
| perc_adicional1      | CHAR     | 4       | Não         | Percentual de adicional das horas extras tipo 1                      |
| modalidade_extra1    | CHAR     | 1       | Não         | Modalidade das horas extras tipo 1                                   |
| horas_extras2        | TIME     | -       | Não         | Horas extras tipo 2                                                  |
| perc_adicional2      | CHAR     | 4       | Não         | Percentual de adicional das horas extras tipo 2                      |
| modalidade_extra2    | CHAR     | 1       | Não         | Modalidade das horas extras tipo 2                                   |
| horas_extras3        | TIME     | -       | Não         | Horas extras tipo 3                                                  |
| perc_adicional3      | CHAR     | 4       | Não         | Percentual de adicional das horas extras tipo 3                      |
| modalidade_extra3    | CHAR     | 1       | Não         | Modalidade das horas extras tipo 3                                   |
| horas_extras4        | TIME     | -       | Não         | Horas extras tipo 4                                                  |
| perc_adicional4      | CHAR     | 4       | Não         | Percentual de adicional das horas extras tipo 4                      |
| modalidade_extra4    | CHAR     | 1       | Não         | Modalidade das horas extras tipo 4                                   |
| horas_falta_atraso   | TIME     | -       | Não         | Horas faltantes ou atrasos no dia                                    |
| sinal_compensacao    | CHAR     | 1       | Não         | Indica sinal positivo ou negativo no saldo de compensação            |
| saldo_compensacao    | CHAR     | 4       | Não         | Saldo de horas a compensar ou compensadas                            |

---

## Tabela: `ads_funcionarios`
Dados cadastrais dos funcionários.

| Campo               | Tipo     | Tamanho | Obrigatório | Descrição                                      |
|---------------------|----------|---------|-------------|-------------------------------------------------|
| id                  | SERIAL   | -       | Sim         | Identificador único do funcionário              |
| pis                 | CHAR     | 11      | Sim         | Número do PIS (único por funcionário)           |
| nome                | TEXT     | -       | Sim         | Nome completo do funcionário                    |
| codigo_departamento | TEXT     | -       | Sim         | Código ou descrição do departamento             |
| data_admissao       | DATE     | -       | Sim         | Data de admissão na empresa                     |

