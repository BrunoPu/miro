using System;
using System.Data.SqlClient;
using System.Data;
using System.IO;
using System.Web.Services;

namespace ExcelToDatabase
{
    public class YourWebService : WebService
    {
        [WebMethod]
        public void InsertExcelData(string excelFilePath)
        {
            // String de conexão com o banco de dados
            string connectionString = "SuaConnectionString";

            // Abrindo a conexão com o banco de dados
            using (SqlConnection connection = new SqlConnection(connectionString))
            {
                connection.Open();

                // Abrindo o arquivo Excel
                using (FileStream stream = File.Open(excelFilePath, FileMode.Open, FileAccess.Read))
                {
                    using (StreamReader reader = new StreamReader(stream))
                    {
                        // Lendo as linhas do arquivo Excel
                        string line;
                        while ((line = reader.ReadLine()) != null)
                        {
                            // Dividindo a linha em valores separados por vírgula
                            string[] values = line.Split(',');

                            // Montando a query de inserção
                            string query = "INSERT INTO SuaTabela (Nome, Sobrenome, Idade) VALUES (@Nome, @Sobrenome, @Idade)";

                            // Criando o comando SQL com a query e a conexão
                            using (SqlCommand command = new SqlCommand(query, connection))
                            {
                                // Definindo os parâmetros do comando SQL com os valores da linha do Excel
                                command.Parameters.AddWithValue("@Nome", values[0]);
                                command.Parameters.AddWithValue("@Sobrenome", values[1]);
                                command.Parameters.AddWithValue("@Idade", values[2]);

                                // Executando o comando SQL para inserir os dados no banco de dados
                                command.ExecuteNonQuery();
                            }
                        }
                    }
                }
            }
        }
    }
}
