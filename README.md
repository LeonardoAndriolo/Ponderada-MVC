# Ponderada - Design de Arquitetura MVC
 
## Link de download [Draw.io](https://drive.google.com/file/d/1u1ascvHoU5ayWn1yCdNxPvwJKT5ECCgw/view?usp=sharing)

## Arquitetura MVC
- **Nome do Projeto:** SPARK
- **Descrição:** SPARK (Strategic Programming and Application Resource Knowledge) é uma plataforma web para o treinamento e atualização de funcionários montadores de computador através de manuais. A plataforma será acessada também por engenheiros administradores que farão a gestão da mesma.
- **Arquitetura:** MVC (Model-View-Controller)
- **Ferramenta de Diagramação:** draw.io

## Modelos (Models):
### Entidades
#### Manual
##### Atributos:
- **id**: Identificador único do manual (chave primária).
- **titulo**: Título do manual.
- **descricao**: Descrição breve do conteúdo do manual.
- **conteudo**: Conteúdo detalhado do manual, como instruções passo a passo.

#### Usuário
##### Atributos:
- **id**: Identificador único do usuário (chave primária).
- **nome**: Nome do usuário.
- **email**: Endereço de email do usuário.
- **senha**: Senha do usuário (geralmente armazenada como um hash).
- **funcao**: Papel do usuário no sistema (montador ou administrador).

### Relações

#### Usuário <-> Log de Atividade:
- Cada usuário pode ter zero ou muitos logs de atividade associados a ele.
- Relação de um para muitos entre Usuário e Log de Atividade.
- Chave estrangeira `usuario_id` na tabela de Log de Atividade referencia a tabela de Usuário.

#### Usuário <-> Manual:
- Cada usuário pode ter zero ou muitos manuais associados a ele (seja para visualização ou progresso de treinamento, embora este último tenha sido desconsiderado).
- Cada manual pode ser associado a zero ou muitos usuários (montadores).
- Relação de muitos para muitos entre Usuário e Manual.
- Isso pode ser implementado através de uma tabela intermediária (tabela de junção) que armazena as associações entre usuários e manuais.


## Controladores (Controllers):

### Usuário:

#### Responsabilidades:
- Autenticação de usuários
- Gerenciamento de contas de usuário
- Autorização de acesso

#### Ações:
##### Autenticação(username, password)
- **Parâmetros de entrada:** username (string), password (string)
- **Parâmetros de saída:** user (objeto User) ou null
- Interage com o modelo User para autenticar o usuário com as credenciais fornecidas.
- Retorna o objeto User autenticado ou null se as credenciais forem inválidas.

##### Gerenciamento de Funcionário(userId, userData)
- **Parâmetros de entrada:** userId (id do usuário a ser atualizado), userData (objeto com novas informações do usuário)
- **Parâmetros de saída:** user (objeto User) atualizado ou mensagem de erro, se aplicável
- Interage com o modelo User para atualizar as informações do usuário com base no id fornecido.
- Retorna o objeto User atualizado ou uma mensagem de erro se a atualização falhar.

#### Interação com modelos e views:
- Os métodos interagem com o modelo User para realizar operações de CRUD (criar, ler, atualizar, excluir) nos dados dos usuários.
- Após realizar as operações no modelo, os controladores retornam os resultados para as views correspondentes, que então são renderizadas para os usuários.

### Manual:

#### Responsabilidades:
- Visualização de manuais
- Busca de manuais
- Atualização de manuais

#### Ações:
##### Visualização de manuais (manualId)
- **Parâmetros de entrada:** manualId (id do manual a ser visualizado)
- **Parâmetros de saída:** manual (objeto Manual) ou mensagem de erro, se aplicável
- Interage com o modelo Manual para recuperar as informações do manual com o id fornecido.
- Retorna o objeto Manual correspondente ou uma mensagem de erro se a visualização falhar.

##### Busca de manuais (keyword)
- **Parâmetros de entrada:** keyword (palavra-chave para busca)
- **Parâmetros de saída:** results (array de objetos Manual) ou mensagem de erro, se aplicável
- Interage com o modelo Manual para buscar manuais com base na palavra-chave fornecida.
- Retorna uma lista de resultados correspondentes ou uma mensagem de erro se a busca falhar.

##### Atualização de manuais (manualId, newData)
- **Parâmetros de entrada:** manualId (id do manual a ser atualizado), newData (novas informações do manual)
- **Parâmetros de saída:** manual (objeto Manual) atualizado ou mensagem de erro, se aplicável
- Interage com o modelo Manual para atualizar as informações do manual com base no id fornecido.
- Retorna o objeto Manual atualizado ou uma mensagem de erro se a atualização falhar.

### Interação com modelos e views:
- Os métodos interagem com o modelo Manual para recuperar informações sobre os manuais, buscar manuais e atualizar informações de manuais.
- Os resultados são então passados para as views correspondentes para renderização e exibição para os usuários.

### Admin Page:

#### Responsabilidades:
- Gerenciamento de usuários
- Gerenciamento de manuais
- Monitoramento de atividades

#### Ações:
##### Gerenciamento de usuários()
- **Parâmetros de entrada:** nenhum
- **Parâmetros de saída:** users (array de objetos User) ou mensagem de erro, se aplicável
- Interage com o modelo User para listar todos os usuários do sistema.
- Retorna uma lista de todos os usuários ou uma mensagem de erro se a operação falhar.

