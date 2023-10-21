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
  get{return nome}
  set{nome = value}
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

    ### Métodos importantes da classe

    **EnsureSuccessStatusCode()**
    - Usado para verificar se uma resposta HTTP foi bem sucedida
    - Caso a requisicao não for bem sucedida, uma execeção sem tratamento será lançada, caso o erro não ocorra, a execução continua e o método não retorna nada.
    

    </details>
    <details>
      <summary>Método GET</summary>      
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













