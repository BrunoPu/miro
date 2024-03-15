import unittest

# Função que queremos testar
def somar(a, b):
    return a + b

# Classe de teste
class TestSomar(unittest.TestCase):

    # Teste para verificar se a função retorna o resultado esperado
    def test_soma(self):
        resultado = somar(3, 5)
        self.assertEqual(resultado, 8, "A soma está incorreta")

# Executar os testes
if __name__ == '__main__':
    unittest.main()
