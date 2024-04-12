#!/bin/bash

# Verifica se o script python está rodando na porta 5000
if ! lsof -i :5000 > /dev/null 2>&1; then
    echo "Script request.py não está rodando. Iniciando..."
    nohup python3 request.py > /dev/null 2>&1 &
    echo "Script iniciado com sucesso."
else
    echo "Script request.py já está rodando. Não é necessário iniciar novamente."
fi
