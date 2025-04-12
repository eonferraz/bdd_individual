
## Exercício 2 – Ambiente Hadoop

### Enunciado
Este projeto teve como objetivo demonstrar o funcionamento de um ambiente Hadoop, com ênfase na utilização do HDFS (Hadoop Distributed File System) para o armazenamento e processamento distribuído de arquivos de texto. Como exemplo prático, foi utilizado um trecho da obra *“A Guerra dos Mundos”*, de H. G. Wells, permitindo validar o fluxo de leitura, distribuição e manipulação de dados no ecossistema Hadoop.

---

### Desenvolvimento

#### 1. Preparação do Arquivo
Foi criado um arquivo `.txt` contendo alguns parágrafos selecionados do livro *A Guerra dos Mundos*, que foi salvo no ambiente local com o nome `guerra.txt`. Esse conteúdo textual foi utilizado como base de simulação para tarefas de contagem de palavras, uma prática comum em testes de processamento distribuído utilizando o modelo MapReduce no ecossistema Hadoop.

#### 2. Upload para o HDFS
O arquivo foi enviado ao Hadoop HDFS com o seguinte comando:

```bash
hdfs dfs -put guerra.txt /entrada
```

Esse comando transfere o arquivo do sistema local para o diretório `/entrada` no HDFS.

---

#### 3. Execução do WordCount
Utilizamos o exemplo clássico `wordcount`, incluído na distribuição padrão do Hadoop, para processar o arquivo:

```bash
hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar wordcount /entrada /saida
```

Esse job realiza a leitura do arquivo `guerra.txt` e aplica o modelo MapReduce para contar a ocorrência de cada palavra no texto.

---

#### 4. Resultado
Após a execução do job, o Hadoop criou arquivos de saída na pasta `/saida`, contendo as palavras do texto e o número de ocorrências de cada uma delas. A leitura dos resultados foi feita com o comando:

```bash
hdfs dfs -cat /saida/part-r-00000
```

Esse comando exibe os pares `<palavra> <frequência>` diretamente no terminal.

---

#### 5. Exemplo de Resultado

| Palavra | Frequência |
|---------|------------|
| que     | 7          |
| com     | 6          |
| de      | 5          |
| em      | 4          |
| homem   | 4          |
| os      | 4          |
| ou      | 4          |
| se      | 4          |
| seus    | 4          |
| um      | 4          |
