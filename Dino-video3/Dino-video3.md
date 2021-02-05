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

Para que possamos iniciar o processo de criação volte para Unity e em `SampleScene` selecione a opção `Chão`. Na guia `Inspector`, abaixo de `Chão` haverá a opção `Layer` que estará pré-definada como `Default`, clique e selecione a opção `Add Layer` . Conforme mostrado abaixo adicione o nome Chão em `User Layer 8`.  Tecle `Enter ` e feche. Agora vá até a opção `Default` e altera para a Layer que acabamos de criar(`Chão`). (Colocar como comentário)

![](E:\Githib\apostilas\Dino-video3\Add_Layer.PNG)





















































### Concluindo

> Salve a cena e faça os últimos testes para garantir que tudo está funcionando como esperado.

Agora que o código do pulo está pronto, suba as modificações no `GitHub` através de um `Commit` e um `Push`, para que elas fiquem salvas na nuvem.



