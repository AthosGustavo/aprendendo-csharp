# Estudo sobre C#
<details>
  <summary>Arquitetura</summary>
  <details>
    
  <summary>namespace,using e IDisposable</summary>

   ## namespace
 - namespace é uma propriedade usada como forma de agrupar e organizar código.Ela literalmente dá um nome para o espaço onde foi declarado tal código.
 - O namespace tem como objetivo melhorar a organização e permitir a declaração de variáveis com nome repetido, uma vez que para acessar essa variável é necessário informar o seu namespace. EXEMPLO
    ```C#
    nomeEspaço.nomeVariavel
    ```
 - Não é permitido declarar variáveis com mesmo nome dentro de um namespace

   ## using

   #### Importando namespace com using
 - Nesse contexto a propriedade using permite importar namespaces para utilizar no código.Sendo assim, using é semelhante ao **import** utilizada no java.
  ```C#
  using System;
  using System.Collections.Generic;

    class Program
    {
      static void Main()
      {
        List<string> myList = new List<string>(); // Não é necessário usar System.Collections.Generic
        Console.WriteLine("Hello, World!"); // Não é necessário usar System
      }
    }

  ```

  #### Declaração using para recursos descartáveis(IDisposable)
   - Nesse contexto, using é usado para lidar com classes que implementam interface **IDisposable** que são objetos que necessitam ter recursos liberados após o seu uso.

  ##### Sintaxe
  ```C#
  using(instancia do objeto){utilização do objeto e os seus métodos}
  ```
  ```C#
  using System;
  using System.Net.Http;
  using System.Threading.Tasks;

  class Program
  {
      static async Task Main()
      {
        using (HttpClient httpClient = new HttpClient())
        {
          string apiUrl = "https://jsonplaceholder.typicode.com/posts/1";
          HttpResponseMessage response = await httpClient.GetAsync(apiUrl);

          if (response.IsSuccessStatusCode)
          {
            string jsonResponse = await response.Content.ReadAsStringAsync();
            Console.WriteLine(jsonResponse);
          }
        } // O "using" garante que o HttpClient seja fechado e os recursos liberados aqui.
    }
}

  ```

   - Ao declarar dessa forma,aṕos a passagem do compilador pelo escopo do using, os recursos serão automaticamente liberados sem ser necessário a declaração explícita do fechamento.
  
  ## Objetos IDisposable
   - Objetos IDisposable são classes que implementam a interface IDisposable e necessitam ter os seus recursos fechados após o uso.
   - O não fechamento desses recursos pode ocasionar vazamento de memória e perca de performance

  #### Como identificar uma classe que implementa a interface IDisposable ?
   - Se você estiver trabalhando com uma classe que interaja com recursos externos (por exemplo, abrindo arquivos ou estabelecendo conexões de rede), é importante considerar a liberação adequada desses recursos quando a classe não for mais necessária.

  #### Fechamento explícito de um objeto IDisposable
  ```C#
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        HttpClient httpClient = new HttpClient();
        string apiUrl = "https://jsonplaceholder.typicode.com/posts/1";
        HttpResponseMessage response = await httpClient.GetAsync(apiUrl);

        if (response.IsSuccessStatusCode)
        {
            string jsonResponse = await response.Content.ReadAsStringAsync();
            Console.WriteLine(jsonResponse);
        }

        // Não estamos usando "using", então precisamos explicitamente chamar Dispose() para liberar os recursos.
        httpClient.Dispose();
    }
}

  ```


  </details>
</details>
<details>
  <summary>Lógica de programação</summary>

  ## Imprimindo e capturando valores no console
  
  #### Console.WriteLine()
   - printa e quebra linha
  
  #### Console.Write();
   - apenas printa

  #### Console.ReadLine();
   - Esse método e suas variáveis abre um input para o usuário.

  #### Console.Clear();
   - limpa o console
  
  <details>
    <summary>Sintaxes</summary>

  #### foreach
