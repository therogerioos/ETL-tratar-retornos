# Projeto ETL - Rotinas Diárias
Este repositório hospeda três projetos de ETL, que utilizei na empresa de call center que trabalho.

### O motivo da criação deste projeto?
* Tinhamos um volume muito grande de dados sobre chamadas para tratar e gerar informações para enviar ao contratante diariamente.
* Precisavamos identificar o comportamento das chamadas para tomada de decisão, de qual perfil seria melhor para entrar em contato.
* Não tinhamos acesso direto ao banco de dados da aplicação, com isso, deveríamos resgatar o arquivo bruto, diretamente na aplicação.

### Qual a solução encontrada?
Diante dos obstaculos que encontravamos para gerar informações, e devido a morosidade para gerar reports, decidi desenvolver um ETL que pega o arquivo bruto extraído, realizasse a tratativa e disponibilizasse um arquivo tratado para enviar ao contratante ou importar no banco de dados da empresa.

### Projeto 1 - TratadorRotinasRetorno.ipynb
O projeto foi criado para rodar com Google Colab, pois, a capacidade de processamento das máquinas remotas da Google, agilizaria o processo de tratativa.
Utilizo neste tratador, a linguagem de programação Python e os frameworks Pandas (para manipulação das tabelas), OS (par exportação de arquivos) e DateTime (para gerenciamento de tempos e datas).

#### Etapas do processo

```python
nome_do_arquivo = 'ArquivoBruto_ControleControle_31052024'
```

Na pasta de arquivos brutos, eram hospedados arquivos diariamente, com isso, precisamos definir qual nome do arquivo seria tratado naquele momento em que o script fosse rodado.

```python
valorInicial = 6515592
```

Tinhamos uma regra de negócio no banco de dados para ter arquivos identificação única, poderia ser utilizado o autoincremento, porém, como correria o risco de arquivos incorretos, a inserção da chave primária do arquivo seria mais seguro para identificar futuras inconsistências, por isso, deveríamos incluir o valor inicial para o ID do arquivo a ser tratado.

```python
parametroData = nome_do_arquivo.split("_")[2]
operation = nome_do_arquivo.split("_")[1]
```

Utilizaremos no processo de ETL, a data e qual operação estamos tratando, com isso, separamos a data na variavel parametroData e o nome da operação em operation.

```python
caminho_do_arquivo = f'/content/drive/MyDrive/TIMrotinasPython/RetornoBruto/{nome_do_arquivo}.csv'
caminho_da_tabela_comparativa = '/content/drive/MyDrive/TIMPythonTabelasComparativa/tabelaComparativaStatus.csv'
caminho_da_tabela_ddd = '/content/drive/MyDrive/TIMPythonTabelasComparativa/tabelaDDD.csv'
caminho_da_tabela_MOP = '/content/drive/MyDrive/TIMPythonTabelasComparativa/tabelaMOPparaPython.csv'
```

Variaveis que contém os caminhos para todos os arquivos auxiliares de parametros que serão utilizados no projeto.

Os demais comandos inclusos no arquivo, disponível neste repositório, realizam junção de DDD com Telefone, adiciona qual estado pertence o DDD, atribui o status de cada substatus da chamada, realiza o looping pra atribuir o ID único, organiza as colunas dentro do Pandas, cria um arquivo tratado e exporta.

Tempo de execução sem o ETL: 1 a 2 horas (a depender do tamanho do arquivo)
Tempo de execução com o ETL: menos de 10 minutos.

O script pode ser adaptado para cada tipo de negócio, gerando uma eficiência no processo diário, e também pode ser introduzido, uma importação no banco de dados ou em qualquer datawarehouse.

### Projeto 2 - TratadorDeBaseMetaRecalculada.ipynb

Ele possui semelhança com projeto 1 no processo, de buscar arquivo bruto e geração de arquivo, a diferença é concernente a tratativa, que neste caso, uma filtragem de dois status, gerando um arquivo com analitico que será utilizado para atualização de um KPI gerencial.

Tempo de execução sem o ETL: 10 a 30 horas (a depender do tamanho do arquivo)
Tempo de execução com o ETL: menos de 5 minutos.

### Projeto 3 - TratadorDeURACheckout.ipynb

Neste projeto possui duas fontes de dados, que serão cruzadas, removendo as inconsistências, modelando a informação, com base na regra de negócio estabelecida, gerando um arquivo final, que será enviado ao contratante.

Tempo de execução sem o ETL: 2 a 3 horas (a depender do tamanho do arquivo)
Tempo de execução com o ETL: menos de 15 minutos.

Mais informações no nosso site: [therogerioos.com](https://therogerioos.com.br/)
