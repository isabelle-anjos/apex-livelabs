# Executar o Aplicativo

## Introdução

Nos laboratórios anteriores, você configurou e utilizou o **Generative AI** em um aplicativo de Highschool que ajuda os pais a escolherem a melhor escola para seus filhos. Agora, chegou o momento de ver o aplicativo completo em ação.

**Nota:** As capturas de tela deste workshop foram feitas usando o **Dark Mode** no APEX 24.1.2.

**Tempo Estimado:** 5 minutos

---

### *Objetivos*

Neste laboratório, você irá:

- Executar o aplicativo
- Explorar os vários recursos do app

---

## Tarefa 1: Executar o Aplicativo

1. No **Page Designer**, clique em **Save and Run** para executar o aplicativo. Alternativamente, você pode ir até a página inicial do aplicativo e clicar em **Run Application**.
    ![Page Designer](images/save-and-run.png ' ')

2. Faça login no aplicativo usando sua conta do APEX.
    ![Tela de login do aplicativo](images/login.png ' ')

---

## Tarefa 2: Explorar os Recursos do App

1. Na página inicial, veja os cartões listando as escolas. Use os filtros (**Faceted Search**) para refinar os resultados. Selecione as seguintes opções em **Interest**:
    - **Science & Math**
    - **Computer Science & Technology**

    ![Página inicial com faceted search](images/apply-facet.png ' ')

2. Altere para a aba **Maps** para exibir as escolas no mapa.
    - Aplique o filtro de Distância: **Less than 5 Miles**
    - Aplique o filtro para **Borough:** **Manhattan**
    Isso reduz os resultados para 15 escolas.
    ![Mapa com escolas filtradas](images/map.png ' ')

3. Volte para a aba **Cards**. Para a escola **Manhattan Center for Science and Mathematics**, clique no ícone **Learn More (i)**. Uma interface de chat com o AI Assistant será exibida.

    ![Aba de busca com escolas](images/learn-more.png ' ')

4. Na interface de chat, selecione o chip de sugestão **Provide an overview of the school**. Pergunte sobre a escola em linguagem natural e receba respostas apropriadas. Exemplos de perguntas:
    - What language courses are taught here?
    - What advanced placement courses are taught at this school?

    Revise as respostas e feche o diálogo.
    ![Chat com AI Assistant](images/chat.png ' ')

5. Como um pai satisfeito com a escola, clique em **Apply** para a escola *Manhattan Center for Science and Mathematics*.
    ![Página com cartões](images/apply.png ' ')

6. Um drawer **Apply to School** será aberto para preenchimento. Insira o nome do estudante como **Joe** e clique em **Generate Letter**. Isso invocará o serviço Gen AI para gerar um e-mail.
    ![Drawer para aplicar na escola](images/student-name.png ' ')

7. Revise o e-mail gerado e faça alterações, se necessário. Clique em **Send Application**.
    ![Drawer para envio do e-mail](images/generate-letter.png ' ')

8. A aplicação foi enviada com sucesso.
    ![Página inicial após envio](images/apply-sent.png ' ')

9. Explore mais sobre escolas de NYC utilizando o botão **Ask a Question**.

    Clique em **Ask a Question** para abrir um diálogo de chat com uma mensagem de boas-vindas. Digite a seguinte pergunta:

    ```plaintext
    What are the top 3 reasons to choose a highschool in New York city?
    ```

    ![Chat com pergunta genérica](images/ask-a-q.png ' ')

    Descubra mais sobre as escolas de NYC fazendo perguntas e recebendo respostas do **Generative AI**!

---

## Resumo

Agora você sabe como executar o aplicativo e explorar os recursos de **Generative AI** no app.

---
