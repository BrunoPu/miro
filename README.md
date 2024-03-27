
from datetime import datetime

# Data atual
data_atual = datetime.now().strftime("%Y-%m-%d")

# Defina o nome do arquivo com a data atual
nome_arquivo = "nome_do_arquivo_{}.csv".format(data_atual)

# Defina o caminho completo para o arquivo no S3
output_path = "s3://iteu-corp-spec-sa-east-1-597043998505/pessoas-sp/" + nome_arquivo

# Escreva o DataFrame no S3 com o nome do arquivo contendo a data atual
glueContext.write_dynamic_frame.from_options(
    frame=AddCurrentTinestamp_nodes711542072377.coalesce(1),
    connection_type="s3",
    format="csv",
    connection_options={
        "path": output_path,
        "partitionKeys": []
    },
    transformation_ctx="AnazonS3 pode1711394247763"
)
