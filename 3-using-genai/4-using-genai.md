# Criar um Chat Conversacional sobre Escolas com Generative AI

## Introdução

Neste laboratório, você aprenderá a construir uma consulta conversacional sobre escolas usando **Generative AI**, onde o usuário pode fazer perguntas sobre uma escola via chat, e o widget de chat utiliza **Generative AI** para fornecer respostas sensíveis ao contexto. Este laboratório utiliza o mais recente recurso do APEX 24.1 chamado **Open AI Assistant**.

**Tempo Estimado:** 20 minutos

---

### *Objetivos*

Neste laboratório, você:

- Configurará um serviço de **Generative AI** no seu workspace
- Criará um chatbot conversacional usando **Generative AI**

---

## Tarefa 1: Configurar o Serviço de Generative AI

Para usar o serviço de **Generative AI** no APEX, você deve configurá-lo no nível do workspace.

1. No **App Builder**, navegue até **Workspace Utilities > All Workspace Utilities**.

    ![Página inicial do Workspace](images/ws-utilities.png ' ')

2. Selecione **Generative AI**.

    ![Página de Workspace Utilities](images/select-genai.png ' ') 

3. Clique em **Create** para configurar um serviço de **Generative AI**.

     ![Página de serviços Gen AI](images/create-genai.png ' ') 

