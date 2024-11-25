Criar um projeto em C# para consumir uma API envolve configurar um projeto e fazer as chamadas HTTP para a API. Aqui está um guia passo a passo para criar um projeto C# que consuma uma API:

1. Configurar o projeto em C#
Certifique-se de que o .NET SDK está instalado:

Para verificar:

dotnet --version

Crie um novo projeto de console:

dotnet new console -n ConsumirAPI

Navegue para a pasta do projeto:

cd ConsumirAPI

Abra o projeto em seu editor ou IDE favorito (Visual Studio, VS Code, etc.).

2. Instalar o pacote para chamadas HTTP
O pacote System.Net.Http já está incluído na maioria dos projetos .NET. Porém, se quiser usar algo como RestSharp ou Newtonsoft.Json para manipulação de JSON, instale os pacotes:

Para instalar o RestSharp (opcional):

dotnet add package RestSharp

Para instalar o Newtonsoft.Json (opcional, para manipular JSON):

dotnet add package Newtonsoft.Json

Atualize os pacotes para garantir compatibilidade:

dotnet restore

3. Consumir a API no código
Aqui está um exemplo básico usando HttpClient e Newtonsoft.Json para consumir uma API e exibir os dados:

Editar o arquivo Program.cs:

using System;
using System.Net.Http;
using System.Threading.Tasks;
using Newtonsoft.Json;

namespace ConsumirAPI
{
    class Program
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine("Consumindo API...");

            // Configurar o cliente HTTP
            using (HttpClient client = new HttpClient())
            {
                try
                {
                    // URL da API (substitua pela URL real)
                    string apiUrl = "https://api.example.com/endpoint";

                    // Fazer a requisição GET
                    HttpResponseMessage response = await client.GetAsync(apiUrl);
                    response.EnsureSuccessStatusCode();

                    // Ler a resposta como string
                    string responseBody = await response.Content.ReadAsStringAsync();

                    // Converter o JSON para objeto (defina um modelo)
                    var dados = JsonConvert.DeserializeObject<dynamic>(responseBody);

                    // Exibir os dados no console
                    Console.WriteLine("Dados recebidos:");
                    Console.WriteLine(dados);
                }
                catch (HttpRequestException e)
                {
                    Console.WriteLine($"Erro ao consumir API: {e.Message}");
                }
            }
        }
    }
}

4. Criar um modelo para mapear os dados (opcional)
Se você sabe o formato dos dados da API, crie uma classe para mapear a resposta:

Adicionar um modelo para os dados:

public class DadosAPI
{
    public string Nome { get; set; }
    public int Idade { get; set; }
}

Atualize o código para desserializar para a classe DadosAPI:

var dados = JsonConvert.DeserializeObject<DadosAPI>(responseBody);
Console.WriteLine($"Nome: {dados.Nome}, Idade: {dados.Idade}");

5. Rodar o projeto
No terminal, execute:

dotnet run

Verifique se os dados da API aparecem no console.

6. Personalizar e expandir
Métodos POST, PUT ou DELETE: Use os métodos client.PostAsync, client.PutAsync ou client.DeleteAsync para outros tipos de requisições.
Headers de autenticação: Adicione headers no cliente:

client.DefaultRequestHeaders.Add("Authorization", "Bearer seu-token");
Reutilizar HttpClient: Crie uma instância compartilhada de HttpClient para evitar desperdício de recursos.

Com isso, você pode consumir qualquer API REST em C#! 🚀