```C#
foreach(tipo variavel in nomeArray){}
```

  #### Dicionario
   - No C# um dicionário é semelhante a um objeto em javascript.

  **Sintaxe de declaração**
  ```C#
  Dictionary<TipoChave(TKey), TipoValor(TValue)> nomeVariavel = new Dictionary<TipoChave(TKey), TipoValor(TValue)>();

  ```

  **Adição de elementos**
  ```C#
  nomeVariavel.Add("nomeCarro","onix");
  nomeVariavel.Add("nomeMarca","chevrolet");
  nomeVariavel.Add("cor", "branco");
  ```

  **Remoção de elementos**
  ```C#
  meuDicionario.Remove("um");

  ```
  
  **Recuperando valores**
   - Para ter acesso a algum valor, basta usar a chave.
  ```C#
  String nomeCarro = meuDicionario["nomeCarro"];  //onix
  ```

  **TryGetValue**
   - Pega o valor de uma chave
  ```
  if(nomeArray.TryGetValue("chave", out tipoVarial nomeVarial)){
  }
  ```
  *Caso a chave exista no dicionário nomeArray, o valor da chave será guardado em nomeVarial*

  **Exemplo**
  ```C#
  Dictionary<string, string> dicionario = new Dictionary<string, string>();

        // Adicione algumas palavras ao dicionário
        dicionario["apple"] = "maçã";
        dicionario["banana"] = "banana";
        dicionario["car"] = "carro";

        // Tente obter a tradução de uma palavra
        string palavra = "apple";
        string traducao;

        if (dicionario.TryGetValue(palavra, out traducao))
        {
            Console.WriteLine($"A tradução de '{palavra}' é '{traducao}'.");
        }
  ```

  **ContainsKey**
   - Verifica se uma chave está presente em um dicionário
   - Retorna true/false

  ```C#
  nomeArray.ContainsKey(nomeChave)
  ```

  **Iteração sobre elementos**
  ```C#
  foreach (var x in meuDicionario){
    string chave = x.Key;
    int valor = x.Value;
    Console.WriteLine($"Chave: {chave}, Valor: {valor}");
}

  ```
  </details>
</details>
<details>
  <summary>Orientação a objetos</summary>
  <details>
    <summary>Visibilidade, atributos e encapsulamento</summary>
    
  ## Visibilidade
    
  - **public ->** atributos e metodos visiveis em qualquer classe
  - **private ->** atributos e metodos visiveis apenas na classe onde sao criadas
  - **protected ->** atributos e metodos visiveis em classes onde sao criados ou herdados

  - Geralmente um atributo privado e usado para esconder a lógica ou a regra de negocio de algo e
  o seu valor e obtido atraves de um método publico

  Exemplo
  ```C#
  public class CalculaNota{
    public double nota1;
    public double nota2;

    private double media(){
      return (nota1 + nota2)/2
    }

    public void resultado(){
      Console.Write($"O resultado é {media()}")
    }
  }
  ```
  ```C#
  using System;

  class Application{
    static void Main(string[] args){
      CalculaNota calculaNota = new CalculaNota();
      calculaNota.nota1 = 5;
      calculaNota.nota2 = 7;
      calculaNota.resultado();
    }

  }
```
 - Nesse caso, os atributos e o método resultado são de visibilidade publica e apenas o metodo media possui visibilidade privada, pois o objetivo é esconder a sua logica e apenas devolver o cálculo através do método resultado.

## Encapsulamento

 - Para atribuir o encapsulamento ao atributo privado, é necessário criar um atributo publico com o mesmo nome do atributo privado, mas a inicial deve ser em maiúsculo e no escopo desse novo atributo, basta declarar o get e set.

