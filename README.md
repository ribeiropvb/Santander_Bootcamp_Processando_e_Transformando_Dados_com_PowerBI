# Processando e Transformando Dados com PowerBI

Importamos dados do MySQL na Azure para o Power BI. Nosso foco foi transformar esses dados, resultando em um conjunto pronto para análise. Estabelecemos uma conexão firme entre o MySQL e o Power BI, e a seguir, limpamos e unimos tabelas, aplicando também filtros e cálculos. Criamos relatórios no Power BI, usando visualizações avançadas para destacar insights. Resumindo, superamos desafios para entregar dados confiáveis e facilitar decisões informadas.

# 1. Configuração na Azure:
Criamos uma instância MySQL na Azure, garantindo um ambiente seguro e adaptável.

# 2. Montagem do Banco de Dados:
Usando um arquivo do GitHub, estruturamos o banco com tabelas e preenchemos com dados iniciais.

# 3. Conexão Power BI - MySQL:
Estabelecemos uma ligação direta entre o Power BI Desktop e o MySQL na Azure, facilitando a importação de dados.

# 4. Análise e Correção da Base:
Após importar, revisamos os dados:

    - Cabeçalhos e Tipos de Dados: Ajustamos erros e definimos os tipos corretos.
    - Valores Monetários: Convertidos para "decimal fixo" para precisão.
    - Tratamento de Nulos: Identificamos um valor nulo, decidindo mantê-lo, dada a sua relevância.
    - Funcionários Sem Gerente: Usando Power Query no Power BI, adicionamos a coluna "FuncionarioSemGerente". Através da função "if", identificamos se "Super_ssn" era nulo, classificando como "Sim" ou "Não".

# 5. Análise de Gerência dos Departamentos:
Examinamos a tabela "azure_company departament", especificamente a coluna "azure_company.employee(Mgr_ssn)", para verificar departamentos sem gerentes. Resultado: Todos os departamentos têm gerentes atribuídos.

# 6. Confirmação de Departamentos com Gerência:
Com a análise anterior, confirmamos que não existem departamentos desprovidos de gerência. Assim, não foram necessárias ações corretivas.

# 7. Avaliação das Horas dos Projetos:
Na tabela "azure_company works_on", analisamos as horas alocadas a cada projeto:
  - Acessamos a tabela e somamos as horas por projeto.
  - Implementamos uma medida calculada no Power BI para exibir os totais.

O resultado forneceu insights sobre o esforço dedicado a cada projeto.

# 8. Desmembramento de Colunas Complexas:
Na "azure_company employee", colunas como "azure_company.departament(Ssn)" possuíam estruturas tipo "table". Contudo, devido à inconsistência dos dados, não conseguimos expandi-las. A situação foi semelhante na "azure_company project". No entanto, outras tabelas permitiram a separação das colunas, otimizando a análise. Esse processo destaca o quão crucial é avaliar a consistência dos dados antes de tentar desmembrá-los.

# 9. Fusão das Consultas Employee e Departament:
Mesclamos as consultas "employee" e "departament" no Power Query Editor do Power BI, formando uma tabela "Employee Department". A fusão ocorreu entre as colunas "Ssn" de "employee" e "Mgr_ssn" de "departament". A tabela resultante, além dos dados de "employee", possui o campo "DepartmentName", indicando o departamento do colaborador. Esta ação aprimorou a riqueza de nossa análise.

# 10. Otimização da Estrutura de Dados:
Para aprimorar a análise no Power BI, simplificamos a estrutura de dados após a fusão das tabelas "employee" e "departament". A seguir, os pontos principais desta fase:

  - Descarte de Colunas Irrelevantes: Eliminamos colunas que não contribuíam diretamente para nossa análise, tornando o conjunto de dados mais focado e manejável.

Esse refinamento assegura que as futuras visualizações e análises sejam claras e alinhadas aos nossos objetivos. Manter a estrutura de dados enxuta é vital para análises eficazes.

# 11. Associação entre Colaboradores e Gerentes:

Iniciamos a tarefa utilizando uma consulta SQL na Azure que associava cada colaborador ao seu gerente. A consulta retornou uma tabela que continha as colunas "EmployeeName" e "ManagerName".

