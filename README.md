from flask import Flask, request, jsonify
from lxml import etree

app = Flask(__name__)

# Rota para receber o objeto XML via POST
@app.route('/receive_xml', methods=['POST'])
def receive_xml():
    xml_data = request.data  # Recebe o objeto XML na solicitação POST

    # Parse do XML recebido
    root = etree.fromstring(xml_data)

    # Extrair os dados do XML
    object_type = root.find('.//object_type').text
    object_id = root.find('.//object_id').text
    attribute1 = root.find('.//attribute1').text
    attribute2 = root.find('.//attribute2').text

    # Montar a solicitação SOAP usando os dados extraídos
    soap_request = """
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:web="http://www.example.com/webservice">
        <soapenv:Header/>
        <soapenv:Body>
            <web:CreateObject>
                <web:ObjectType>{object_type}</web:ObjectType>
                <web:ObjectID>{object_id}</web:ObjectID>
                <web:Attributes>
                    <web:Attribute name="attribute1">{attr_value1}</web:Attribute>
                    <web:Attribute name="attribute2">{attr_value2}</web:Attribute>
                    <!-- Adicione mais atributos conforme necessário -->
                </web:Attributes>
            </web:CreateObject>
        </soapenv:Body>
    </soapenv:Envelope>
    """.format(
        object_type=object_type,
        object_id=object_id,
        attr_value1=attribute1,
        attr_value2=attribute2
        # Adicione mais atributos aqui, se necessário
    )

    return soap_request, 200, {'Content-Type': 'application/xml'}

if __name__ == '__main__':
    app.run(debug=True)
