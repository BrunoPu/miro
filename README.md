using System;
using System.IO;
using EWSsqlDataProvider;

namespace ExcelToDatabase
{
    public class YourWebService : WebService
    {
        [WebMethod]
        public void InsertExcelData(string excelFilePath)
        {
            // Abrindo o arquivo Excel
            using (StreamReader reader = new StreamReader(excelFilePath))
            {
                // Ignora a primeira linha do arquivo Excel que contém os nomes das colunas
                reader.ReadLine();

                // Iterando sobre as linhas restantes do arquivo Excel
                while (!reader.EndOfStream)
                {
                    // Lendo a próxima linha do arquivo Excel
                    string[] values = reader.ReadLine().Split(',');

                    // Criando um novo objeto EWSsqlDataProvider para cada linha
                    using (EWSsqlDataProvider sql = new EWSsqlDataProviderConfig(parametros.sql))
                    {
                        // Adicionando os parâmetros para inserção no banco de dados
                        sql.ADDParam("@nome", SQLDbType.Varchar, 300, parameter.input, values[0]); // Adiciona o valor da coluna "nome"
                        sql.ADDParam("@sobrenome", SQLDbType.Varchar, 300, parameter.input, values[1]); // Adiciona o valor da coluna "sobrenome"
                        sql.ADDParam("@idade", SQLDbType.Int, 0, parameter.input, int.Parse(values[2])); // Adiciona o valor da coluna "idade"
                        sql.ADDParam("@telefone", SQLDbType.Varchar, 20, parameter.input, values[3]); // Adiciona o valor da coluna "telefone"
                        
                        // Executando o comando SQL para inserir os dados no banco de dados
                        sql.RunSQL();
                    }
                }
            }
        }
    }
}