**Exemplo**
```C#
private string nome;
public string Nome{
  get{return nome;}
  set{nome = value;}
}
```
*Atribuindo valor (SET)*
```C#
Pessoa pessoa1 = new Pessoa();
pessoa1.Nome = "Athos";
```
*Acessando valor (GET)*
```C#
Console.Write(pessoa1.Nome);
```
  </details>
  <details>
    <summary>Herança e polimorfismo</summary>
     - Sintaxe de herança
      - Classe filhos : Classe Herdada

  - Sintaxe de polimorfismo
    - O metodo original recebe: visibilidade virtual tipo nome(){}
    - O metodo sobrescrito recebe: visibilidade override tipo nome(){}

  ## namespace
  
  ## Internal class

  
  </details>
</details>

<details>
  <summary>HTTPClient</summary>

  ## HttpClient
- Biblioteca usada para fazer solicitações HTTP a serviços web, como consumir APIs.

  <details>
      <summary>Requisições</summary>
    <details>
      <summary>Classe HttpResponseMessage</summary>  
        
    ## Classe HttpResponseMessage

    - Classe utilizada para manipular e tratar respostas de requisições
    - Essa classe terá o primeiro contato com a resposta da requisição ainda no formato JSON e para ler a resposta é necessário primeiro converter a response para uma string e após isso transformar para um array ou objeto através do pacote Newtonsoft.

    ```C#
    HttpResponseMessage response = await httpClient.GetAsync(apiUrl);
    ```

    ### Métodos importantes da classe

    **EnsureSuccessStatusCode()**

    - Usado para verificar se uma resposta HTTP foi bem sucedida
    - Caso a requisicao não for bem sucedida, uma execeção sem tratamento será lançada, caso o erro não ocorra, a execução continua e o método não retorna nada.
    

  ```C#
  using System;
  using System.Net.Http;
  using System.Threading.Tasks;

  class Program
  {
      static async Task Main()
      {
          // Crie uma instância de HttpClient.
          using (HttpClient httpClient = new HttpClient())
          {
              string apiUrl = "https://jsonplaceholder.typicode.com/posts/101"; // URL inválida

              try
              {
                  // Faça uma solicitação GET.
                  HttpResponseMessage response = await httpClient.GetAsync(apiUrl);

                  // Verifique se a solicitação foi bem-sucedida.
                  response.EnsureSuccessStatusCode();

              }
              catch (HttpRequestException ex)
              {
                  // Trate a exceção se a solicitação não foi bem-sucedida.
                  Console.WriteLine("Erro na solicitação: " + ex.Message);
              }
          }
      }
    }

  ```
  ### Propriedades mais importantes da classe

  <details>
    <summary>Content</summary>
    
  ## content
   - Propriedade usada para acessar o conteúdo da resposta HTTP
   - Essa propriedade é do tipo HttpContent que fornece métodos para ler o conteúdo da resposta
  ```C#
  HttpClient httpClient = new HttpClient()                           
  string apiUrl = "https://api.example.com/data";                    // URL da requisiç~cao
  HttpResponseMessage response = httpClient.GetAsync(apiUrl).Result; // Classe utilizada para receber e manipular uma response por meio de métodos e propriedades 
  
  HttpContent content = response.Content;                          // A propriedade Content contém o conteúdo da resposta.
  ```
   ### Método ReadAsStringAsync()
    - Usado para ler o conteúdo como uma string,utilizado para conteúdo JSON
    ```C#
    string contentString = await content.ReadAsStringAsync();
    ```

  </details>
  <details>
    <summary>StatusCode</summary>

    ## StatusCode
    - Propriedade usada para retornar o código de status da resposta HTTP
    - A variável que contém o retorno é do tipo HttpStatusCode
    ```C#
    using System.Net.Http;

    // Crie uma instância de HttpClient
    using (HttpClient httpClient = new HttpClient())
    {
      string apiUrl = "https://api.example.com/data";
      HttpResponseMessage response = httpClient.GetAsync(apiUrl).Result;
    }

    ```
    ```C#
    // A propriedade StatusCode contém o código de status da resposta.
    HttpStatusCode status = response.StatusCode;

    if (status == HttpStatusCode.OK)
    {
        Console.WriteLine("A solicitação foi bem-sucedida (código 200 - OK).");
    }
    else if (status == HttpStatusCode.NotFound)
    {
      Console.WriteLine("A solicitação não encontrou o recurso (código 404 - Not Found).");
    }
    // Outros casos de verificação de códigos de status podem ser adicionados conforme necessário.

    ```
    </details>
    <details>
      <summary>IsSuccessStatusCode</summary>

    ## IsSuccessStatusCode
    - Verifica se o código de status da requisição foi bem sucedido
    - Devolve um boleano

    ```C#
    using System;
    using System.Net.Http;

    // Crie uma instância de HttpClient
    using (HttpClient httpClient = new HttpClient())
    {
      string apiUrl = "https://api.example.com/data";
      HttpResponseMessage response = httpClient.GetAsync(apiUrl).Result;

      // Use o método IsSuccessStatusCode para verificar se a solicitação foi bem-sucedida.
      if (response.IsSuccessStatusCode)
      {
          // A solicitação foi bem-sucedida, continue o processamento da resposta.
          string content = response.Content.ReadAsStringAsync().Result;
          Console.WriteLine("Conteúdo da resposta: " + content);
      }
      else
      {
          // A solicitação não foi bem-sucedida, lide com o erro de acordo.
          Console.WriteLine("A solicitação falhou com o código de status: " + (int)response.StatusCode);
      }
    }

    ```

    </details>
  

    </details>
    <details>
      <summary>Método GET</summary>
    
    ## Método GET
    ### Sintaxe do método GET
    - GetAsync();

    ```C#
    HttpClient httpClient = new HttpClient();
    string endeApi = endereço url da api
    HttpResponseMessage response = awai httpClient.GetAsync(endeApi);
    ``` 
      
    </details>
    <details>
      <summary>Método POST</summary>
    </details>
    <details>
      <summary>Método PUT</summary>
    </details>
    <details>
      <summary>Método DELETE</summary>
    </details>
  </details>