##### Gerenciamento de manuais()
- **Parâmetros de entrada:** nenhum
- **Parâmetros de saída:** manuals (array de objetos Manual) ou mensagem de erro, se aplicável
- Interage com o modelo Manual para listar todos os manuais disponíveis no sistema.
- Retorna uma lista de todos os manuais ou uma mensagem de erro se a operação falhar.

##### Monitoramento de atividades()
- **Parâmetros de entrada:** nenhum
- **Parâmetros de saída:** activities (array de logs de atividade) ou mensagem de erro, se aplicável
- Interage com o modelo de Logs de Atividade para recuperar informações sobre as atividades realizadas no sistema.
- Retorna uma lista de logs de atividade ou uma mensagem de erro se a operação falhar.

#### Interação com modelos e views:
- Os métodos interagem com os modelos correspondentes para recuperar informações sobre usuários, manuais e atividades, bem como para gerar relatórios.
- Os resultados são passados para as views correspondentes para renderização e exibição para os usuários.


## Views (Views):
- **Login:** Permitir que os usuários façam login no sistema.

- **Page Usuário:** Fornecer uma visão geral do sistema após o login, exibindo os manuais disponíveis e as funcionalidades disponíveis.

- **Page Admin:** Interface para o administrador gerenciar usuários, adicionar novos manuais, visualizar relatórios, etc.

- **Manual:** Mostrar detalhes específicos de um manual, como instruções de montagem, atualização, etc.

## Infraestrutura:
### Banco de Dados:

- **Escolha:** PostgreSQL. Um banco de dados relacional seria adequado para armazenar informações sobre usuários, manuais e logs de atividade.

- **Integração com MVC:** Os modelos de dados (Usuário e Manual) se comunicam com o banco de dados por meio do modelo MVC. Os controllers interagem com esses modelos para realizar operações de leitura, escrita, atualização e exclusão de dados.

- **Impacto:** Um banco de dados relacional oferece consistência e integridade dos dados, garantindo que as informações sejam armazenadas de forma estruturada e segura. Isso facilita a manutenção e o acesso aos dados conforme necessário pelas diferentes partes da aplicação.

### Servidor Web:

- **Escolha:** Node.js com Express.js.

- **Integração com MVC:** O Express.js permite organizar o código em rotas (Controllers), que lidam com as requisições, interagem com os modelos de dados (Models) para acessar o banco de dados e retornam as respostas adequadas.

- **Impacto:**  Utilizar Node.js com Express.js proporciona escalabilidade e desempenho, lidando eficientemente com um grande volume de requisições. A modularidade e flexibilidade do Express.js facilitam o desenvolvimento e manutenção. A integração com o padrão MVC promove uma organização clara do código e separação de responsabilidades.

### Autenticação e Autorização:

- **Escolha:** SSO. O uso de um serviço de autenticação pode ser implementado para lidar com a autenticação e autorização dos usuários.

- **Integração com MVC:** Os controllers de usuário podem integrar-se com o serviço de autenticação para verificar as credenciais do usuário durante o login e controlar o acesso às funcionalidades da aplicação com base nas permissões do usuário.

- **Impacto:** Implementar uma solução de autenticação e autorização robusta é fundamental para proteger os dados e funcionalidades sensíveis da aplicação. Isso garante que apenas usuários autorizados tenham acesso às informações e operações permitidas dentro do sistema.


## Implicações da Arquitetura:
### Escalabilidade:

- **Impacto:** A arquitetura MVC permite escalabilidade horizontal e vertical, facilitando o aumento da carga em partes específicas da aplicação.

- **Considerações:** É importante projetar os componentes da aplicação para escalabilidade desde o início, utilizando técnicas como cache e balanceamento de carga.

### Manutenção:

- **Impacto:** A separação de responsabilidades entre modelos, views e controllers simplifica a manutenção do código, reduzindo o impacto de alterações em uma parte da aplicação sobre outras.

- **Considerações:** Adotar boas práticas de desenvolvimento, testar o código e implementar processos de controle de versão são essenciais para manter a estabilidade do sistema ao longo do tempo.

### Testabilidade:

- **Impacto:** A arquitetura MVC facilita a testabilidade da aplicação devido à separação clara de responsabilidades entre os componentes.

- **Considerações:** Desenvolver testes unitários e de integração é fundamental para garantir o funcionamento correto de cada componente da aplicação.

### Segurança:
- **Impacto:** A arquitetura MVC não aborda automaticamente questões de segurança, mas fornece uma estrutura para implementar medidas de segurança em várias camadas da aplicação.
- **Considerações:** É fundamental aplicar validação de entrada, autenticação, autorização e prevenção de vulnerabilidades conhecidas, como XSS e injeção de SQL, em todas as partes da aplicação.

### Desempenho:
- **Impacto:** A arquitetura MVC pode influenciar o desempenho da aplicação com base na eficiência dos controllers, views e acesso ao banco de dados.
- **Considerações:** Para otimizar o desempenho, é necessário projetar controllers e views leves, utilizar consultas eficientes ao banco de dados e implementar caching quando apropriado. Testes regulares de desempenho são fundamentais para identificar e corrigir possíveis problemas de desempenho.



Recursos Adicionais:
Documentação do Sails.js: https://github.com/balderdashy/sails Tutorial do draw.io: https://m.youtube.com/watch?v=w3zm-wbmlpc Exemplos de diagramas MVC: https://www.lucidchart.com/pages/templates
