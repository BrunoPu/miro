using System;
using System.IO;
using System.Net;
using System.Text;

namespace ExemploSOAP
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                // URL do serviço SOAP
                string url = "http://exemplo.com/servico-soap";

                // Criação do corpo da mensagem SOAP
                string mensagemSOAP = @"
                    <Envelope xmlns=""http://schemas.xmlsoap.org/soap/envelope/"">
                        <Body>
                            <ExemploRequisicao xmlns=""http://exemplo.com/servico"">
                                <Parametro1>Valor1</Parametro1>
                                <Parametro2>Valor2</Parametro2>
                            </ExemploRequisicao>
                        </Body>
                    </Envelope>
                ";

                // Configuração da requisição SOAP
                HttpWebRequest request = (HttpWebRequest)WebRequest.Create(url);
                request.Method = "POST";
                request.ContentType = "text/xml; charset=utf-8";
                request.Headers.Add("SOAPAction", "http://exemplo.com/servico/ExemploRequisicao");

                // Escrevendo a mensagem SOAP no corpo da requisição
                using (Stream stream = request.GetRequestStream())
                {
                    byte[] data = Encoding.UTF8.GetBytes(mensagemSOAP);
                    stream.Write(data, 0, data.Length);
                }

                // Realizando a requisição SOAP e obtendo a resposta
                using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                {
                    if (response.StatusCode == HttpStatusCode.OK)
                    {
                        // Processar a resposta SOAP
                        using (Stream responseStream = response.GetResponseStream())
                        {
                            StreamReader reader = new StreamReader(responseStream, Encoding.UTF8);
                            string respostaSOAP = reader.ReadToEnd();
                            // Tratar a resposta para obter os dados desejados
                            // ...
                            Console.WriteLine("Resposta SOAP:");
                            Console.WriteLine(respostaSOAP);
                        }
                    }
                    else
                    {
                        Console.WriteLine("Erro na requisição SOAP. Código de status: " + response.StatusCode);
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Ocorreu um erro: " + ex.Message);
            }
        }
    }
}
