# Estudo sobre C#
<details>
  <summary>Montagem de Ambiente</summary>

  # Montagem de ambiente

  ## Comandos mais usados
  ```json
  dotnet -h 		: lista de comandos
  dotnet new + template	: comando para criar um projeto
  dotnet run		: comando para rodar a aplicação
  mkdir + nomePasta	: comando para criar pasta
  ```
  
</details>
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
   
   ## Método GET
   
   ### Controllers
   *Contém as classes que definem os controladores da API e as ações que correspondem às solicitações.*
 
   #### ControllerBase
   -  fornece funcionalidades comuns e recursos que os controllers usam para atender às solicitações HTTP e enviar respostas aos clientes
   
   #### Route
   - Define o endpoint de um classe controlador
   
   #### ApiController
   - indica que esta classe é um Controller de API.
   
   #### HttpGet
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
   ## Método POST

   #### Atributo readOnly
   - Só pode ser atribuído um valor durante sua inicialização ou dentro do construtor da classe.
   - Após a inicialização, o valor de um atributo readonly não pode ser alterado.
   
   #### FromBody
   - Especifica que o modelo de dados deve ser extraído do corpo da requisição.

   #### Método Add
   - Usado para colocar uma entidade no estado "Added" (adicionado) no contexto, no entanto ainda não está salvo.

   #### Método SaveChanges
   - Efetua as alterações no banco de dados com uma query de insert.

   #### Método CreatedAtAction()
   - 

  ```c#
  public class FilmeController : ControllerBase
  {
    private readonly FilmeContext _context;

    public FilmeController(FilmeContext context)
    {
        _context = context;
    }

    [HttpPost]
    public IActionResult AdicionaFilme([FromBody] Filme filme)
    {
        _context.Filmes.Add(filme);
        _context.SaveChanges();

        return CreatedAtAction(nameof(RecuperaFilmePorId), new { id = filme.Id }, filme);
    }
  }

  ```
   ## Método PUT
   
 


   
 </details>
 **Services**
  - Contém classes que fornecem lógica de negócios e serviçoes para os controladores.Essa classe ajuda  a manter o código do controlador enxuto,movendo a lógica complexa para classes de serviço.

</details>

<details>
  <summary>Relacionamento</summary>

  ## Relacionamento

  ### Passo a passo de um relacionamento
   - Criacão da classe.
   - Adicionar na classe Context as propriedades que irão representar as classes no banco de dados.
   - Relacionamento.

  #### Classe DbContext
   - A classe ficará guardada dentro de uma pasta chamada Data
   - Classe fundamental no Entity Framework (EF), que é um ORM (Object-Relational Mapping).
   - Desempenha um papel crucial na interação entre as classes da aplicação e o banco de dados.

  ```c# 
  public DbSet<nomeClasse> nomeVariavel { get; set; }
  ```
   -  Define uma propriedade que representa a tabela no banco de dados para a uma determinada entidade.
   -  Usado para consultar e salvar instâncias da entidade no banco de dados.

  #### Relacionamento utilizando public virtual ou OnModelCreating
   - No caso de relacionamentos do tipo 1:1 e 1:N, o public virtual pode ser usado tranquilamente ou até mesmo omitido,pois o Entity Framework irá entender o relacionamento.
   - O método OnModelCreating é indicado para relacionamento do tipo N:N,pois exigem configurações específicas como chave primária,estrangeira e etc.
  
  #### Método OnModelCreating
   - Usado para configurar o modelo de dados do aplicativo.
   - Permite definir como as entidades são mapeadas para tabelas no banco de dados.
   - ```Argumento modelBuilder``` usado para definir o modelo de dados e o mapeamento das entidades,bem como as suas chaves e relacionamento

  #### modelBuilder.Entity<ClasseModelo>
   -  Usado para obter uma instância do EntityTypeBuilder para uma determinada classe de entidade. O EntityTypeBuilder é uma classe que fornece uma API fluente para configurar o mapeamento de uma classe de entidade para uma tabela no banco de dados.
   -  Dentro do generics é passada a classe que você deseja configurar e mapear para o modelo relacional.

  ##### Métodos do modelBuilder
  
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



<details>
  <summary>Relacionamento 1:1</summary>

  ## Relacionamento entre Cinema e Endereco

 - Um cinema deve conter obrigatoriamente um endereço.
 - Um endereço não precisa ter um cinema para existir.
 - Um cinema deve possuir no máximo um único endereço.
 - Na tebela cinema, deve existir uma coluna para fazer refência a um cinema.
 - A maneira de indicar que essas duas tabelas possuem um relacionamento é adicionando o atributo public virtual.

  ### Criação das classes
```c#
public class Cinema
    {
        [Key]
        [Required]
        public int Id { get; set; }
        [Required(ErrorMessage = "O campo de nome é obrigatório.")]
        public string Nome { get; set; }
        public int EnderecoId { get; set; }
        public virtual Endereco Endereco { get; set; }

    }
```
```c#
 public class Endereco
    {
        [Key]
        [Required]
        public int Id { get; set; }
        public string Logradouro { get; set; }
        public int Numero { get; set; }
        public virtual Cinema Cinema { get; set; }
    }
```

### Contexto entre as classes e o banco de dados.

```c#
public class FilmeContext : DbContext
{
  public FilmeContext(DbContextOptions<FilmeContext> opts) : base(opts)
  {
  }

  public DbSet<Cinemas>Cinemas{get;set;}
  public DbSet<Endereco>Enderecos{get;set;}

}
```
  
