import boto3

# Configure as credenciais manualmente
aws_access_key_id = 'SEU_ACCESS_KEY_ID'
aws_secret_access_key = 'SEU_SECRET_ACCESS_KEY'
aws_default_region = 'sua_regiao'
aws_session_token = 'SEU_SESSION_TOKEN'  # Opcional, dependendo das suas credenciais

# Crie um cliente S3 com as credenciais configuradas manualmente
s3 = boto3.client('s3', 
                  aws_access_key_id=aws_access_key_id, 
                  aws_secret_access_key=aws_secret_access_key, 
                  region_name=aws_default_region,
                  aws_session_token=aws_session_token)

# Agora você pode usar o cliente S3 normalmente
buckets = s3.list_buckets()

for bucket in buckets['Buckets']:
    print(bucket['Name'])
