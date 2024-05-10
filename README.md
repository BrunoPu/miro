@echo off
set origem1=\\caminho\para\origem\arquivo1.txt
set origem2=\\caminho\para\origem\arquivo2.txt
set destino=\\caminho\para\destino

echo Verificando se ambos os arquivos existem...
if exist "%origem1%" (
    if exist "%origem2%" (
        echo Movendo ambos os arquivos...
        move /Y "%origem1%" "%destino%"
        move /Y "%origem2%" "%destino%"
    ) else (
        echo O arquivo2.txt não foi encontrado na pasta de origem.
    )
) else (
    echo O arquivo1.txt não foi encontrado na pasta de origem.
)

echo Processo concluído.