</details>
<details>
  <summary>Relacionamento 1:N</summary>
  
  ## Relacionamento entre Sessão e Filme

 - Para uma seção existir, é necessário que ela esteja associada a um filme.
 - Uma secão deve estar associada no máximo a um único filme.
 - Um filme pode ter várias seções.

 ### Criação das classes
```c#
using System.ComponentModel.DataAnnotations;

namespace FilmesApi.Models
{
    public class Sessao
    {
        [Key]
        [Required]
        public int Id { get; set; }

        [Required]
        public int FilmeId { get; set; }

        public virtual Filme Filme { get; set; }
    }
}
```
```c#
using System.ComponentModel.DataAnnotations;

namespace FilmesApi.Models;

public class Filme
{
    [Key]
    [Required]
    public int Id { get; set; }

    [Required(ErrorMessage = "O título do filme é obrigatório")]
    public string Titulo { get; set; }

    [Required(ErrorMessage = "O gênero do filme é obrigatório")]
    [MaxLength(50, ErrorMessage = "O tamanho do gênero não pode exceder 50 caracteres")]
    public string Genero { get; set; }

    [Required]
    [Range(70, 600, ErrorMessage = "A duração deve ter entre 70 e 600 minutos")]
    public int Duracao { get; set; }

    public virtual ICollection<Sessao> Sessoes { get; set; }
}
```
### Contexto entre as classes e o banco de dados
```c#
public class FilmeContext : DbContext
{
  public FilmeContext(DbContextOptions<FilmeContext> opts) : base(opts)
  {
  }

  public DbSet<Cinemas>Cinemas{get;set;}
  public DbSet<Endereco>Enderecos{get;set;}
  public DbSet<Filme>Filmes{get;set;}

}
```
  
</details>

<details>
  <summary>Relacionamento N:N</summary>

  ## Relacionamento entre Filme e Cinema
   - Um filme pode estar associado a vários cinemas.
   - Um cinema pode estar associado a vários filmes.

  ### Criando as classes.
   - o id do cinema foi removido
  
```c#
{
  public class Sessao
  {

    public int? FilmeId { get; set; }
    public virtual Filme Filme { get; set; }
    public int? CinemaId { get; set; }
    public virtual Cinema Cinema { get; set; }
  }
}
```

### Contexto entre as classes e o banco de dados
```c#
protected override void OnModelCreating(ModelBuilder builder)
{
  builder.Entity<Sessao>()
    .HasKey(sessao => new { sessao.FilmeId, sessao.CinemaId });

  builder.Entity<Sessao>()
    .HasOne(sessao => sessao.Cinema)
    .WithMany(cinema => cinema.Sessoes)
    .HasForeignKey(sessao => sessao.CinemaId);

  builder.Entity<Sessao>()
    .HasOne(sessao => sessao.Filme)
    .WithMany(filme => filme.Sessoes)
    .HasForeignKey(sessao => sessao.FilmeId);
}
```
</details>


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

<details>
  <summary>LINQ</summary>
  
  ## LINQ

  *Permite realizar consultas em fontes de dados utilizando uma sintaxe semelhante a SQL diretamente no código.*

   - O LINQ possui duas sintaxes de consulta: Sintaxe de Consulta e Sintaxe de Método.

  #### Sintaxe de Consulta
  ```c#
  var consulta = from livro in biblioteca
                  where livro.Autor == "AutorX"
                  select livro.Titulo;
  ```
  #### Sintaxe de Método
  ```c#
  var consulta = biblioteca
    .Where(livro => livro.Autor == "AutorX")
    .Select(livro => livro.Titulo);
  ```

  ### Método FirstOrDefault()
   - Usado para recuperar o primeiro elemento de uma sequência que atende a uma condição.
   - O método aceita uma callback em seu parâmetro e retorna true ou false.

  ```c#
  List<int> numeros = new List<int> { 1, 2, 3, 4, 5 };

  // Retorna o primeiro número maior que 2, ou 0 se nenhum número atender à condição.
  int primeiroMaiorQueDois = numeros.FirstOrDefault(num => num > 2);
  Console.WriteLine(primeiroMaiorQueDois);
  ```
  

  

  
  ### Consultas em métodos GET
  #### Retornando todos os registros: Método ToListAsync();
   - Converte e retorna todos os resultados de uma consulta em uma lista de objetos.
   - O método vem da propriedade DbSet que herda o DbContext
  
  #### return Ok(livros);
   - Este método pode ser usado para retornar dados junto com uma resposta HTTP de sucesso.
   - livros é passado como o objeto de conteúdo da resposta.

  #### Código para capturar o status code.
  ```c#
  var currentStatusCode = HttpContext.Response.StatusCode;
  ```
    
  ```c#
  [Route("api/[controller]")]
  [ApiController]
  public class LivroController : ControllerBase
  {
    private readonly BibliotecaContexto _contexto;

    public LivroController(BibliotecaContexto contexto)
    {
        _contexto = contexto;
    }

    [HttpGet]
    public async Task<IActionResult> GetLivros()
    {
        // Retorna todos os livros do banco de dados
        var livros = await _contexto.Livros.ToListAsync();

        return Ok(livros);
    }
  }
  ```





  
</details>
  
</details>

</details>




















