int year = 2024;
int month = 5;
int day = 2;

// Criar uma string no formato "YYYY-MM-DD"
string dataString = $"{year}-{month:00}-{day:00}";

// Converter a string para um objeto DateTime
DateTime data;
if (DateTime.TryParse(dataString, out data))
{
    Console.WriteLine($"Data formatada: {data.ToString("yyyy-MM-dd")}");
}
else
{
    Console.WriteLine("Não foi possível converter para data válida.");
}