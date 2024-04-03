import subprocess

# Configuração das credenciais
aws_access_key_id = 'SEU_ACCESS_KEY_ID'
aws_secret_access_key = 'SEU_SECRET_ACCESS_KEY'
aws_default_region = 'sua_regiao'

# Endpoint personalizado
endpoint_url = 'https://Backet.vpce-134133'

# Comando AWS CLI para listar objetos no bucket com endpoint personalizado
command = f"aws s3 ls s3://{bucket_name}/ --endpoint-url {endpoint_url} --region {aws_default_region} --access-key-id {aws_access_key_id} --secret-access-key {aws_secret_access_key}"

# Executar o comando e capturar a saída
output = subprocess.run(command, shell=True, capture_output=True, text=True)

# Imprimir a saída
print(output.stdout)
