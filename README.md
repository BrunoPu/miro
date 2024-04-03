import requests

# Função para chamar a API e obter o JSON
def chamar_api():
    url = "URL_DA_API_AQUI"
    response = requests.get(url)
    if response.status_code == 200:
        return response.json()
    else:
        print("Erro ao chamar a API:", response.status_code)
        return None

# Chama a função para obter o JSON
json_data = chamar_api()

# Verifica se o JSON foi obtido com sucesso
if json_data:
    # Obtém os valores de access_key e token
    access_key = json_data["data"]["access_key"]
    token = json_data["data"]["token"]

    # Você pode então usar as variáveis conforme necessário
    print("Access Key:", access_key)
    print("Token:", token)
