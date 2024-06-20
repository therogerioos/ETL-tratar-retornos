# Projeto ETL - Rotinas Diárias
Este repositório hospeda três projetos de ETL, que utilizei na empresa de call center que trabalho.

### O motivo da criação deste projeto?
* Tínhamos um volume muito grande de dados sobre chamadas para tratar e gerar informações para enviar ao contratante diariamente.
* Precisávamos identificar o comportamento das chamadas para a tomada de decisão sobre qual perfil seria melhor para entrar em contato.
* Não tínhamos acesso direto ao banco de dados da aplicação, então, precisávamos resgatar o arquivo bruto diretamente na aplicação.

### Qual a solução encontrada?
Diante dos obstáculos para gerar informações e devido à morosidade na geração de relatórios, decidi desenvolver um ETL que pegasse o arquivo bruto extraído, realizasse a tratativa e disponibilizasse um arquivo tratado para enviar ao contratante ou importar no banco de dados da empresa.

### Projeto 1 - TratadorRotinasRetorno.ipynb
O projeto foi criado para rodar no Google Colab, pois a capacidade de processamento das máquinas remotas da Google agilizaria o processo de tratativa. Utilizo neste tratador a linguagem de programação Python e os frameworks Pandas (para manipulação das tabelas), OS (para exportação de arquivos) e DateTime (para gerenciamento de tempos e datas).

#### Etapas do processo

```python
nome_do_arquivo = 'ArquivoBruto_ControleControle_31052024'
```

Na pasta de arquivos brutos, eram hospedados arquivos diariamente, então, precisávamos definir qual nome do arquivo seria tratado no momento em que o script fosse rodado.

```python
valorInicial = 6515592
```

Tínhamos uma regra de negócio no banco de dados para ter arquivos com identificação única. Poderia ser utilizado o autoincremento, mas, como havia o risco de arquivos incorretos, a inserção da chave primária do arquivo seria mais segura para identificar futuras inconsistências. Por isso, incluímos o valor inicial para o ID do arquivo a ser tratado.

```python
parametroData = nome_do_arquivo.split("_")[2]
operation = nome_do_arquivo.split("_")[1]
```

Utilizamos no processo de ETL a data e a operação que estamos tratando, separando a data na variável `parametroData` e o nome da operação em `operation`.

```python
caminho_do_arquivo = f'/content/drive/MyDrive/TIMrotinasPython/RetornoBruto/{nome_do_arquivo}.csv'
caminho_da_tabela_comparativa = '/content/drive/MyDrive/TIMPythonTabelasComparativa/tabelaComparativaStatus.csv'
caminho_da_tabela_ddd = '/content/drive/MyDrive/TIMPythonTabelasComparativa/tabelaDDD.csv'
caminho_da_tabela_MOP = '/content/drive/MyDrive/TIMPythonTabelasComparativa/tabelaMOPparaPython.csv'
```

Variáveis que contêm os caminhos para todos os arquivos auxiliares de parâmetros que serão utilizados no projeto.

Os demais comandos inclusos no arquivo disponível neste repositório realizam a junção de DDD com Telefone, adicionam o estado a que pertence o DDD, atribuem o status de cada substatus da chamada, realizam o looping para atribuir o ID único, organizam as colunas dentro do Pandas, criam um arquivo tratado e exportam.

Tempo de execução sem o ETL: 1 a 2 horas (a depender do tamanho do arquivo)
Tempo de execução com o ETL: menos de 10 minutos.

O script pode ser adaptado para cada tipo de negócio, gerando eficiência no processo diário, e também pode ser introduzido para importação no banco de dados ou em qualquer data warehouse.

### Projeto 2 - TratadorDeBaseMetaRecalculada.ipynb

Este projeto possui semelhança com o Projeto 1 no processo de buscar o arquivo bruto e gerar o arquivo. A diferença é na tratativa, que neste caso envolve uma filtragem de dois status, gerando um arquivo analítico que será utilizado para a atualização de um KPI gerencial.

Tempo de execução sem o ETL: 10 a 30 horas (a depender do tamanho do arquivo)
Tempo de execução com o ETL: menos de 5 minutos.

### Projeto 3 - TratadorDeURACheckout.ipynb

Neste projeto, há duas fontes de dados que serão cruzadas, removendo as inconsistências e modelando a informação com base na regra de negócio estabelecida, gerando um arquivo final que será enviado ao contratante.

Tempo de execução sem o ETL: 2 a 3 horas (a depender do tamanho do arquivo)
Tempo de execução com o ETL: menos de 15 minutos.

Mais informações no nosso site: [therogerioos.com](https://therogerioos.com.br/)
