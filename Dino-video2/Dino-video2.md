# Jogo do Dino: Programando o Pulo

Agora é hora de abrir o editor de códigos e começar a entender como construir o pulo do personagem, utilizando a linguagem de programação C#.

## Criação do Script

1. Abra o projeto que começamos na aula anterior;
2. Abra a cena `SampleScene`;
3. Selecione o `GameObject` do Dinossauro;
4. Vá até o `Inspector` e clique no botão `Add Component`;
5. Digite o nome do script, no caso será `Jogador` (certifique-se de que o nome começa com letra maiúscula);
6. ---COLOCAR PASSOS DE CRIAR PASTA PARA ORGANIZAR O PROJETO

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

```csharp
void Start()
{
    var test = 1;
}
```

