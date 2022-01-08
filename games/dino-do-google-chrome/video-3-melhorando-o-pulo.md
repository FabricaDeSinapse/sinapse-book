# Melhorando Pulo

Tá! Já temos o pulo funcionando, mas... percebe que quando apertamos a tecla de pular várias vezes em sequência, o dinossauro vira um foguete e vai ao infinito e além? Se fosse um jogo da NASA, tudo bem, mas como queremos reproduzir o comportamento exato do jogo oficial, precisamos corrigir isso. Partiu?

Nosso objetivo é fazer com que o dinossauro possa pular apenas quando estiver no chão e bloqueie novos pulos quando estiver no ar.

![To Infinity And Beyond](../../Games/Dino%20do%20Google%20Chrome/imagens/infinity-and-beyond.gif)

## Código Completo

Ao longo dos capítulos a seguir, explicaremos o passo a passo para ajustar o pulo do dinossauro. Caso queira testar a implementação, você pode usar o código completo, disponível a seguir:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Jogador : MonoBehaviour
{
    public Rigidbody2D rb;

    public float forcaPulo = 700;

    public LayerMask layerChao;

    public float distanciaMinimaChao = 1;

    private bool estaNoChao;

    // Start is called before the first frame update
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.UpArrow))
        {
            Pular();
        }
    }

    void Pular()
    {
        if (estaNoChao)
        {
            rb.AddForce(Vector2.up * forcaPulo);
        }
    }

    private void FixedUpdate()
    {
        estaNoChao = Physics2D.Raycast(transform.position, Vector2.down, distanciaMinimaChao, layerChao);
    }
}
```

## Explicando o código

Abrindo o script podemos notar que já temos o código que detecta quando a tecla foi pressionada e o código que adiciona uma força ao `Rigidbody`, fazendo com o que o dinossauro pule:

```
void Update()
{
    if (Input.GetKeyDown(KeyCode.UpArrow))
    {
        Pular();
    }
}
```

Para impedir que o dinossauro pule sempre que o jogador pressionar a tecla, precisamos validar quando dinossauro está no chão antes de adicionar uma força ao corpo.

Existem algumas formas de detectar se o jogador está no chão:

* A primeira é utilizando o método `OnCollisionEnter` ou `OnCollisionEnter2D` que é chamado sempre que um `Collider` com `Rigidbody` encosta em outro `Collider`, mesmo que esse segundo `Collider` não tenha um `Rigidbody` associado.
* A segunda é utilizando um `Raycast`, que desenha uma linha em uma direção e detecta se há algum `Collider` em cima dessa linha desenhada.

A primeira abordagem funciona bem na maioria dos casos e geralmente é a abordagem utilizada em diversos tutoriais. Entretanto, quando utilizamos o `OnCollisionEnter`, dependemos da colisão entre os dois objetos, fazendo com que um novo pulo só esteja liberado quando o jogador de fato toca o chão.

Quando estamos falando de game flow, podemos perceber que certos comportamentos ajudam o jogador a ter um fluxo de jogo que flui melhor e é mais natural. Quando liberamos um novo pulo para o jogador, mesmo que o `GameObject` ainda não esteja tocando o chão, compensamos o fato do jogador apertar a tecla um pouquinho antes de tocar o chão, fazendo com que o pulo não seja uma ação tão punitiva caso o jogador erre o tempo e pressione a tecla um pouquinho antes da colisão acontecer.

Portanto, utilizaremos o`Raycast`, desenhando ele para que comece na base do dinossauro e termine um pouco embaixo dele.

Essa linha irá detectar se há um objeto de chão (marcado com a layer `Chão`) próximo, definindo que o dinossauro está no chão, mesmo que não esteja encostando nele diretamente.

* **Raycast** - Projeta um raio em uma direção, a partir de uma posição.
* \*\*Layer (Camada) \*\*- As camadas da Unity servem para organizar os objetos, permitindo alterar, entre outras funções, a ordem de renderização e a interação do sistema de física.

## Criando uma Layer para o projeto

> Abra a `SampleScene` e selecione o `GameObject` `Chão`.
>
> Na aba `Inspector`, procure pela opção `Layer`, que estará com o valor definido como `Default`, e clique para abrir a lista de opções.
>
> Clique na opção `Add Layer`.
>
> Clique no input de `User Layer 8` e escreva o nome `Chão`, como mostrado na figura a seguir.
>
> Pressione `Enter` para confirmar e selecione novamente o `GameObject` `Chão`.
>
> Em `Layer`, clique na opção `Default` e altere para a camada que acabamos de criar.

![Add Layer](../../Games/Dino%20do%20Google%20Chrome/imagens/Add\_Layer.PNG)

Agora que definimos a nova camada no objeto, podemos utilizá-la para detectar colisões entre o Dinossauro e o Chão.

## Detectando se o Jogador está no Chão

O primeiro passo para detectar se o Jogador está no Chão é definir qual é a layer que corresponde ao chão. Para isso, utilizaremos uma variável `public` do tipo `LayerMask`, que permite com que essa layer seja definida no `Inspector`.

> Abra o script do `Jogador`.
>
> Logo abaixo da variável `forcaPulo`, declare o seguinte conteúdo:

```
public LayerMask layerChao;
```

Como estamos trabalhando com física, é recomendado que as colisões entre `Raycast` os objetos da cena sejam declaradas no `FixedUpdate`.

Assim como o método `Update`, o `FixedUpdate` também é executado constantemente. Entretanto, a diferença é que o `FixedUpdate` não é executado uma vez por quadro (frames per second) e sim num intervalo de tempo fixo que consiste em uma chamada a cada 0.02 segundos, resultando em 50 chamadas por segundo.

> Logo após o término do método `Pular`, declare o método `FixedUpdate` da seguinte maneira:

```
private void FixedUpdate()
{
}
```

Antes de declarar o `Raycast` precisamos de um local para guardar a informação de que o Dinossauro está ou não no chão. Para isso, declaramos uma variável pública do tipo `bool`.

> Logo após a variável `layerChao`, logo no começo do arquivo, declare o seguinte conteúdo:

```
private bool estaNoChao;
```

> Volte para o `FixedUpdate`.
>
> Dentro da chaves `{}`, declare o seguinte conteúdo:

```
private void FixedUpdate()
{
    estaNoChao = Physics2D.Raycast(transform.position, Vector2.down, 2f, layerChao)
}
```

> Salve o script.

Para entender um pouco mais da declaração do `Raycast`, precisamos entender cada pedacinho individualmente:

Estamos utilizando a seguinte declaração do `Raycast`:

`Physics2D.Raycast(Vector3 posicao, Vector3 direcao, float distancia, LayerMask layer)`

* `Physics2D`: Pacote da Unity para trabalhar com física 2D.
* `Raycast`: Projeta um raio em uma direção, a partir de uma posição.
* `tranform.position`: Pega a posição atual do `Transform` que está no mesmo `GameObject` que o script em questão. Nesse caso, o script `Jogador.cs` está no `GameObject` do Dinossauro, portanto, pegará a posição do Dinossauro.
* `Vector2.down`: É o mesmo que declarar `new Vector2(0, -1)`, ou seja, `0` no eixo `X` e `-1` no eixo `Y`. Como esse argumento do `Raycast` significa a direção da linha, esses valores indicam que a linha estará apontada para baixo.
* `2f`: O tamanho da linha.

## Testando o `Raycast`

> Volte para a Unity.
>
> Selecione o `GameObject` do Dinossauro.
>
> Na aba `Inspector`, procure pelo componente `Jogador (Script)`.
>
> Note que a variável `Layer Chao` apareceu (caso não esteja aparecendo, verifique se salvou o script ou se há erros no Console. Corrija os problemas até que a variável apareça corretamente).
>
> Clique na opção `Nothing` e altere para `Chao`. Isso fará com que a camada `Chao` seja definida como a camada de detecção de colisão pelo `Raycast`.

![Layer Chao](../../Games/Dino%20do%20Google%20Chrome/imagens/LayerMask\_LayerChao.PNG)

Note que a variável `estaNoChao` não aparece, pois ela está definida com `private` e a Unity exibe apenas variáveis que são `public` ou que estão marcadas como `[SerializeField]`, que cobriremos em outra oportunidade.

Para conseguir validar se o `Raycast` está marcando corretamente quando o Jogador está no chão, precisamos visualizar essa variável. Para visualizar o conteúdo de variáveis privadas, precisamos ativar o modo `Debug` do `Inspector`.

> Para ativar o modo `Debug` no `Inspector` vá no canto superior direito e, embaixo de `Layout`, clique nos três pontinhos e selecione a opção `Debug`, como mostrado na imagem a seguir.

![Ativando o Modo Debug do Inspector](../../Games/Dino%20do%20Google%20Chrome/imagens/Ativando\_Debug.PNG)

## Visualizando se o Jogador está no Chão

> Aperte o botão `Play` ou use o atalho `Ctrl + P`.
>
> Clique na aba `Game`.
>
> Pressione a tecla `UpArrow` (seta para cima) e verifique se a variável `estaNoChao` altera o valor conforme o Jogador se afasta do chão.

Observe que a variável só fica com o valor `true` quando o Jogador está muito próximo do chão. Para melhorar o game flow e permitir ao game designer testar diferente combinações de valores e ajustar o melhor valor possível, é interessante colocar uma variável pública no `Inspector` que permite esse ajuste fino.

> Volte para o script do `Jogador.cs`.
>
> Procure pela declaração do `Raycast` e altere o valor de `2f` para `distanciaMinimaChao`. Note que essa variável ainda não existe e a IDE deve mostrar um erro.
>
> Precisamos declarar essa variável. Vá para parte superior do script e, logo após a variável `layerChao`, declare o seguinte código:

```
public float distanciaMinimaChao = 1f;
```

> Salve o script e volte para Unity.
>
> Vá até o componente `Jogador (script)` e verifique se a variável `Distancia Minima Chao` apareceu, como mostrando na imagem a seguir:

![Variável DistanciaMinimaChao](../../Games/Dino%20do%20Google%20Chrome/imagens/DistanciaMinimaChao.PNG)

Agora basta ajustar o valor da variável para um que achar interessante, testando o jogo e validando se está com o game flow que acha interessante.

No nosso caso, ajustar a distância do chão para `0.8` funcionou de uma maneira interessante.

## Integrando a variável `estaNoChao` com a ação do pulo

Para finalizar o script, precisamos colocar uma checagem logo antes de realizar a ação do pulo.

> No script do `Jogador.cs`, vá até o método `Pular()`.
>
> Em volta da declaração do `AddForce`, adicione um `if` para checar se a variável `estaNoChao` possui o valor `true`, da seguinte maneira:

```
void Pular()
{
    if (estaNoChao)
    {
        rb.AddForce(Vector2.up * forcaPulo);
    }
}
```

> Salve o script e volte para a Unity.
>
> Aperte o botão `Play` e teste o novo comportamento. Note que só conseguimos realizar um novo pulo quando o Dinossauro está próximo do chão.

## Desativando a rotação do `Rigidbody`

Em alguns casos, o Dinossauro está rotacionando logo após o pulo, fazendo com que ele caia e fique com um comportamento estranho. Uma forma de resolver isso de maneira bem simples é desativando a rotação do componente `Rigidbody`.

> Primeiro, precisamos desativar o modo `Debug` do `Inspector`.
>
> Para isso, vá até a parte superior direita do `Inspector` e, abaixo de `Layout`, clique nos três pontinhos.
>
> Selecione a opção `Normal`.

![Desativando o modo Debug](../../Games/Dino%20do%20Google%20Chrome/imagens/Desativando\_Debug.PNG)

> No `Jogador`, vá até o componente `Rigidbody 2D`.
>
> Clique em `Constraints` e ative a opção `Freeze Rotation (Z)`.

![Freeze Rotation](../../Games/Dino%20do%20Google%20Chrome/imagens/Freeze\_Rotation.PNG)

Com isto, o Dinossauro ficará sempre na mesma rotação para o eixo `Z`.

> Salve a cena.

### Concluindo

Nesse tutorial trabalhamos com algumas funções do pacote de física da Unity, aprendemos como o `Raycast` pode ser muito útil para detectar colisões entre camadas, entendemos que funções de física precisam ser declaradas no `FixedUpdate`, descobrimos uma forma de visualizar a informação de variáveis privadas e descobrimos que existem várias maneiras de realizar o mesmo comportamento, mas que algumas permitem a construção de um game flow mais interessante.

Para finalizar, suba as modificações no `GitHub` para que elas fiquem salvas na nuvem e você não perca o progresso de desenvolvimento do jogo.
