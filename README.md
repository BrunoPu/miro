import subprocess  # Importa o módulo subprocess para executar comandos do sistema

def run_aws_command(command):
    # Executa o comando AWS CLI como um subprocesso
    process = subprocess.Popen(command, stdout=subprocess.PIPE, stderr=subprocess.PIPE, shell=True)
    # Captura a saída e os erros do subprocesso
    output, error = process.communicate()
    # Retorna a saída e os erros
    return output, error

# Comando AWS CLI desejado
command = "aws s3 ls s3://meus3 --endpoint-url HTTPS://meuendpint"
# Executa o comando AWS usando a função definida anteriormente
output, error = run_aws_command(command)

if error:
    # Se houver erro, imprime o erro decodificado
    print("Ocorreu um erro:", error.decode("utf-8"))
else:
    # Se não houver erro, imprime a saída decodificada do comando AWS
    print("Saída do comando AWS CLI:", output.decode("utf-8"))