Com essa tabela em mãos, no PowerQuery, adicionamos uma nova coluna. Utilizamos a seguinte lógica: se o nome do colaborador fosse "John B Smith", "Franklin T Wong" ou "Ahmad V Jabbar", ele seria categorizado como "Gerente". Caso contrário, seria categorizado como "Colaborador".

# 12. Unificação das Colunas de Nome:

Para tornar a análise mais simples e direta, combinamos as colunas Fname, Minit e Lname em uma única coluna chamada "Name". Dessa forma, cada colaborador agora é identificado por um nome completo em uma única coluna, facilitando a visualização e a análise dos dados.

# 13. Criação da Tabela "Department By Local":

Nesta fase, mesclamos as tabelas "azure_company departament" e "azure_company dept_locations" para consolidar informações sobre departamentos e seus respectivos locais em uma única tabela denominada "Department By Local".

Detalhamento da Mesclagem:

Mesclagem de Tabelas: Usando uma ferramenta específica, unimos as tabelas "azure_company departament" e "azure_company dept_locations" com base em campos comuns, como um identificador de departamento. Isso gerou a nova tabela "Department By Local".

Objetivo e Benefícios: Com "Department By Local", agora possuímos combinações distintas de departamentos e seus locais. Isso serve como base para o desenvolvimento de um modelo estrela no futuro, onde essa tabela central será crucial para conectar outras tabelas e proporcionar análises mais ricas.

A consolidação dessas informações em uma tabela otimiza a estrutura dos dados, facilitando análises futuras e a criação de visualizações no Power BI ou em outras ferramentas de BI.

# 14. Por que Optamos pela Mesclagem ao Invés da Atribuição?

**Entendendo o Contexto:**
No universo dos bancos de dados relacionais, a mesclagem, também chamada de "join", é uma técnica primordial. Ela é aplicada quando desejamos combinar tabelas diferentes com base em campos que têm valores em comum, no caso do nosso desafio, o campo "Mgr_ssn".

**Relacionando as Tabelas:**
Dispomos de duas tabelas distintas. Uma destaca detalhes dos gerentes e a outra, dos colaboradores. Dada a estrutura, cada colaborador é vinculado a um gerente por meio do "Mgr_ssn". Para construir uma visão integrada que congregue informações dos gerentes, colaboradores, e dos detalhes dos departamentos e lojas, é indispensável estabelecer uma relação entre essas tabelas.

# 15. Por que Optamos pela Mesclagem ao Invés da Atribuição?

**Entendendo o Contexto:**
No universo dos bancos de dados relacionais, a mesclagem, também chamada de "join", é uma técnica primordial. Ela é aplicada quando desejamos combinar tabelas diferentes com base em campos que têm valores em comum, no caso do nosso desafio, o campo "Mgr_ssn".

**Relacionando as Tabelas:**
Dispomos de duas tabelas distintas. Uma destaca detalhes dos gerentes e a outra, dos colaboradores. Dada a estrutura, cada colaborador é vinculado a um gerente por meio do "Mgr_ssn". Para construir uma visão integrada que congregue informações dos gerentes, colaboradores, e dos detalhes dos departamentos e lojas, é indispensável estabelecer uma relação entre essas tabelas.

**Por que Mesclagem e não Atribuição?**
A ação de "atribuir" está relacionada principalmente a adicionar ou modificar valores dentro de uma tabela já existente conforme determinada lógica. Ela não se concentra na combinação de dados de tabelas diferentes. No nosso caso, a intenção não era apenas ajustar valores, mas sim consolidar informações de múltiplas tabelas.

**O Impacto da Mesclagem:**
Com a mesclagem centrada na chave "Mgr_ssn", conseguimos uma tabela integrada que correlaciona cada colaborador ao seu gerente, e também elucida informações referentes ao departamento e loja aos quais estão associados. Este processo amplia o valor informativo do nosso conjunto de dados, preparando-o para análises mais profundas.

**Em Resumo:**
A decisão entre usar mesclagem ou atribuição se baseia no propósito e na configuração dos dados em mãos. Neste cenário, para alcançar uma visão abrangente que conecta gerentes e colaboradores, bem como outros detalhes associativos, a mesclagem se mostrou a escolha acertada.
    
