# Melhorando Pulo

O intuito deste tutorial é ajustar o pulo do Dinossauro, podemos perceber que que ao teclar a `seta para cima` ou `UpArrow` o dinossauro pula, porém ao apertar mais vezes ele continua pulando no ar. Nosso objetivo é fazer com que o dinossauro pule apenas uma vez quando está no chão, e pare de pular quando está no ar.

##  Código Completo

Explicaremos o passo a passo para ajustar o pulo do dinossauro, caso queira testar a implementação, você pode usar o código completo, disponível a seguir.

```C# 
using System.Collections;
using System.Collections.Generic;
using UnityEditor;
using UnityEngine;

public class Jogador : MonoBehaviour
{
    public Rigidbody2D rb;
    public float forcaPulo = 700;

    public LayerMask layerchao;

    public float distanciaMinimaChao = 1;

    private bool estaNochao;

    // Start is called before the first frame update
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKey(KeyCode.UpArrow))
        {
            Pular();
        }
    }

    void Pular()
    {
        if (estaNochao)
        {
            rb.AddForce(Vector2.up * forcaPulo);
        }
    }

    private void FixedUpdate()
    {
        estaNochao = Physics2D.Raycast(transform.position, Vector2.down, distanciaMinimaChao, layerchao);
    }
}

```

## Explicando o código

Abrindo nosso script podemos notar que já temos a linha que faz com que o dinossauro pule ```if (Input.GetKey(KeyCode.UpArrow))``` e temos a linha que adiciona força ao pulo ```rb.AddForce(Vector2.up * forcaPulo);```. Agora será necessário checar se dinossauro está no chão, e para isto, iremos criar uma linha vertical que estará localizada na base do dinossauro apontada para baixo, e quando ela colidir com o chão, irá definir que ele está no chão. E caso não colida com nada, irá definir que ele não está no chão. Mas para isto, é necessário entender o conjunto de física da Unity:

* **RAYCAST** - Projeta um raio em uma direção, a partir de uma posição.
* **LAYER (Camada) **- Os gameobjects são organizados em layers (Camadas).

## Criando uma Layer para o projeto

> Para que possamos iniciar o processo de criação volte para Unity e em `SampleScene` selecione a opção `Chão`. Na guia `Inspector`, abaixo de `Chão` haverá a opção `Layer` que estará pré-definhada como `Default`, clique e selecione a opção `Add Layer` . Conforme mostrado abaixo adicione o nome Chão em `User Layer 8`.  Tecle `Enter ` e feche. Agora vá até a opção `Default` e altera para a Layer que acabamos de criar(`Chão`). 

![](E:\Githib\apostilas\Dino-video3\Add_Layer.PNG)

Volte para o Script pois faremos fazer a checagem. O primeiro passo será: criar uma variável "`public LayerMask layerchao`". Pois queremos que esta informação seja enviada para Unity e alterar qual é a layer que representa o chão. 

> Abaixo de `public float forcaPulo;` tecle `enter` e adicione a variável `public LayerMask layerchao;`

```c#
public Rigidbody2D rb;

public float forcaPulo = 700;

public LayerMask layerchao;
```

Como estamos trabalhando com física, é recomendado que estas funções sejam checadas pelo **Fixed Update**. A função **FixedUpdate**, assim como a **Update**, ele é executada constantemente. Porém, a diferença é que a função **FixedUpdate** não depende do número de FPS, nem interfere na quantidade de FPS que o jogo vai ter. Ela é sempre executada num intervalo de tempo fixo.

Agora iremos adicionar uma variável que define se o dino está no chão ou não. Para isso iremos definir uma variável do tipo bool(`booleana`) que tem apenas dois valores true ou false. 

> Abaixo de `public LayerMask layerchao;` tecle `Enter` e adicione a variável `private bool estaNochao;`.

``````c#
    public Rigidbody2D rb;

    public float forcaPulo;

    public LayerMask layerchao;

    private bool estaNochao;
``````

> Continuando nossa checagem, vá até o `FixedUpdate` e após a `{`adicione  `estaNochao = Physics2D.Raycast(transform.position, Vector2.down, 2f, layerchao)`. Salve e volte para Unity.

* `Physics2D`: configurações globais para a física 2D
* `Raycast:`Projeta um raio em uma direção, a partir de uma posição.

* `tranform.position`: Irá pegar a posição exata do nosso Dinossauro
* `Vector2.down`: É o mesmo que declarar `new Vector2(0, -1)`, ou seja:
  - Neutro no eixo X, representando sem movimentação horizontal (para os lados);
  - Negativo no eixo Y, representando movimentação vertical para baixo.

