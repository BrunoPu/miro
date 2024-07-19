import boto3
import logging
from awsglue.context import GlueContext
from pyspark.sql import DataFrame
from awsglue.dynamicframe import DynamicFrame

def persist_dataframes(glue_context: GlueContext, persist_df: DataFrame, s3_path: str, database: str, table: str) -> None:
    """
    Persiste DataFrame no S3 em formato Parquet e atualiza o catálogo do Glue.
    
    Args:
    glue_context (GlueContext): O contexto do Glue.
    persist_df (DataFrame): O DataFrame a ser persistido.
    s3_path (str): O caminho do S3 onde os dados serão armazenados.
    database (str): O nome do banco de dados do Glue.
    table (str): O nome da tabela do Glue.
    """
    
    try:
        table_glue = glue_context.getSink(
            connection_type="s3",
            path=s3_path,
            enableUpdateCatalog=True,
            updateBehavior="LOG",
            format="glueparquet",
            transformation_ctx="datasink_tb"
        )
        # Configura o sink do Glue com o tipo de conexão, caminho do S3, atualização do catálogo, comportamento de atualização, formato e contexto de transformação

        table_glue.setFormat("glueparquet")
        # Define o formato do sink do Glue para 'glueparquet'

        table_glue.setCatalogInfo(
            catalogDatabase=database,
            catalogTableName=table
        )
        # Define as informações do catálogo do sink do Glue com o banco de dados e o nome da tabela

        table_glue.writeFrame(DynamicFrame.fromDF(persist_df, glue_context, "persist"))
        # Escreve o DataFrame fornecido para o sink do Glue

    except Exception as e:
        logging.error("Erro ao escrever DataFrame em um arquivo Parquet: %s", e)
        # Registra um erro caso ocorra uma exceção durante a escrita do DataFrame

        raise e
        # Relança a exceção para tratamento posterior

# Exemplo de uso
# glue_context = GlueContext(sc)
# persist_df = ...
# persist_dataframes(glue_context, persist_df, "s3://meu-bucket/minha-tabela/", "meu_database", "minha_tabela")
