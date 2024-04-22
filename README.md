#!/bin/bash

# Verifica se o processo está em execução
if ! lsof -i :5000 > /dev/null; then
    # Se não estiver em execução, inicie o processo
    nohup python3 sua_aplicacao.py &

    # Aguarde um momento para o processo iniciar
    sleep 5

    # Verifique novamente se o processo está em execução
    if ! lsof -i :5000 > /dev/null; then
        echo "O processo não foi iniciado corretamente."
        exit 1
    fi
fi