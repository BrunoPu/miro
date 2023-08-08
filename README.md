from flask import Flask, request

app = Flask(__name__)

@app.route('/receive_xml', methods=['POST'])
def receive_xml():
    xml_data = request.data  # Recebe o objeto XML na solicitação POST

    # Extrair os dados do XML recebido
    user_name = get_xml_value(xml_data, 'UserNane')
    password = get_xml_value(xml_data, 'Password')
    product_code = get_xml_value(xml_data, 'productcode')

    # Montar a solicitação SOAP usando os dados do XML recebido
    soap_request = f"""
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:web="http://system.globalsign/kb/ws/v1/">
        <soapenv:Header/>
        <soapenv:Body>
            <v1:PVOrder>
                <Request>
                    <OrderRequestHeader>
                        <AuthToken>
                            <UserNane>{user_name}</UserNane>
                            <Password>{password}</Password>
                        </AuthToken>
                        <OrderRequestHeader>
                            <productcode>{product_code}</productcode>
                        </OrderRequestHeader>
                    </OrderRequestHeader>
                </Request>
            </v1:PVOrder>
        </soapenv:Body>
    </soapenv:Envelope>
    """

    return soap_request, 200, {'Content-Type': 'application/xml'}

def get_xml_value(xml_data, tag_name):
    import re
    match = re.search(f'<{tag_name}>(.*?)<\/{tag_name}>', xml_data.decode("utf-8"))
    if match:
        return match.group(1)
    return ""

if __name__ == '__main__':
    app.run(debug=True)
