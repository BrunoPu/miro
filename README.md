
from flask import Flask, request
import requests

app = Flask(__name__)

@app.route('/process_xml', methods=['POST'])
def process_xml():
    # Receber o XML da solicitação POST
    xml_data = request.data

    # Enviar um pedido SOAP usando o mesmo XML recebido
    response = requests.post('URL_DO_WEBSERVICE', data=xml_data, headers={'Content-Type': 'text/xml'})

    return response.content, response.status_code, response.headers.items()

if __name__ == '__main__':
    app.run(debug=True)
Neste exemplo, o código recebe o XML via solicitação POST e, em seguida, envia uma solicitação SOAP usando o mesmo XML recebido para uma URL fictícia (você deve substituir 'URL_DO_WEBSERVICE' pela URL real do serviço SOAP que você deseja acessar).

Certifique-se de adaptar este exemplo para a sua situação específica, incluindo a substituição da URL real e quaisquer outras considerações de segurança que você precise ter em mente ao lidar com dados de solicitação e envio de solicitações para um serviço SOAP.





