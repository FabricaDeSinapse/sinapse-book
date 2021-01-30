# Jogo do Dino: Programando o Pulo

Agora é hora de abrir o editor de códigos e começar a entender como construir o pulo do personagem, utilizando a linguagem de programação C#.

## Criação do Script

1. Abra o projeto que começamos na aula anterior;
2. Abra a cena `SampleScene`;
3. Selecione o `GameObject` do Dinossauro;
4. Vá até o `Inspector` e clique no botão `Add Component`;
5. Digite o nome do script, no caso será `Jogador` (certifique-se de que o nome começa com letra maiúscula);
6. Após a criação do script, click com o botão direto na pasta `Assets`, selecione `Create` > ` Folder`. Arraste o script(Jogador) para dentro da pasta. (Isso irá nos ajudar na organização dos Scripts).

## Configurando o Visual Studio Code

1. Abra o arquivo `settings.json`, localizado dentro da pasta `.vscode`;
2. Logo após a primeira linha do arquivo, que contém uma chave `{`, cole o seguinte trecho:

```json
 "editor.formatOnSave": true,
 "editor.formatOnType": true,
 "editor.formatOnPaste": true,
 "explorer.confirmDelete": false,
 "explorer.confirmDragAndDrop": false,
 "files.insertFinalNewline": true,
 "files.trimFinalNewlines": true,
 "files.trimTrailingWhitespace": true,
```

- Note que o trecho copiado começa na `linha 2` do arquivo e termina na `linha 9`, logo antes da linha 10 que contém o trecho `"files.exclude":`;

3. Aperte o atalho `Ctrl + S` duas vezes. Uma delas será para salvar as alterações. E a segunda para ativar uma das configurações("editor.formatOnSave": true,) que ajusta a formatação sempre que o arquivo é salvo.

## Programando Pulo

Ainda no Visual Studio Code, abra o arquivo Jogador.cs. Localizado dentro da pasta `Scripts`. 

Agora iremos iniciar a configuração do pulo, mas para isso e preciso entender duas funções. A primeira delas é o `Start`, ele é executado sempre que o jogo começar.(Será chamado uma vez para cada objeto que tenha um script). 

A segunda é o `Update` , Ele será chamado uma vez por quadro. Pense em um jogo de 60 FPS(60 quadros por segundo), a Unity irá chamar o `Update` 60 vezes por segundo.

Vamos adicionar o primeiro código que irá verificar todas as vezes que o jogador apertar a seta para cima faça com que o dinossauro pule.

1. Insira o comando `if (Input.GetKey(KeyCode.UpArrow))` e use o `Debug.Log(Pular)`; para validar se escrita do código está correta.

![E:\Githib\apostilas\Dino-video2](E:\Githib\apostilas\Dino-video2\Input.GekeyDown.PNG)

2. Volte para Unity, espere ela compilar as informações e verificar se há algum erro. Aperte `Play` na cena e após o inicio da cena aperte a `seta para cima`. Verifique se a mensagem "Pular" Aparece no Console.

![](E:\Githib\apostilas\Dino-video2\Teste1.PNG)

3. Volte para o Visual Studio Code e apague a linha Debug.Log(Pular).
4. Em seu lugar digite Pular(). (Uma mensagem de erro ira aparecer), Aperte `ESC`. 

![](E:\Githib\apostilas\Dino-video2\erro pular.PNG)

5. Depois da `}` do Update e antes da `}` que fecha a classe. Adicione a função (void Pular).

   Agora iremos precisar puxar o comando rigidbody que está na Unity, e para isso será necessário informá-lo. 

6. Após a `{` da linha 6 crie  duas linhas pressionando  `Enter` e adicione o comando `public rigidbody2D rb`.

![](E:\Githib\apostilas\Dino-video2\public rigidbody2D rb.PNG)

7. Salve(`CRTL+S`) e volte para Unity.

8. Na guia `Hierarchy`, selecione `Dinossauro`, e na guia `Inspector` desça ate `Jogador(Script)` e verifique se apareceu a variável (rb). Caso não, verifique o erro apresentado no console

![](E:\Githib\apostilas\Dino-video2\Inspector.PNG)

9. Em seguida click no `Dinossauro` e arraste ate `None (Rigidbody 2D)`.  Assim o script ira reconhecer o rigidbody que foi atrelado a ele.

![](E:\Githib\apostilas\Dino-video2\Arrastando rb Dino.PNG)

10. Volte ao Visual Studio Code.

## Adicionando Força ao pulo

1. Adicione o comando `rb.AddForce(Vector2.up * forcaPulo)`.

`rb` Acessar o rigidbody 

`ad.force` Adicionar forca

`vector2.up` Adicionar força vetorial no eixo Y (Vertical).

`Forcapulo` Descobrir a forca necessária para executar o pulo.

![](E:\Githib\apostilas\Dino-video2\forcaPulo.PNG)

2. Será necessário criar o "forcaPulo" pois iremos configurá-lo na Unity. Usando o comando `public float forcaPulo;`. Salve e volte para Unity

`float` Número decimal.

## Configurando Força e Gravidade

1. Na guia `Hierarchy`, selecione `Dinossauro`, e na guia `Inspector` desça ate `Jogador(Script)` e verifique se apareceu a variável `Forca Pulo`. Caso não verifique o erro apresentado no console. Altere o valor de `Forca Pulo` para 300.

![](E:\Githib\apostilas\Dino-video2\Força e Gravidade.PNG)

Você notará que o dinossauro ainda está demorando muito para cair, diferentemente do jogo apresentado pelo Google.

2. Em `Inspector` suba ate `Rigidbody 2D` e altere o valor da `Gravity Scale` para 5. Porem, após esta alteração você notara que a força adicionada ao pulo não é o suficiente e precisa ser aumentada. Altera para 800.

![](E:\Githib\apostilas\Dino-video2\Gravity Scale.PNG)

3. É importante verificar se todas estas alterações foram feitas com o `Play` acionado. Caso sim, será necessário realizar as alterações feitas no `Forca Pulo` e `Gravity Scale` novamente.