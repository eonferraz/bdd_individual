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
