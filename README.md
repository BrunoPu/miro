using System;

class Program
{
    static void Main()
    {
        int year = 2024;
        int month = 5;
        int day = 2;

        // Formatar month e day com dois dígitos
        string formattedMonth = month.ToString("00");
        string formattedDay = day.ToString("00");

        // Concatenar as partes formatadas para formar a data no formato "YYYY-MM-DD"
        string dataString = $"{year}-{formattedMonth}-{formattedDay}";

        Console.WriteLine($"Data formatada: {dataString}");

        // Converter a string diretamente para um objeto DateTime
        DateTime data;
        if (DateTime.TryParseExact(dataString, "yyyy-MM-dd", System.Globalization.CultureInfo.InvariantCulture, System.Globalization.DateTimeStyles.None, out data))
        {
            Console.WriteLine($"Data convertida: {data}");
        }
        else
        {
            Console.WriteLine("Erro ao converter para data válida.");
        }
    }
}



SELECT 
    *
FROM 
    SuaTabela T
WHERE 
    DataAtualizacao = (
        SELECT 
            MAX(DataAtualizacao)
        FROM 
            SuaTabela
        WHERE 
            GrupoID = T.GrupoID
    );