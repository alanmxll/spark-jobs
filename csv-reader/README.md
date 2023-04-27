# Script Spark para ler vários arquivos CSV do DBFS

Este script Python lê todos os arquivos CSV de um diretório no DBFS usando o Apache Spark, processa os dados de acordo com uma regra de negócios e grava o resultado em uma nova localização. O script assume que os dados estão armazenados na AWS S3.

## Executando o Script

Para executar o script, use o `spark-submit` e passe os seguintes parâmetros:

```
spark-submit \
    --name <nome-do-job> \
    <caminho-para-o-script>
```

Ex:

```
spark-submit \
    --name csv-reader-job> \
    csv-reader-job.py
```

Certifique-se de substituir `<nome-do-job>` por um nome para o job e `<caminho-para-o-script>` pelo caminho para o arquivo de script.

## Configuração

O script requer que as seguintes variáveis de ambiente sejam definidas:

- `S3A_ENDPOINT`: O endpoint para seu armazenamento S3
- `S3A_ACCESS_KEY`: Sua chave de acesso S3
- `S3A_SECRET_KEY`: Sua chave secreta S3

Estes podem ser definidos em um arquivo `.env` no mesmo diretório do script.

## Configuração do Spark

O script define as seguintes opções de configuração do Spark:

- `spark.sql.warehouse.dir`: O local para armazenar metadados do Spark
- `fs.s3a.endpoint`: O endpoint para seu armazenamento S3
- `fs.s3a.access.key`: Sua chave de acesso S3
- `fs.s3a.secret.key`: Sua chave secreta S3
- `fs.s3a.impl`: A classe de implementação para S3A
- `fs.s3a.path.style.access`: Se deve usar acesso estilo caminho para S3A

## Processamento de Dados

O script lê os arquivos CSV de `s3a://landing/*.csv` e os carrega em um DataFrame. Em seguida, converte o DataFrame para o formato Parquet e grava em `s3a://processing/df-parquet-file.parquet`. O script lê o arquivo Parquet e cria uma visualização temporária, que é usada para processar os dados de acordo com a regra de negócios. O resultado é então gravado em `s3a://curated/df-result-file.parquet`.

## Registro

O script define o nível de registro para `ERROR` no contexto do Spark. Se você precisar ver um registro mais detalhado durante o desenvolvimento, pode alterar isso para `INFO`.

## Dependências

O script usa as seguintes dependências:

- `dotenv`: para carregar variáveis de ambiente de um arquivo `.env`
- `pyspark`: a API Python para o Apache Spark
