# Introdução

## Sobre este Workshop

Desde a síntese precisa de dados complexos até a criação de respostas contextuais, a IA Generativa está à frente de uma mudança tecnológica, prometendo redefinir como navegamos e utilizamos conhecimento em nossas interações diárias. Com a introdução do desenvolvimento assistido por IA no Oracle APEX, habilitar suas aplicações com recursos de IA generativa nunca foi tão fácil.

Neste workshop, você aprenderá a trazer recursos de IA generativa baseados em grandes modelos de linguagem (LLMs) para suas aplicações construídas com Oracle APEX, sua plataforma low-code favorita. O serviço de IA Generativa pode ser acessado por meio de APIs REST, e ao usar os poderosos recursos de Fonte de Dados REST do APEX, você pode incorporar essa tecnologia avançada em suas aplicações de maneira simples e de baixa codificação.

<!-- ## O que é o serviço de IA Generativa da OCI?

[Oracle Cloud Infrastructure Generative AI](https://www.oracle.com/artificial-intelligence/generative-ai/large-language-models/) é um serviço totalmente gerenciado que fornece um conjunto de modelos de linguagem de última geração e personalizáveis que cobrem uma ampla variedade de casos de uso para geração de texto. A IA Generativa atualmente suporta os seguintes modelos fundamentais pré-treinados disponíveis da Meta e Cohere:

- Llama 2
- Command
- Summarize
- Embed

Você pode ler mais sobre esses modelos na [documentação](https://docs.oracle.com/en-us/iaas/Content/generative-ai/pretrained-models.htm). -->

Este workshop irá guiá-lo pelo processo de utilizar os modelos de geração dentro do Serviço de IA Generativa para desenvolver um recurso de "Fazer Perguntas" para o aplicativo APEX das Escolas de Ensino Médio de Nova York. Esta funcionalidade permite que os pais façam perguntas sobre instalações, políticas e muito mais, ajudando-os a decidir se uma escola específica é a escolha ideal para seu filho.

Tempo Estimado: 60 minutos

### **Objetivos**

* Implementar Pesquisa Facetada para Cartões e Regiões de Mapa para filtrar e buscar a melhor escola.
* Construir um chatbot conversacional usando IA Generativa para fazer perguntas sobre uma escola.
* Gerar e-mail usando IA Generativa para aplicar para uma escola.

*Nota: Este workshop pressupõe o uso do Oracle APEX 24.1.2 ou superior.*

## Pré-requisitos

- Uma conta paga na Oracle Cloud Infrastructure (OCI) ou uma conta GRATUITA na Oracle Cloud com $300 em créditos por 30 dias para usar em outros serviços. Saiba mais em: [oracle.com/cloud/free/](https://www.oracle.com/cloud/free/). A conta OCI deve ser criada ou assinada em uma das regiões que suportam o Serviço de IA Generativa da OCI. Atualmente, o Serviço de IA Generativa da OCI é suportado nas seguintes regiões:

    - Centro-Oeste dos EUA (Chicago)
    - Centro da Alemanha (Frankfurt)
    - Sul do Reino Unido (Londres)
    - Leste do Brasil (São Paulo)

- Este workshop utiliza o Serviço de IA Generativa da OCI. O serviço de IA Generativa da OCI está disponível em regiões limitadas. Para verificar se sua região de cloud suporta o serviço de IA Generativa da OCI, visite a [documentação](https://docs.oracle.com/en-us/iaas/Content/generative-ai/overview.htm#regions).

- Um compartimento OCI. Uma conta Oracle Cloud vem com dois compartimentos pré-configurados - A tenência (compartimento raiz) e o ManagedCompartmentForPaaS (criado pela Oracle para serviços da Plataforma Oracle).

- O usuário logado deve ter privilégios necessários para criar e gerenciar instâncias de Banco de Dados Autônomo nesse compartimento. Você pode configurar esses privilégios por meio de uma Política de IAM OCI. Se você estiver usando uma conta Free Tier, é provável que já possua todos os privilégios necessários.

- Um workspace APEX 24.1.2. Recomendamos que você se inscreva para um workspace em [apex.oracle.com](https://apex.oracle.com). Consulte o lab [Get Started: Option 3](?lab=1-sign-up-apex#Option3:apexoraclecom) para se inscrever em um novo workspace.

*Nota: Este workshop pressupõe o uso do Oracle APEX 24.1.2. Alguns recursos podem não estar disponíveis em versões anteriores, e as instruções, fluxo e capturas de tela podem diferir se você usar uma versão mais antiga do Oracle APEX.*

## Laboratórios

| Módulo | Tempo Estimado |
| --- | --- |
| [Criando uma aplicação APEX](?lab=1-create-app) | 5 minutos |
| [Visualizar Escolas no Mapa](?lab=2-schools-on-map) | 10 minutos |
| [Configurar as Chaves da API OCI](?lab=3-configure-oci) | 10 minutos |
| [Construir uma Consulta Conversacional usando IA Generativa](?lab=4-using-genai) | 20 minutos |
| [Gerar E-mail para Aplicação em uma Escola](?lab=5-apply-to-school) | 15 minutos |
| [Executar a Aplicação](?lab=6-run-app) | 5 minutos |

Tempo total estimado: 60 minutos

### **Vamos Começar!**

Se o menu não estiver visível, você pode abri-lo clicando no botão de menu (![Ícone do Menu](./images/menu-button.png)) no canto superior esquerdo da página.

## Downloads

Se você tiver algum problema ou o aplicativo não estiver funcionando como esperado, você pode baixar e instalar o aplicativo concluído da seguinte maneira:

1. [Clique aqui](https://c4u04.objectstorage.us-ashburn-1.oci.customer-oci.com/p/EcTjWk2IuZPZeNnD_fYMcgUhdNDIDA6rt9gaFj_WZMiL7VvxPBNMY60837hu5hga/n/c4u04/b/livelabsfiles/o/labfiles/nyc-gen-ai-app.zip) para baixar o arquivo zip do aplicativo concluído.

2. Importe o arquivo **nyc-gen-ai-app.zip** para seu workspace. Para importar o app, vá para **App Builder > Import**.

3. Siga os passos no assistente de Instalar Aplicação para instalar o app junto com os Objetos Suporte.

4. Uma vez que a aplicação esteja instalada, navegue até **App Builder > Workspace Utilities > Web Credentials**.

5. Edite os detalhes de **apex\_ai\_cred** para que correspondam à sua Chave da API OCI. Consulte o lab: [Configurar as Chaves da API OCI](?lab=3-configure-oci) para criar uma Chave da API OCI.

     ![Página de Credenciais da Web](images/edit-web-cred.png " ")
6. Navegue até **App Builder > Workspace Utilities > Generative AI**. Edite o serviço **OCI Gen AI**.

    - Insira o **ID do Compartimento**. Consulte a [Documentação](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/contactingsupport_topic-Locating_Oracle_Cloud_Infrastructure_IDs.htm#:~:text=Finding%20the%20OCID%20of%20a,displayed%20next%20to%20each%20compartment.) para obter seu ID de Compartimento. Se você tiver apenas um compartimento, use o OCID do arquivo de configuração que você salvou ao criar sua Chave da API OCI.
    - Para Credencial, selecione **apex\_ai\_cred**.
    - Clique em **Aplicar Alterações**.
    ![Página de IA Generativa](images/edit-oci-genai.png " ")

7. Pronto. Agora, execute a aplicação e veja-a em ação! Siga o lab: [Executar a Aplicação](?lab=6-run-app) para rodar o app.

## Saiba Mais - *Links Úteis*

- [OCI Generative AI](https://www.oracle.com/artificial-intelligence/generative-ai/large-language-models/)
- [APEX on Autonomous](https://apex.oracle.com/autonomous)
- [APEX Collateral](https://www.oracle.com/database/technologies/appdev/apex/collateral.html)
- [Tutoriais](https://apex.oracle.com/en/learn/tutorials)
- [Comunidade](https://apex.oracle.com/community)
- [Site Externo + Slack](http://apex.world)

## Agradecimentos

 - **Autores** - Toufiq Mohammed, Senior Product Manager; Apoorva Srinivas, Senior Product Manager
 - **Última Atualização Por/Data** - Apoorva Srinivas, Senior Product Manager, julho de 2024