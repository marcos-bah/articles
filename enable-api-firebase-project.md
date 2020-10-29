# Habilite API's em um Projeto Firebase

Com um projeto Firebase criado, você poderá adicionar fiversas API's em sua aplicação, tudo de uma maneira bem simples.

>Lembre-se de estar com uma chave SHA-1 configurada de seu dispositivo no projeto Firebase.

## Habilitando

1. Acesse o site [Google Console](https://console.developers.google.com).   
  
2. Na parte superior esquerda, clique em `Selecione um projeto`. 
  
3. Um modal irá abrir, aparecerá todas as opções de projetos já criados, selecione o projeto que você criou no Firebase (mesmo nome).  

![Imagem01](https://github.com/marcos-bah/articles/blob/main/Docs/enable-api/01.png)

>Lembre-se de estar logado com a mesma conta do Firebase.

4. Feito isso, na parte supeior central há um campo de busca, escreva o nome da API que deseja incorporar ao seu projeto.

![Imagem01](https://github.com/marcos-bah/articles/blob/main/Docs/enable-api/02.png)

5. O site irá carregar todas as API's encontadas, selecione a de sua escolha.

![Imagem01](https://github.com/marcos-bah/articles/blob/main/Docs/enable-api/03.png)

6. O site te encaminhará para uma página daquela API, um botão em azul `Habilitar` aparecerá. Clique-o. Caso contrário, terá um botão em azul `Gerenciar`. Clique-o.

> Como a API já estava habilitada, apareceu para mim, `Gerenciar`.

![Imagem01](https://github.com/marcos-bah/articles/blob/main/Docs/enable-api/04.png)

7. Após isso o site deve encaminhar-te para uma página de gerencia da API.

8. Pronto! Sua API está **habilitada**.

## Configurando Credenciais

1. Caso já tenha habilitado a API, entre novamnete no site [Google Console](https://console.developers.google.com).

2. Cumpra os passo 2 até o passo 7 e clique em `Gerenciar`. Caso já não tenha feito.

3. No *drawer* esquerdo, selecione `Credenciais`.

![Imagem01](https://github.com/marcos-bah/articles/blob/main/Docs/enable-api/05.png)

4. Se você configurou corretamente seu projeto no Firebase as credenciais `IDs do cliente OAuth 2.0` devem aparecer, basta copiar e colar na parte requisitada em seu código.

![Imagem01](https://github.com/marcos-bah/articles/blob/main/Docs/enable-api/06.png)

5. Pronto! Sua credencial está **configurada**.