</details>
      
<details>
  <summary>Newtonsoft.json</summary>

  ## Newtonsoft.json
  - É uma biblioteca de serialização e desserialização de JSON que oferece uma maneira fácil de converter objetos C# em JSON e vice-versa.

  ### Serialização
  - Consiste em transformar um objeto ou string para o formato JSON
  - A serialização é usada para enviar dados por uma rede

  ### Desserialização
  - Consiste em transformar o objeto JSON em uma string ou objeto que pode ser lido pelo compilador.
  
  <details>
    <summary>Classe JsonConvert</summary>

  ## JsonConvert
  - Classe estática que fornece métodos para realizar operações relacionadas ao JSON

  ### SerializeObject
  - Recebe um objeto e devolve uma string JSON
  ```C#
  Person person = new Person { Name = "John", Age = 30 };
  string json = JsonConvert.SerializeObject(person);
  ```
  ### DeserializeObject
  - Recebe uma string JSON e devolve um objeto
  ```C#
  string json = @"{'Name':'Jane','Age':25}";
  Person person = JsonConvert.DeserializeObject<Person>(json);
  ```
    
  </details>
  
</details>

<details>
  <summary>biblioteca JSON.NET</summary>
    <details>
      <summary>Classe JArray</summary>

  ## JArray
  - Essa classe serve para representar arrays em formato JSON.
  - A classe JObject possui as mesmas características de JArray
  - Ambas as classes, JObject e JArray, herdam de JToken, o que significa que podem ser usadas de maneira genérica quando você não sabe se está trabalhando com um objeto JSON ou uma matriz JSON.
  
  **Importante :** 
  
  *A classe não é responsável pela desserialização completa de um JSON em um objeto C#. O JArray é útil quando você precisa interagir com dados em formato JSON sem convertê-los em objetos C#.*

  ### Serialização
  - Convertendo um array para JSON
  ```C#
  string json = nomeArray.ToString();
  ```
  ### Desserialização
  - Convertendo uma string JSON para um Array usando o método JArray.Parse
  ```C#
  string json = "[1, 2, 3, 4, 5]";
  JArray jsonArray = JArray.Parse(json);
  ```
    
    
  </details>
  
  <details>
    <summary>JToken</summary>
  
  ## JToken
  -  Ela é a classe base para muitos tipos de objetos que representam elementos em estruturas JSON, como objetos, matrizes, valores simples (números, strings, booleanos) e nulos.
  -  A classe JToken define várias propriedades e métodos comuns que são compartilhados por suas classes derivadas, incluindo JObject, JArray, JValue, JProperty
  -  Então muitas variaveis que recebem valores dessas classes podem ser do tipo JToken
    
  </details>
