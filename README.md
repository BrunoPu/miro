#!/bin/bash

# Obtenha a data de ontem
yesterday=$(date -d "yesterday" "+%Y-%m-%d")

# Verifique se o processo está em execução na porta 5001
if lsof -i :5001 -P -sTCP:LISTEN | grep "(LISTEN)"; then
    echo "O processo na porta 5001 está em execução."
else
    # Se não estiver em execução, inicie o processo
    nohup python3 sua_aplicacao.py &
    sleep 5

    # Verifique novamente se o processo está em execução
    if lsof -i :5001 -P -sTCP:LISTEN | grep "(LISTEN)"; then
        echo "O processo foi iniciado com sucesso."
    else
        echo "Erro ao iniciar o processo."
        exit 1
    fi
fi