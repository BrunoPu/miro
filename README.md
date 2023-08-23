import requests

url = "https://teste.login.com.br"
cert = ("/home/brupugl/cer/certificado.crt", "/home/brupugl/cer/chave.key")
cacert = "/home/brupugl/cer/CA-cert.pem"

response = requests.post(url, cert=cert, verify=cacert)

print(response.status_code)
print(response.text)