</details>

<details>
  <summary>API .NET</summary>

<details>
  <summary>Pastas e arquitetura do projeto</summary>
  
 ## Pastas e arquitetura do projeto.

 <details>
   <summary>Models</summary>
   
   ### Models
   *Aqui as classes são definidas.*
   
   **Anotações**
   
   **Table**
   - permite definir o nome da tabela no banco de dados.
  ```java
  [Table("Produtos")]
  public class Produto
  {
    // Propriedades da entidade Produto
  }
  ```

  **Auto incremente**
  - Declara o atributo como uma coluna auto-incrementável no banco de dados.
 ```c#
 [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
 ```

  **Key**
   - Indica que uma propriedade é a chave primária da entidade.
  ```java
  [Key]
  public int ClienteId { get; set; }
  ```

  **Required**
   - Indica que o preenchimento de uma propriedade é obrigatória.
  ```java
  [Required]
  public string Nome { get; set; }
  ``` 
   
 </details>
 <details>
   <summary>Controller</summary>
   
   ## Controllers
   *Contém as classes que definem os controladores da API e as ações que correspondem às solicitações.*
 
   ### ControllerBase
   -  fornece funcionalidades comuns e recursos que os controllers usam para atender às solicitações HTTP e enviar respostas aos clientes
   
   ### Route
   - Define o endpoint de um classe controlador
   
   ### ApiController
   - indica que esta classe é um Controller de API.
   
   ### HttpGet
   - Define qual será o verbo HTTP que um método irá responder após uma requisição para o endpoint da classe

   ```c#
  using Microsoft.AspNetCore.Mvc;

  [Route("api/[controller]")]
  [ApiController]
  public class MeuController : ControllerBase
  {
      // GET api/meu
      [HttpGet]
      public IActionResult Get()
      {
        // Lógica para buscar dados e retornar uma resposta HTTP
        var dados = new { Nome = "Exemplo", Descricao = "Isso é um exemplo." };
        return Ok(dados);
      }
   }

   ```



   
 </details>
 **Services**
  - Contém classes que fornecem lógica de negócios e serviçoes para os controladores.Essa classe ajuda  a manter o código do controlador enxuto,movendo a lógica complexa para classes de serviço.

</details>

<details>
  <summary>Entity Framework ORM</summary>
  
  ## Entity Framework ORM

  ### Classe DbContext
   - Classe fundamental no Entity Framework (EF), que é um ORM (Object-Relational Mapping).
   - Desempenha um papel crucial na interação entre o código da aplicação e o banco de dados.

  #### Método OnModelCreating
   - Usado para configurar o modelo de dados do aplicativo.
   - Permite definir como as entidades são mapeadas para tabelas no banco de dados.
   - ```Argumento modelBuilder``` usado para definir o modelo de dados e o mapeamento das entidades,bem como as suas chaves e relacionamento

  #### modelBuilder.Entity<ClasseModelo>
   -  Usado para obter uma instância do EntityTypeBuilder para uma determinada classe de entidade. O EntityTypeBuilder é uma classe que fornece uma API fluente para configurar o mapeamento de uma classe de entidade para uma tabela no banco de dados.
   -  Dentro do generics é passada a classe que você deseja configurar e mapear para o modelo relacional.
  
