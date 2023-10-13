# Estudo sobre C#

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


    
    
  </details>
</details>

