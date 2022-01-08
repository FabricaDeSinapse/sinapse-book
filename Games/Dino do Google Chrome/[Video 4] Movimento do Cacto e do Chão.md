# Jogo do Dino: Adicionando Obstáculos 

Neste tutorial iremos adicionar os obstáculos, e também adicionar movimentação aos obstáculos e ao chão.

## Código Completo

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movimentar : MonoBehaviour
{
    public Vector2 direcao;

    public float velocidade;

    private void Update()
    {
        transform.Translate(direcao * velocidade * Time.deltaTime);
    }
}

```

## Adicionando Obstáculos

Inicialmente vá até a Unity, pois  iremos separar as imagens dos obstáculos pré adicionadas, para isto.

> Vá até `Project` > Clique na pasta `Assets` > Selecione a pasta `Sprites` > Selecione `CactosFinos`.

![](imagens/Select_Cactos_Finos.PNG)



Após executar os passos acima, será necessário separar os cactos que estão juntos na imagem.

> Selecione `CactosFinos` > Vá até a guia `Inspector`> Desça até `Sprite Mode ` > Selecione a opção `Multiple` > Desça e clique em `Apply` > Selecione a opção `Sprite Editor`.

![](imagens/Sprite_Mode.PNG)

> Uma tela com 6 cactos será aberta, clique em `Slice` > Altere o `Type` de Automatic para Grid By Cell Count > No campo `Column & Row` altere o valor de `C` que estará como 1 para 3 > Clique em `Slice`

Isto fara com que nossa imagem seja divida em 3 partes iguais.

![](imagens/Sprites_Editor.PNG)

> Voltando para tela inicial da Unity verifique se a divisão da imagem foi feita corretamente em Sprites. Expandindo `CactosFinos` verifique se as imagens (CactosFinos_0, CactosFinos_1 e CactosFinos_2) foram criadas. Clique em `CactosFinos_0` e arraste até `SampleScene`.

![](imagens/Sample_Scene.PNG)

Após executar este passo, perceba que o cacto foi levado até a nossa cena, porém a escala das imagens estão diferentes. Nosso Dino está muito maior que cacto. Será necessário fazer a alteração da escala do Cacto. 

As dimensões da imagem do nosso Dino são 761x94, e as dimensões da imagem do cacto recém adicionado são 34x35. Fazendo uma conta rápida, dividindo a altura do nosso Dino pela altura do cacto (94/35=2,68), podemos ver que o Dino é quase 3 vezes maior, então iremos alterar os valores da escala do cacto para 2,68. 

> Selecione `CactosFinos_0` > Vá até a guia `Inspector` > Em `Transform` desça até a opção `Scale` e altere os valores de X,Y e Z para 2.68.

![](imagens/Scale_Cacto.PNG)

Antes de iniciar nosso código iremos fazer uma pequena alteração em nosso chão, deixando ele mais alinhado com o cacto e com o Dino.

> Vá até a guia `Hierarchy` > clique em `Chão` e vá até nossa `Scene` > Use a tecla `W` para usar o modo de movimentação e alinhe o Chão com os pés do Dino.

![](imagens/Alinhando_Chão.PNG)



## Explicando Código

Se observarmos o game original, podemos tirar duas conclusões. A primeira delas é que o cenário está se movimentando, fazendo com que o Dino tenha apenas que pular os obstáculos. E a segunda, é que o Dino se movimenta junto com a câmera pulando os obstáculos e vendo mais partes do mapa conforme ele avança. Nas duas situações o game irá funcionar, porém em nosso game iremos usar a forma que o os obstáculos se movimentam.

> Vá até Unity > Na guia `Hierarchy` selecione `CactosFinos_0` > Vá até a guia `Inspector` e desça até `Add Component` > Dê o nome do script de "Movimentar"> Tecle `Enter` duas vezes.

Sempre que um `Script` novo é criado pelo `Add Component` é fica localizado dentro da pasta Project/Assets. Para que fique organizado, clique no script recém criado e arraste para cima da pasta `Scripts`. 

> Clique duas vezes no `Script` recém criado (Movimentar). Iremos iniciar a criação do nosso script de movimentação.

O que nós queremos fazer em nosso Script é que nosso obstáculo se movimente, porém ainda não sabemos qual é a direção e ela será definida na Unity(Inspector).

> Inicialmente iremos adicionar uma variável publica, pois queremos que ele se movimente para uma direção(Horizontal/Vertical). Para isto usaremos a seguinte variável: 

```c#
public Vector2 direcao;
```

> iremos adicionar também uma variável publica que definirá a velocidade em que nosso obstáculo se movimenta.

```c#
public float velocidade;
```

A princípio não usaremos a função `Start`, então apague as linhas 10,11,12,13,14,15,16 e o comentário que está na linha 17. Iremos utilizar apenas a função `Update`. Certifique-se que a função `Update` está na linha 11. 

> Vá até a linha 11 e antes de `void Update()` adicione `private` que define qual é modificador de acessos e de variáveis. Verifique se o `private` está escrito com letra minúsculas. Salve as alterações feitas com `Ctrl + S`.

Iremos utilizar as seguintes funções:

```C#
 transform.Translate(direcao * velocidade * Time.deltaTime); 
```

* transform: Guarda as informações de posição do objeto.  

* translate: Movimenta o objeto em uma direção.

* direcao: Direção do movimento.
* velocidade: Velocidade do movimento.
* Time.deltaTime: Nos trás o tempo absoluto de frame a frame.

Desta forma temos o código completo de movimentação do nosso cacto. Salve (Ctrl +S) e volte para Unity.

## Configuração de Movimentação

Já na Unity espere compilar os dados e verifique se o script está funcionando corretamente, caso ocorra algum erro (apresentado no console), verifique se existe algum erro de digitação em nosso script.

> Vá até `Inspector` e desça até `Movimentação` iremos adicionar os valores para que nosso cacto se movimente. Com o `Play` desativado, altere o valor de X para -1. O -1 fará com o cacto se mova para esquerda. E altere a velocidade para 3. 

![](imagens/Alteracao_valores.PNG)

> Dê play na cena e verifique a movimentação do Cacto. Caso ele esteja muito perto do Dino, selecione `CactosFinos_0` > Vá até a `Scene` e arraste o cacto levemente para direita. Desta forma você irá pode verificar melhor a movimentação do cacto.

## Adicionando Movimento ao Chão

Voltando a analisar o jogo original, podemos notar que o chão se move junto com os obstáculos. E para fazer com que o chão se movimente também, iremos utilizar o mesmo script que criamos para o cacto.

>  Vá até `SampleScene` selecione Chão > Desça até `Project` e abra a pasta `Scripts` > Selecione o script `Movimentar` > Arraste até o lado esquerdo de `Add Component` e solte. 
>
> Com o chão selecionado, desça até `Movimentação` iremos adicionar os valores para que nosso chão se movimente. Com o `Play` desativado, altere o valor de X para -1. O -1 fará com o chão se mova para esquerda. E altere a velocidade para 3. 



## Concluindo

> Salve a cena e faça os últimos testes para garantir que tudo está funcionando como esperado.

Agora que o código de movimentação do cacto está pronto, suba as modificações no `GitHub` através de um `Commit` e um `Push`, para que elas fiquem salvas na nuvem.