from flask import Flask, jsonify
import requests

app = Flask(__name__)

@app.route('/fazer_login', methods=['GET'])
def fazer_login():
    # URL do serviço de login (substitua pela URL correta)
    url = 'https://teste.login.com.br'
    
    # Caminhos para os certificados e chaves
    cert_path = '/home/brupugl/cer/certificado.crt'
    key_path = '/home/brupugl/cer/chave.key'
    cacert_path = '/home/brupugl/cer/CA-cert.pem'
    
    # Realiza a solicitação POST com os certificados e verifica o certificado do servidor
    response = requests.post(
        url,
        cert=(cert_path, key_path),
        verify=cacert_path
    )
    
    # Verifica se o login foi bem-sucedido (código de status 200)
    if response.status_code == 200:
        token = response.text  # Assume que o token é retornado no corpo da resposta
        return jsonify({'token': token})  # Retorna o token como JSON
    else:
        return jsonify({'error': 'Login failed'}), 401  # Retorna erro 401 se o login falhar

if __name__ == '__main__':
    app.run(debug=True)
