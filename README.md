from suds.client import Client
from suds.transport.http import HttpTransport

# URL do serviço SOAP
url = 'http://www.example.com/soap?wsdl'

# Função para desativar a validação do certificado SSL
def disable_ssl_verification():
    class NoCertVerificationTransport(HttpTransport):
        def u2open(self, u2request):
            u2request.verify = False
            return HttpTransport.u2open(self, u2request)

    return NoCertVerificationTransport()

# Criar o cliente SOAP com a verificação de certificado desativada
client = Client(url, transport=disable_ssl_verification())

# Exemplo de chamada de método SOAP
def make_soap_request(param1, param2):
    try:
        # Chame o método SOAP desejado
        response = client.service.MethodName(param1, param2)

        # Processar a resposta
        # (substitua 'MethodName' pelo nome do método real que deseja chamar)

        # Exemplo: Imprimir a resposta
        print(response)

    except Exception as e:
        print(f"Ocorreu um erro durante a solicitação SOAP: {e}")

# Exemplo de uso
make_soap_request('valor_parametro1', 'valor_parametro2')