<details>
  <summary>Relacionamento Muitos para Muitos</summary>

  ## Relacionamento Muitos para Muitos
  
  

  -------------------------------------------------------------------------------------------------------------
  ```c# 
  HasKey(ug --> new {ug.UsuarioId, ig.GrupoId});
  ```
   - Lê-se: A entidade UsuarioGrupo terá como chave primária a combinação das propriedades UsuarioId e GrupoId.
   - Tradução HasKey: "Tem chave"
   - Configura a chave primária da entidade UsuarioGrupo.Ela indica que a chave primária é uma combinação das propriedades `UsuarioId` e `GrupoId`
  
  ```c#
    HasOne(x --> x.y)
  ```
   - Lê-se: a entidade x possui uma referência para a entidade y
   - Tradução HasOne: "Tem um"

  ```c#
    WithMany(x --> x.y)
  ```
   - lê-se: A entidade x  pode estar relacionada a muitos objetos y
   - Tradução WithMany: "Com muitos"
  
  ```c#
  HasForeignKey(x --> x.y)
  ```
   - lê-se: A entidade x possui como chave estrangeira o atributo UsuarioId.
   - Tradução HasForeignKey: "Tem chave estrangeira"

  ```c#
  public class Usuario
  {
    public int UsuarioId { get; set; }
    public string Nome { get; set; }
    public ICollection<UsuarioGrupo> UsuarioGrupos { get; set; }
  }

  public class Grupo
  {
    public int GrupoId { get; set; }
    public string Nome { get; set; }
    public ICollection<UsuarioGrupo> UsuarioGrupos { get; set; }
  }

  public class UsuarioGrupo
  {
    public int UsuarioId { get; set; }
    public Usuario Usuario { get; set; }
    public int GrupoId { get; set; }
    public Grupo Grupo { get; set; }
  }
  ```
  ```c#
  public class MeuDbContext : DbContext
  {
    public DbSet<Usuario> Usuarios { get; set; }
    public DbSet<Grupo> Grupos { get; set; }
    public DbSet<UsuarioGrupo> UsuarioGrupos { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<UsuarioGrupo>()
            .HasKey(ug => new { ug.UsuarioId, ug.GrupoId });

        modelBuilder.Entity<UsuarioGrupo>()
            .HasOne(ug => ug.Usuario)
            .WithMany(u => u.UsuarioGrupos)
            .HasForeignKey(ug => ug.UsuarioId);

        modelBuilder.Entity<UsuarioGrupo>()
            .HasOne(ug => ug.Grupo)
            .WithMany(g => g.UsuarioGrupos)
            .HasForeignKey(ug => ug.GrupoId);
    }
  }

  ```
</details>
<details>
  <summary>Relacionamento Um para Muitos</summary>

  ```c#
  HasMany(x --> x.y)
  ```
   - Lê-se: a entidade x possui uma referência para a entidade y
   - Tradução HasOne: "Tem muitos"

  ```c#
  WithOne(x --> x.y)
  ```
   - lê-se: A entidade x  pode estar relacionada a muitos objetos y
   - Tradução WithOne: "Com um"
  
  ```c#
  HasForeignKey(x --> x.y)
  ```
   - lê-se: A entidade x possui como chave estrangeira o atributo UsuarioId.
   - Tradução HasForeignKey: "Tem chave estrangeira"
  
