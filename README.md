from datetime import datetime
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from pyspark.sql import SparkSession

# Função para executar uma consulta SQL no Spark e retornar um DynamicFrame
def sparkSqlQuery(glueContext, query, mapping, transformation_ctx):
    for alias, frame in mapping.items():
        frame.toDF().createOrReplaceTempView(alias)
    result = spark.sql(query)
    return DynamicFrame.fromDF(result, glueContext, transformation_ctx)

# Inicializa o SparkSession e o GlueContext
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session

# Inicializa o job Glue
args = getResolvedOptions(sys.argv, ["JOB_NAME"])
job = Job(glueContext)
job.init(args['JOB_NAME'], args)

# Consulta SQL para selecionar os dados
sql_query = """
SELECT active, name, parent, sys_id, parent FROM SYS_USER_GROUP_GROUP_SOT
"""

# Executa a consulta SQL e obtém o resultado
sql_result = sparkSqlQuery(glueContext, query=sql_query, mapping={"SYS_USER_GROUP_SOT": AWSGlueDataCatalog_node1710960629296}, transformation_ctx="SQLQuery_node1718528576676")

# Data atual
data_atual = datetime.now().strftime("%Y-%m-%d")

# Define o nome do arquivo com a data atual
nome_arquivo = "dados_grupo_sot_{}.csv".format(data_atual)

# Define o caminho completo para o arquivo no S3
output_path = "s3://itau-corp-sot-sa-east-1-597843998505/grupodesuporte-sot/" + nome_arquivo

# Escreve o resultado no S3 com o nome do arquivo contendo a data atual
glueContext.write_dynamic_frame.from_options(
    frame=sql_result.coalesce(1),
    connection_type="s3",
    format="csv",
    connection_options={
        "path": output_path,
        "partitionKeys": []
    },
    transformation_ctx="AmazonS3_node1711394024454"
)

# Consulta SQL para selecionar os dados
sql_query2 = """
SELECT * FROM hub_people_analytics_colaborador
WHERE data_referencia = (SELECT max(data_referencia) FROM hub_people_analytics_colaborador)
"""

# Executa a consulta SQL e obtém o resultado
sql_result2 = sparkSqlQuery(glueContext, query=sql_query2, mapping={"hub_people_analytics_colaborador": SelectFields_node1711475112834}, transformation_ctx="SQLQuery_node1711390319917")

# Define o nome do segundo arquivo com a data atual
nome_arquivo2 = "dados_colaborador_{}.csv".format(data_atual)

# Define o caminho completo para o segundo arquivo no S3
output_path2 = "s3://itau-corp-spec-sa-east-1-597843998505/pessoas-spec/" + nome_arquivo2

# Escreve o segundo resultado no S3 com o nome do arquivo contendo a data atual
glueContext.write_dynamic_frame.from_options(
    frame=sql_result2.coalesce(1),
    connection_type="s3",
    format="csv",
    connection_options={
        "path": output_path2,
        "partitionKeys": []
    },
    transformation_ctx="AmazonS3_node1711394247763"
)

# Finaliza o job Glue
job.commit()
