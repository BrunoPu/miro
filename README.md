using System;
using System.Configuration; // Para acessar o arquivo de configuração web.conf
using System.Data;
using System.Data.SqlClient; // Para trabalhar com SQL Server
using System.IO;

public class JsonToDatabase
{
    public void InserirDadosDoJson()
    {
        string caminhoArquivoJson = "caminho/do/arquivo.json"; // Substitua pelo caminho real do seu arquivo JSON

        // Ler o arquivo JSON
        string[] linhas = File.ReadAllLines(caminhoArquivoJson);

        // Conexão com o banco de dados
        string connectionString = ConfigurationManager.ConnectionStrings["NomeDaSuaConexao"].ConnectionString;
        
        using (SqlConnection conexao = new SqlConnection(connectionString))
        {
            // Abre a conexão
            conexao.Open();

            foreach (string linha in linhas)
            {
                // Convertendo a linha JSON para um objeto C#
                var objetoJson = Newtonsoft.Json.JsonConvert.DeserializeObject<dynamic>(linha);

                // Chamando a procedure para inserir os dados
                using (SqlCommand comando = new SqlCommand("NomeDaSuaProcedure", conexao))
                {
                    comando.CommandType = CommandType.StoredProcedure;
                    comando.Parameters.Add("@Nome", SqlDbType.NVarChar).Value = objetoJson.Nome;
                    comando.Parameters.Add("@Funcional", SqlDbType.NVarChar).Value = objetoJson.Funcional;

                    comando.ExecuteNonQuery();
                }
            }
        }
    }
}
