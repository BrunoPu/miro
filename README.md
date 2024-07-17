from awsglue.context import GlueContext  # Importa o contexto do AWS Glue
from awsglue.dynamicframe import DynamicFrame  # Importa o DynamicFrame do AWS Glue
from pyspark.context import SparkContext  # Importa o contexto do Spark

# Inicializa o contexto do Spark
sc = SparkContext()

# Inicializa o contexto do AWS Glue
glueContext = GlueContext(sc)

# Configurações para conectar ao SQL Server via JDBC
sql_server_options = {
    "url": "jdbc:sqlserver://seu-servidor-sql.database.windows.net:1433;database=seu-banco-de-dados;",
    "dbtable": "nome_da_tabela_sql_server",  # Nome da tabela no SQL Server
    "user": "seu_usuario",  # Usuário para autenticação no SQL Server
    "password": "sua_senha",  # Senha para autenticação no SQL Server
    "customDriverClass": "com.microsoft.sqlserver.jdbc.SQLServerDriver"  # Driver JDBC para SQL Server
}

# Lê os dados do SQL Server usando as opções de conexão configuradas
dynamic_frame = glueContext.create_dynamic_frame.from_options(
    connection_type="sqlserver",  # Tipo de conexão para SQL Server
    connection_options=sql_server_options  # Opções de conexão para SQL Server
)

# Escreve os dados para o Glue Data Catalog
glueContext.write_dynamic_frame.from_catalog(
    frame=dynamic_frame,  # DataFrame dinâmico que contém os dados do SQL Server
    database="seu_database_glue",  # Nome do banco de dados no Glue Data Catalog
    table_name="sua_tabela_glue",  # Nome da tabela no Glue Data Catalog
    transformation_ctx="transformed_data"  # Contexto de transformação (opcional)
)