</details>
<details>
  <summary>Relacionamento Um para Um</summary>

   ```c#
  HasOne(x --> x.y)
  ```
   - Lê-se: a entidade x possui uma referência para a entidade y
   - Tradução HasOne: "Tem muitos"

  ```c#
  WithOne(x --> x.y)
  ```
   - lê-se: A entidade x  pode estar relacionada a muitos objetos y
   - Tradução WithOne: "Com um"
  
  ```c#
  HasForeignKey(x --> x.y)
  ```
   - lê-se: A entidade x possui como chave estrangeira o atributo UsuarioId.
   - Tradução HasForeignKey: "Tem chave estrangeira"
  
  ```c#
  public class Pessoa
  {
    public int PessoaId { get; set; }
    public string Nome { get; set; }

    // Propriedade de navegação para o Endereco
    public Endereco Endereco { get; set; }
  }

  ```
  ```c#
  public class Endereco
  {
    public int EnderecoId { get; set; }
    public string Rua { get; set; }
    public string Cidade { get; set; }
    
    // Chave estrangeira para a Pessoa
    public int PessoaId { get; set; }
    
    // Propriedade de navegação para a Pessoa
    public Pessoa Pessoa { get; set; }
  }

  ```  
  ```c#
  protected override void OnModelCreating(ModelBuilder modelBuilder)
  {
    modelBuilder.Entity<Pessoa>()
        .HasOne(p => p.Endereco)
        .WithOne(e => e.Pessoa)
        .HasForeignKey<Endereco>(e => e.PessoaId);
  }
  ```
  
</details>
<details>
  <summary>Conexão com o banco de dados</summary>

  ## Conexão com o banco de dados
  
  ### Contexto: Configuração entre as classes e o banco de dados

  **Dependências a serem instaladas**
  
  ```
  dotnet add package Microsoft.EntityFrameworkCore
  dotnet add package Microsoft.EntityFrameworkCore.Tools
  ```

  **Criação da pasta Data**
  - Dentro dessa pasta serão guardados os arquivos que irão fazer o contexto entre as classes e o banco de dados

  ```c#
  using Microsoft.EntityFrameworkCore;

  public class ApplicationDbContext : DbContext
  {
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    public DbSet<Pessoa> Pessoas { get; set; }
    public DbSet<Grupo> Grupos { get; set; }
  }

  ```

  ```c# 
  public DbSet<Pessoa> Pessoas { get; set; }
  ```
   -  Define uma propriedade chamada Pessoas que representa a tabela no banco de dados para a entidade Pessoa.

  ### Configurando o banco de dados
  ```
  {
    "Logging": {
      "LogLevel": {
        "Default": "Information",
        "Microsoft.AspNetCore": "Warning"
      }
    },
    "AllowedHosts": "*",
    "ConnectionStrings": {
      "ApplicationDbConnection": "server=localhost;database=filme;user=root;password=root"
    }
  }
  ```

  ### Conectando o banco

  **Dependências do MySQL**
  ```
  dotnet add package Pomelo.EntityFrameworkCore.MySql
  dotnet add package MySql.Data.EntityFrameworkCore
  ```
  
  **Arquivo Program.cs**
  ```c#
  using FilmesApi.Data;
  using Microsoft.EntityFrameworkCore;

  var builder = WebApplication.CreateBuilder(args);

  var connectionString = builder.Configuration.GetConnectionString("ApplicationDbConnection");

  builder.Services.AddDbContext<FilmeContext>(opts =>
    opts.UseMySql(connectionString, ServerVersion.AutoDetect(connectionString)));
  ```
  
</details>
  
  ### Anotações

  **Table**
   - permite definir o nome da tabela no banco de dados.
  ```java
  [Table("Produtos")]
  public class Produto
  {
    // Propriedades da entidade Produto
  }
  ```

  **Key**
   - Indica que uma propriedade é a chave primária da entidade.
  ```java
  [Key]
  public int ClienteId { get; set; }
  ```

  **Required**
   - Indica que o preenchimento de uma propriedade é obrigatória.
  ```java
  [Required]
  public string Nome { get; set; }
  ```

  


  

  
</details>






  
</details>



pasta data
  arquivos de mapeamento objeto relacional

appsettings.json
arquivo onde sera declarado as configuracoes com o banco