4. Neste laboratório, você usará o **OCI Generative AI Service** como o provedor de IA. Insira/seleciona as seguintes informações:

    - **AI Provider:** **OCI Generative AI Service**  
    - **Name:** **OCI Gen AI**  
    - **Static ID:** **oci\_gen\_ai**  
    - **Compartment ID:** Insira o ID do Compartimento OCI. Consulte a [Documentação](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/contactingsupport_topic-Locating_Oracle_Cloud_Infrastructure_IDs.htm#:~:text=Finding%20the%20OCID%20of%20a,displayed%20next%20to%20each%20compartment.) para localizar o ID do Compartimento. Caso tenha apenas um compartimento, use o OCID do arquivo de configuração salvo no **Lab 3**.  
    - **Region:** **us-chicago-1** (Atualmente, o serviço OCI Generative AI está disponível em regiões limitadas)  
    - **Model ID:** **meta.llama-3-70b-instruct** (Você pode selecionar outros modelos disponíveis. Consulte a [documentação](https://docs.oracle.com/en-us/iaas/Content/generative-ai/use-playground-chat.htm#chat))  
    - **Used by App Builder:** Habilite o botão de alternância para **ON**. (O **Base URL** será gerado automaticamente.)  
    - **Credentials:** **apex\_ai\_cred**  

    Clique em **Create**.

    ![Página de serviços Gen AI](images/oci-gen-ai-details.png ' ')

---

## Tarefa 2: Criar a Página de Chat

1. Navegue até a página inicial do aplicativo e clique em **Create Page**. Selecione **Blank Page**.

    ![Página inicial do aplicativo](images/create-blank-page.png ' ')

2. Na janela de diálogo **Create Blank Page**, insira/selecione o seguinte:
    - **Page Number:** **3**  
    - **Name:** **Learn More**  
    - **Page Mode:** **Modal Dialog**  

    Clique em **Create Page**.

    ![Assistente Create Page](images/learn-more.png ' ')

3. Com **Page 3: Learn More** selecionado na **Rendering Tree**, insira/selecione o seguinte no **Property Editor**:
    - **Appearance > Template Options:**
        - **General:** Marque **Remove Body Padding**  
        - **Content Padding:** **Remove Padding**

    ![Configuração de Template](images/learn-more-template.png ' ')

4. Na **Rendering Tree**, abaixo de **Components**, clique com o botão direito em **Content Body** e selecione **Create Region**.

    ![Criar Região](images/create-region.png ' ')

5. No **Property Editor**, insira/selecione o seguinte:

    - **Identification > Name:** **Chat**

        ![Configuração de Identificação](images/chat-to1.png =40%x*)

    - **Appearance > Template Options:**
        - **Common:**
            - **General:** Marque **Remove Body Padding**  
            - **Body Height:** **320px**  
            - **Header:** **Hidden**  
        - **Advanced > Bottom Margin:** **None**

        ![Configuração de Aparência](images/chat-to2.png =50%x*)

    - **Advanced > Static ID:** **chat**  
        ![Configuração Avançada](images/chat-to3.png =40%x*)

## Tarefa 3: Configurar o Contexto do Prompt

### 1. Criar Page Items
1. Na **Rendering Tree**, clique com o botão direito em **Content Body** e selecione **Create Page Item**.

    ![Page Designer](images/create-page-item.png ' ')

2. No **Property Editor**, insira/selecione o seguinte:

    - **Under Identification**:
        - **Name:** P3\_SCHOOL\_ID
        - **Type:** Hidden

    ![Page Designer](images/school-id.png ' ')

3. Repita o passo 1 para criar outro Page Item. Em seguida, insira/selecione o seguinte no **Property Editor**:

    - **Under Identification**:
        - **Name:** P3_CONTEXT
        - **Type:** Hidden

    ![Page Designer](images/context-item.png ' ')


4. Na **Rendering Tree**, clique com o botão direito em **P3_CONTEXT** e selecione **Create Computation**.

    ![Page Designer](images/create-computation.png ' ')

5. No **Property Editor**, insira/selecione o seguinte:

    - **Execution > Point:** Before Regions
    - **Under Computation**:
        - **Type:** SQL Query (return single value)
        - **SQL Query:** Usaremos o **APEX Assistant** para gerar a query. Clique no ícone **Code Editor**.

    ![Page Designer](images/compute-sql.png =40%x*)

6. No **Code Editor**, clique em **APEX Assistant** para abrir a interface de chat com o assistente de IA. Se aparecer uma janela solicitando aceitar os **Terms and Conditions**, clique em **Accept**.

    Insira o seguinte prompt no chat e clique em **Send**:

    ```plaintext
    Help me create a query that returns only one column concatenating the following information for the HIGHSCHOOLS table. Provide an alias for the column name as prompt_context.

    Overview of the school,
    Language Courses,
    Advanced Placement Courses,
    Diversity in Admission Policy,
    Extra Curricular Activities,
    Public Schools Athletic League (PSAL) sports for boys,
    Public Schools Athletic League (PSAL) sports for girls,
    Facilities,
    Academic opportunities,
    Attendance rate,
    Graduation rate

    filtering by the id of the school
    ```

    ![Page Designer](images/enter-prompt.png ' ')

7. O **AI Assistant** sugerirá uma query SQL. Você pode refinar o resultado. Após estar satisfeito, clique em **Insert**.

    ![Page Designer](images/insert-query.png ' ')

8. A query será inserida no **Code Editor**. Substitua *your\_school\_id* por **:P3_SCHOOL_ID** e clique em **Validate**. A query deve ficar assim:

    ```sql
    SELECT 'Overview of the school: ' || OVERVIEW_PARAGRAPH || chr(10) || chr(13) ||
           'Language Courses: ' || LANGUAGE_CLASSES || chr(10) || chr(13) ||
           'Advanced Placement Courses: ' || ADVANCED_PLACEMENT_COURSES || chr(10) || chr(13) ||
           'Diversity in Admission Policy: ' || DIADETAILS || chr(10) || chr(13) ||
           'Extra Curricular Activities: ' || EXTRACURRICULAR_ACTIVITIES || chr(10) || chr(13) ||
           'Public Schools Athletic League (PSAL) sports for boys: ' || PSAL_SPORTS_BOYS || chr(10) || chr(13) ||
           'Public Schools Athletic League (PSAL) sports for girls: ' || PSAL_SPORTS_GIRLS || chr(10) || chr(13) ||
           'Facilities: ' || ADDTL_INFO1 || chr(10) || chr(13) ||
           'Academic Opportunities: ' || ACADEMIC_OPPORTUNITIES || chr(10) || chr(13) ||
           'Attendance rate: ' || ATTENDANCE_RATE || chr(10) || chr(13) ||
           'Graduation rate: ' || GRADUATION_RATE as prompt_context
    FROM HIGHSCHOOLS
    WHERE ID = :P3_SCHOOL_ID;
    ```

    ![Page Designer](images/edit-validate.png ' ')

9. Se a validação for bem-sucedida, clique em **OK**.

    ![Page Designer](images/success-ok.png ' ')

---

## Tarefa 4: Criar uma Ação Dinâmica para o Chat Widget

1. Na **Rendering Tree**, vá para a aba **Dynamic Actions**, clique com o botão direito em **Page Load** e selecione **Create Dynamic Action**.

    ![Page Designer Dynamic Actions](images/create-da.png ' ')

2. No **Property Editor**, insira **Name:** Open AI Assistant - Chat.

    ![Page Designer Dynamic Actions](images/da-name.png ' ')

3. Na **True action**, selecione **Show**. No **Property Editor**, insira/selecione o seguinte:
    - **Action:** Open AI Assistant  
    - **Generative AI > Service:** OCI Gen AI  
    - **System Prompt:**  
        ```plaintext
        Use the below context to answer all questions:

        '''

        &P3_CONTEXT.

        '''

        If the question cannot be answered based on the above context, say "Information Not Found!".
        ```
    - **Welcome Message:** Welcome! How may I help you?  
    - **Appearance > Display as:** Inline  
    - **Container Selector:** #chat  

    ![Page Designer Dynamic Actions](images/true-action.png ' ')

4. Em **Quick Actions**, insira:
    - **Message 1:** Provide an overview of the school  
    - **Message 2:** What is the graduation rate?  

    ![Page Designer Dynamic Actions](images/quick-action.png =40%x*)

Clique em **Save**.

---

## Tarefa 5: Criar Ação para Abrir o Chat

1. Na página **Search and Apply**, acesse a **Rendering Tree**, vá para **Body > Search Results**, clique com o botão direito em **Actions** e selecione **Create Action**.

    ![Page Designer](images/create-action.png =40%x*)

2. No **Property Editor**, configure:
    - **Type:** Button  
    - **Label:** Learn More  
    - **Target Page:** 3  
    - **Set Items:**  
  
        |Name | Value|
        |------|------|
        |P3\_SCHOOL\_ID| &ID. |
        {: title="Set Item name and value"}

    Clique em **OK**.

    - **Appearance > Icon:** fa-info-circle-o u-opacity-60  
    - **CSS classes:** t-Button--noUI  

    Clique em **Save**.

---

## Tarefa 6: Criar Botão "Ask a Question"

1. Na página **Search and Apply**, na barra **Breadcrumb**, clique com o botão direito em **New York City** e selecione **Create Button**.

    ![Page Designer](images/create-button.png ' ')

2. Configure no **Property Editor**:
    - **Name:** ask  
    - **Label:** Ask a Question  
    - **Appearance > Hot:** ON  

    ![Page Designer](images/button-properties.png =50%x*)

3. Clique com o botão direito em **ask** e selecione **Create Dynamic Action**:
    - **Action:** Open AI Assistant  
    - **Service:** OCI Gen AI  
    - **System Prompt:**  
  
        ```plaintext
        ###ROLE: Expert on NYC high schools  
        ###GUARDRAILS:
        - Only answer questions about NYC high schools.
        - Respond with "This utility only answers questions about NYC high schools" for unrelated questions.
        ```
    - **Welcome Message:** What would you like to know about New York City High Schools?  

    Clique em **Save**.

---

## Resumo

Agora você sabe configurar o contexto do prompt e criar um chatbot interativo no APEX usando **Generative AI**. Siga para o próximo laboratório!