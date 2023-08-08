from flask import Flask, request, Response
import urllib.request
import ssl

app = Flask(__name__)

@app.route('/process_xml', methods=['POST'])
def process_xml():
    # Receber o XML da solicitação POST
    xml_data = request.data

    # Enviar um pedido SOAP usando o XML recebido
    url = 'URL_DO_WEBSERVICE'  # Substitua pelo URL real do serviço SOAP
    headers = {'Content-Type': 'text/xml'}  # Cabeçalhos da solicitação

    # Criar um contexto SSL sem verificação de certificado
    ssl_context = ssl.create_default_context()
    ssl_context.check_hostname = False
    ssl_context.verify_mode = ssl.CERT_NONE

    # Configurar um manipulador HTTPS com o contexto SSL personalizado
    https_handler = urllib.request.HTTPSHandler(context=ssl_context)

    # Criar um opener personalizado
    opener = urllib.request.build_opener(https_handler)

    # Instalar o opener personalizado
    urllib.request.install_opener(opener)

    # Criar uma solicitação usando urllib.request
    req = urllib.request.Request(url, data=xml_data, headers=headers, method='POST')

    # Fazer a solicitação e ler a resposta
    with urllib.request.urlopen(req) as response:
        soap_response = response.read()  # Ler a resposta da solicitação

    # Retornar a resposta SOAP como uma resposta HTTP do Flask
    return Response(response=soap_response, content_type='text/xml')

if __name__ == '__main__':
    app.run(debug=True)  # Iniciar o servidor Flask em modo de depuração
