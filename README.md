Se o script Python estiver na mesma pasta que o script Bash, você pode especificar o caminho relativo para o script Python. Aqui está o script Bash atualizado para usar o caminho relativo:

```bash
#!/bin/bash

# Verifica se o processo está em execução
if ! lsof -i :5000 > /dev/null; then
    # Se não estiver em execução, inicie o processo
    nohup python3 ./sua_aplicacao.py &

    # Aguarde um momento para o processo iniciar
    sleep 5

    # Verifique novamente se o processo está em execução
    if ! lsof -i :5000 > /dev/null; then
        echo "O processo não foi iniciado corretamente."
        exit 1
    fi
fi
```

Nesse caso, `./sua_aplicacao.py` indica que o script Python está localizado na mesma pasta que o script Bash. Certifique-se de ter permissão de execução para ambos os scripts e execute o script Bash conforme necessário.