* `2f`: Será a distância que iremos testar.

```c#
}
    private void FixedUpdate()
    {
        estaNochao = Physics2D.Raycast(transform.position, Vector2.down, 2f, layerchao);
    }
}
```

> Voltando para Unity e selecione `Dinosauro` e na guia `Inspector` desça até `Jogador (Script)`. Perceba que `Layer Chao` está marcada como "Nothing". Selecione e altere para `Chão`.

![](E:\Githib\apostilas\Dino-video3\LayerMask_Layerchao.PNG)

Note que neste momento a variável que criamos `estaNochao` não está aparecendo pois está pré-definida com private e a Unity não mostra as variáveis privadas por padrão.

>  Para tornar ela visível, no canto superior direito, em baixo de `Layout` clique nos três pontinhos e selecione a opção `Debug.`

![](E:\Githib\apostilas\Dino-video3\Variável_Privada.PNG)

Dê play na cena e click na janela do "Game". Perceba agora que ao teclar a seta para cima, em determinado momento a variável `Esta No Chão` é assinalada como false e em outro momento com o dinossauro no ar ela é assinalada como true. Caso queira testar, com play ativado, vá até `Rigidbody 2D`, e zere o `Gravity Scale`. Vá até a Scene e arraste o dino para cima e perceba que em certa altura o `Esta No Chão` é contabilizado, vendo isto, teremos que encontrar o valor exato em que o dino não estará mais no chão. 

> Para que isto fique mais fácil, volte para o script pois iremos fazer uma alteração em nossa variável `EstaNoChão`. Altere o valor `2f` por `distanciaMinimaChao`. Voltando para parte superior do script, entre  `public LayerMask layerchao;` e ` private bool estaNochao;` adicione `public float distanciaMinimaChao = 1` . Salve e volte para Unity, Após o carregamento, verifique se a variável `DistanciaMinimaChao` foi criada.

![](E:\Githib\apostilas\Dino-video3\add_DistanciaMinimaChao.PNG)



Ao dar play na cena, veja que o valor que pré-definimos como (1) ainda não é o correto, pois teremos que encontra-lo manualmente. Para fazermos isto, com o play ativado, vá até `Rigidbody 2D` e zere a gravidade. Vá até a guia `Scene` e suba o dino lentamente até a altura limite em que a variável `Esta No Chao` se altera. Desça até `Jogador(Script)` e coloque o mouse em cima de `Distancia Minima Chao` e arraste lentamente para esquerda e encontre o valor que a variável `Esta No Chao` desative. No nosso caso o valor encontrado foi 0.8. 

Click no play novamente para desativa-lo e altere o valor de `Distancia Minima Chao` para 0.8. Salve e volte para o Script.

Agora ao invés de sempre aplicar o Add.force (rb.AddForce(Vector2.up * forcaPulo), iremos verificar se o dino está no chão ou não. 

> Em cima de `rb.AddForce(Vector2.up * forcaPulo);` adicione `if (estaNochao)`. Abra e feche chaves`{}` e coloque a variável que está abaixo `(rb.AddForce(Vector2.up * forcaPulo)`, dentro destas chaves. Aperte `Ctrl + S` para ajudar o código e apague a linha que ficou sobressalente. Aperte `Ctrl+S` novamente para salvar.

``````c#
{
        if (estaNochao)
        {
            rb.AddForce(Vector2.up * forcaPulo);
        }
    }
``````

Voltando para Unity, espere ela compilar e aperte play para testar. Perceba que ao teclar a seta para cima(Up arrow) o dino salta apenas uma vezes e não salta mais no ar. 

Em nossos testes, ao saltar diversas vezes o dino rotacionou, e para tirar a rotação é bem simples.

> Na parte superior direita, abaixo de `Layout`, click nos três pontinhos e selecione o modo normal.

![](E:\Githib\apostilas\Dino-video3\Desativando_Debug.PNG)

> Vá até `Rigidbody 2D`, click em `Constraints` e selecione a opção `Freeze Rotation (Z)`.

![](E:\Githib\apostilas\Dino-video3\Freeze_Rotation.PNG)



Com isto, nosso dino não irá mais rodar.

### Concluindo

> Salve a cena e faça os últimos testes para garantir que tudo está funcionando como esperado.

Suba as modificações no `GitHub` através de um `Commit` e um `Push`, para que elas fiquem salvas na nuvem.



