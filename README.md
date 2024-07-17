import json
import os
import sys
from awsglue.context import GlueContext
from awsglue.dynamicframe import DynamicFrame
from awsglue.utils import getResolvedOptions
from awsglue.jobs import Job
from itaudatautils.data_utils import DataUtils

# Obtenha os parâmetros de entrada do job
args = getResolvedOptions(sys.argv, ['JOB_NAME'])

# Inicialize o SparkSession usando DataUtils
spark = DataUtils.get_spark()

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

def get_table_from_connector(db_host: str, db_name: str, arn_username: str, arn_pwd: str, db_port: str) -> DataFrame:
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
    
    # SQL complexo com várias condições e junções
    sql = """
    SELECT a.*, b.column1, c.column2
    FROM some_table a
    INNER JOIN another_table b ON a.id = b.id
    LEFT JOIN yet_another_table c ON a.id = c.id
    WHERE a.status = 'active' AND b.date > '2023-01-01'
    """
    
    # Executa a consulta SQL e obtém o DataFrame
    df = jdbc.get_sql_to_df(sql=sql)
    return df

def write_to_glue_catalog(df: DataFrame, glue_database_name: str, glue_table_name: str):
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
        database=glue_database_name,  # Nome do banco de dados no Glue Data Catalog
        table_name=glue_table_name    # Nome da tabela no Glue Data Catalog
    )
    
    # Ler o DynamicFrame de volta do Glue Data Catalog para verificação
    dynamic_frame_read = glueContext.create_dynamic_frame.from_catalog(
        database=glue_database_name,  # Nome do banco de dados no Glue Data Catalog
        table_name=glue_table_name    # Nome da tabela no Glue Data Catalog
    )
    
    # Converter DynamicFrame lido de volta para DataFrame
    df_read = dynamic_frame_read.toDF()
    
    # Log das colunas do DataFrame lido do Glue Data Catalog
    print("Colunas do DataFrame lido do Glue Data Catalog:")
    df_read.printSchema()

# Defina as variáveis necessárias para a conexão e obtenção da tabela
db_host = os.getenv("db_host")  # Host do banco de dados SQL Server
db_name = os.getenv("db_name")  # Nome do banco de dados SQL Server
arn_username = os.getenv("arn_username")  # Nome de usuário para autenticação
arn_pwd = os.getenv("arn_pwd")  # Senha para autenticação
db_port = os.getenv("db_port")  # Porta do banco de dados SQL Server

# Defina as variáveis necessárias para a escrita no Glue Data Catalog
glue_database_name = os.getenv("glue_database_name")  # Nome do banco de dados no Glue Data Catalog
glue_table_name = os.getenv("glue_table_name")  # Nome da tabela no Glue Data Catalog

# Obtém a tabela do conector
df = get_table_from_connector(db_host, db_name, arn_username, arn_pwd, db_port)

# Escreve a tabela no Glue Data Catalog
write_to_glue_catalog(df, glue_database_name, glue_table_name)

# Finaliza o job
job.commit()
