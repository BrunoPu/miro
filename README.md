import json
from pyspark.sql import SparkSession, DataFrame
from awsglue.context import GlueContext
from awsglue.dynamicframe import DynamicFrame
from awsglue.utils import getResolvedOptions
from awsglue.jobs import Job
import os
import sys

# Obtenha os parâmetros de entrada do job
args = getResolvedOptions(sys.argv, ['JOB_NAME'])

# Inicialize a SparkSession
spark = SparkSession.builder.getOrCreate()

# Inicialize o GlueContext
glueContext = GlueContext(spark)
spark = glueContext.spark_session

# Inicialize o job do Glue
job = Job(glueContext)
job.init(args['JOB_NAME'], args)

def str_to_list_of_dicts(data_str):
    """
    Converte uma string para um array de dicionários
    """
    data_str = data_str.replace("\'", "\"")  # Substitui apóstrofos por aspas duplas
    return json.loads(data_str)  # Converte a string JSON para uma lista de dicionários

def get_table_from_connector(db_host: str, db_name: str, arn_username: str, arn_pwd: str, table_name: str, db_port: str) -> DataFrame:
    """
    Conecta ao banco de dados SQL Server e retorna uma tabela como DataFrame do Spark
    """
    # Define os parâmetros JDBC para a conexão com o SQL Server
    jdbc_parameters = {
        "url": f"jdbc:sqlserver://{db_host}:{db_port};databaseName={db_name}",
        "driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
        "user": arn_username,
        "password": arn_pwd
    }

    # Conecta ao banco de dados e lê a tabela como um DataFrame do Spark
    df = spark.read.format("jdbc").options(**jdbc_parameters).option("dbtable", table_name).load()
    return df

def write_to_glue_catalog(df: DataFrame, database_name: str, table_name: str):
    """
    Escreve um DataFrame no Glue Data Catalog como um DynamicFrame
    """
    # Converte o DataFrame do Spark para um DynamicFrame do Glue
    dynamic_frame = DynamicFrame.fromDF(df, glueContext, "dynamic_frame")
    
    # Escreve o DynamicFrame no Glue Data Catalog
    glueContext.write_dynamic_frame.from_catalog(
        frame=dynamic_frame,
        database=database_name,
        table_name=table_name
    )

# Defina as variáveis necessárias para a conexão e obtenção da tabela
db_host = os.getenv("db_host")  # Substitua por seu valor real ou use variáveis de ambiente
db_name = os.getenv("db_name")  # Substitua por seu valor real ou use variáveis de ambiente
arn_username = os.getenv("arn_username")  # Substitua por seu valor real ou use variáveis de ambiente
arn_pwd = os.getenv("arn_pwd")  # Substitua por seu valor real ou use variáveis de ambiente
table_name = os.getenv("table_name")  # Substitua por seu valor real ou use variáveis de ambiente
db_port = os.getenv("db_port")  # Substitua por seu valor real ou use variáveis de ambiente

# Obtenha a tabela do banco de dados como um DataFrame do Spark
df = get_table_from_connector(db_host, db_name, arn_username, arn_pwd, table_name, db_port)

# Especifica o nome do database e da tabela no Glue Data Catalog
glue_database_name = "your_glue_database_name"  # Substitua pelo nome do database no Glue Data Catalog
glue_table_name = "your_glue_table_name"  # Substitua pelo nome da tabela no Glue Data Catalog

# Escreve o DataFrame no Glue Data Catalog
write_to_glue_catalog(df, glue_database_name, glue_table_name)

# Finaliza o job do Glue
job.commit()


--_---------------
CREATE VIEW NomeDaSuaView AS
SELECT
    Id,
    NomeCertificado,
    NumeroSerie,
    MultidominioSAN,
    Extensao,
    Ambiente,
    dtEmissao,
    dtVencimento,
    Status,
    CAInternaExterna,
    Balanceador,
    Sigla,
    Ltrim(CASE WHEN (SELECT DISTINCT identificado FROM TBCertificadosAnalistas (NOLOCK) A WHERE A.idCertificado = C.id) IS NULL THEN (AnalistaNome) ELSE (SELECT DISTINCT SUBSTRING(
        (SELECT TOP 1 AnalistaFuncional
        FROM TBCertificadosAnalistas (NOLOCK) P
        WHERE P.idCertificado = C2.idCertificado
        FOR XML PATH ('')), 1, 1000) AnalistaFuncional
        FROM TBCertificadosAnalistas (NOLOCK) C2
        WHERE C2.idCertificado = C.id) END) AS [AnalistaFuncional],
    Ltrim(CASE WHEN (SELECT DISTINCT identificado FROM TBCertificadosAnalistas (NOLOCK) A WHERE A.idCertificado = C.id) IS NULL THEN (AnalistaNome) ELSE (SELECT DISTINCT SUBSTRING(
        (SELECT TOP 1 AnalistaFuncional
        FROM TBCertificadosAnalistas (NOLOCK) P
        WHERE P.idCertificado = C2.idCertificado
        FOR XML PATH ('')), 1, 1000) Nomes
        FROM TBCertificadosAnalistas (NOLOCK) C2
        WHERE C2.idCertificado = C.id) END) AS [AnalistaNome],
    Ltrim((SELECT DISTINCT SUBSTRING(
        (SELECT TOP 1 Nomes
        FROM TBCertificadosSquad (NOLOCK) P
        WHERE P.idCertificado = C2.idCertificado
        FOR XML PATH ('')), 1, 1000) Nomes
        FROM TBCertificadosSquad (NOLOCK) C2
        WHERE C2.idCertificado = C.id)) AS Squad,
    Ltrim((SELECT DISTINCT SUBSTRING(
        (SELECT TOP 1 Nomes
        FROM TBCertificadosComunidade (NOLOCK) P
        WHERE P.idCertificado = C2.idCertificado
        FOR XML PATH ('')), 1, 1000) Nomes
        FROM TBCertificadosComunidade (NOLOCK) C2
        WHERE C2.idCertificado = C.id)) AS Comunidade,
    Aplicacao,
    (SELECT DISTINCT SUBSTRING(
        (SELECT TOP 1 P.DescricaoEmissor
        FROM TBCertificadosEmissor (NOLOCK) P
        WHERE P.idEmissor = C2.idEmissor
        FOR XML PATH ('')), 1, 1000) Nomes
        FROM TBCertificadosEmissor (NOLOCK) C2
        WHERE C2.idEmissor = C.idEmissor) AS Emissor,
    SG.name,
    C.SupportGroup_sys_id,
    Servidores
FROM
    TBCertificados C (NOLOCK)
    LEFT JOIN TBSupportGroup SG (NOLOCK) ON SG.SYS_id = C.SupportGroup_sys_id
ORDER BY
    dtVencimento;
