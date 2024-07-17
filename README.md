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
    # Parâmetros JDBC para a conexão com o SQL Server
    jdbc_parameters = {
        "driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
        "connection_string": f"jdbc:sqlserver://{db_host}:{db_port};databaseName={db_name}",
        "auth": {
            "aut_type": "secrets_manager",
            "user": arn_username,
            "password": arn_pwd
        }
    }
    
    # Conecta ao banco de dados e lê a tabela como um DataFrame do Spark
    jdbc = spark.get_jdbc_connector(jdbc_parameters=jdbc_parameters)
    sql = f"SELECT * FROM {table_name}"
    df = jdbc.get_sql_to_df(sql=sql)
    return df

def write_to_glue_catalog(df: DataFrame, database_name: str, table_name: str):
    """
    Escreve um DataFrame no Glue Data Catalog como um DynamicFrame
    """
    # Converte o DataFrame do Spark para um DynamicFrame do Glue
    dynamic_frame = DynamicFrame.fromDF(df, glueContext, "dynamic_frame")
    
    # Log das colunas do DataFrame antes de escrever no Glue Data Catalog
    print("Colunas do DataFrame original:")
    df.printSchema()
    
    # Escreve o DynamicFrame no Glue Data Catalog
    glueContext.write_dynamic_frame.from_catalog(
        frame=dynamic_frame,
        database=database_name,
        table_name=table_name
    )
    
    # Ler o DynamicFrame de volta do Glue Data Catalog para verificação
    dynamic_frame_read = glueContext.create_dynamic_frame.from_catalog(
        database=database_name,
        table_name=table_name
    )
    
    # Converter DynamicFrame lido de volta para DataFrame
    df_read = dynamic_frame_read.toDF()
    
    # Log das colunas do DataFrame lido do Glue Data Catalog
    print("Colunas do DataFrame lido do Glue Data Catalog:")
    df_read.printSchema()

# Defina as variáveis necessárias para a conexão e obtenção da tabela
db_host = os.getenv("db_host")  # Substitua por seu valor real ou use variáveis de ambiente
db_name = os.getenv("db_name")  # Substitua por seu valor real ou use variáveis de ambiente
arn_username = os.getenv("arn_username")  # Substitua por seu valor real ou use variáveis de ambiente
arn_pwd = os.getenv("arn_pwd")  # Substitua por seu valor real ou use variáveis de ambiente
table_name = os.getenv("table_name")  # Substitua por seu valor real ou use variáveis de ambiente
db_port = os.getenv("db_port")  # Substitua por seu valor real ou use variáveis de ambiente

# Obtém a tabela do conector
df = get_table_from_connector(db_host, db_name, arn_username, arn_pwd, table_name, db_port)

# Escreve a tabela no Glue Data Catalog
write_to_glue_catalog(df, db_name, table_name)

# Finaliza o job
job.commit()
