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

  **Recuperando valores**
   - Para ter acesso a algum valor, basta usar a chave.
  ```C#
  String nomeCarro = meuDicionario["nomeCarro"];  //onix
  ```

  **TryGetValue**
   - Verificação de chaves
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

  
  </details>
</details>

