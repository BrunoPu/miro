using System;
using System.Data.SqlClient;
using OfficeOpenXml;
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

                // Abrindo o arquivo Excel usando o pacote EPPlus
                using (ExcelPackage package = new ExcelPackage(new System.IO.FileInfo(excelFilePath)))
                {
                    // Acessando a primeira planilha do arquivo Excel
                    ExcelWorksheet worksheet = package.Workbook.Worksheets[0];

                    // Obtendo o número de linhas e colunas na planilha
                    int rowCount = worksheet.Dimension.Rows;
                    int columnCount = worksheet.Dimension.Columns;

                    // Iterando sobre as linhas da planilha
                    for (int row = 2; row <= rowCount; row++) // Começa do 2 porque geralmente a primeira linha é o cabeçalho
                    {
                        // Montando a query de inserção
                        string query = "INSERT INTO SuaTabela (Nome, Sobrenome, Idade) VALUES (@Nome, @Sobrenome, @Idade)";

                        // Criando o comando SQL com a query e a conexão
                        using (SqlCommand command = new SqlCommand(query, connection))
                        {
                            // Definindo os parâmetros do comando SQL com os valores da planilha
                            command.Parameters.AddWithValue("@Nome", worksheet.Cells[row, 1].Value);
                            command.Parameters.AddWithValue("@Sobrenome", worksheet.Cells[row, 2].Value);
                            command.Parameters.AddWithValue("@Idade", worksheet.Cells[row, 3].Value);

                            // Executando o comando SQL para inserir os dados no banco de dados
                            command.ExecuteNonQuery();
                        }
                    }
                }
            }
        }
    }
}
