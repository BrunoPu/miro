using System;
using System.Data.SqlClient;
using OfficeOpenXml;

namespace ExcelToDatabase
{
    public class ExcelToDatabaseHandler
    {
        private readonly SqlConnection _connection;

        public ExcelToDatabaseHandler(SqlConnection connection)
        {
            _connection = connection ?? throw new ArgumentNullException(nameof(connection));
        }

        public void InsertDataFromExcel(string excelFilePath)
        {
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
                    using (SqlCommand command = new SqlCommand(query, _connection))
                    {
                        // Definindo os parâmetros do comando SQL com os valores da planilha
                        command.Parameters.AddWithValue("@Nome", worksheet.Cells[row, 1].Value);
                        command.Parameters.AddWithValue("@Sobrenome", worksheet.Cells[row, 2].Value);
                        command.Parameters.AddWithValue("@Idade", worksheet.Cells[row, 3].Value);

                        // Executando o comando SQL
                        command.ExecuteNonQuery();
                    }
                }
            }
        }
    }
}
