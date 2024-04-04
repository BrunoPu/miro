import boto3

# Configure as credenciais manualmente
aws_access_key_id = 'SEU_ACCESS_KEY_ID'
aws_secret_access_key = 'SEU_SECRET_ACCESS_KEY'
aws_default_region = 'sua_regiao'
aws_session_token = 'SEU_SESSION_TOKEN'  # Opcional, dependendo das suas credenciais

# Endpoint personalizado
endpoint_url = 'https://Backet.vpce-134133'

# Crie um cliente S3 com as credenciais configuradas manualmente e o endpoint personalizado
s3 = boto3.client('s3', 
                  aws_access_key_id=aws_access_key_id, 
                  aws_secret_access_key=aws_secret_access_key, 
                  region_name=aws_default_region,
                  aws_session_token=aws_session_token,
                  endpoint_url=endpoint_url)

# Nome do bucket
bucket_name = 'meubucket'

# Liste os objetos no bucket usando o endpoint personalizado
response = s3.list_objects(Bucket=bucket_name)

# Imprima os nomes dos objetos
for obj in response.get('Contents', []):
    print(obj['Key'])
