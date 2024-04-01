import boto3

# Criação de uma sessão Boto3
session = boto3.Session()

# Obtém o ID da conta AWS
account_id = session.client('sts').get_caller_identity().get('Account')

# Agora você pode usar o ID da conta AWS
print("ID da conta AWS:", account_id)
