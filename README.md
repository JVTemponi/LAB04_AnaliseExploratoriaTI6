# LAB04_AnaliseExploratoriaTI6


# Projeto de Pesquisa

### **A Evolução da Cultura de Testes**

A Evolução da Cultura de Testes em Projetos Maduros: Um Estudo de Caso Múltiplo sobre a Evolução de Estratégias, Ferramentas e Sofisticação.

### 1 - GQMs

**RQ1: Como a estratégia de testes, refletida na proporção dos tipos de teste, evolui entre as diferentes releases de um projeto?**

- **Métrica 1.1: Proporção de Tipos de Teste (por arquivos/LOC):** No momento de cada release (tag), calcular a porcentagem de código dedicada a testes unitários, de integração e E2E. Isso mostrará como a "pirâmide de testes" do projeto muda ao longo do tempo.

**RQ2: Como o ecossistema de ferramentas de teste evolui ao longo das releases, marcado pela adoção de novas bibliotecas ou migrações significativas? 

 Major x Mirror Realese**

- **Métrica 2.1: Censo de Ferramentas por Release:** No momento de cada release, analisar o arquivo de build (`pom.xml`, `package.json`, etc.) para registrar o conjunto de ferramentas de teste e suas versões.
- **Métrica 2.2: Eventos de Adoção/Migração:** Identificar em qual ciclo de release uma nova ferramenta de teste foi introduzida ou uma migração de versão major ocorreu.
    - Existe uma correlação entre o tipo de release e o impacto nos testes? Acho que as grandes evoluções na nossa forma de testar (cultura, ferramentas) coincidem com as versões major, enquanto os patches exigem apenas testes focados na correção.
    
    keycloak
    
    Elaborar RQ para isso
    1 ) Separar em pipelines
    
    1. Conceito de shift-left
    2. Numero de contribuidores
    

**RQ3: A sofisticação e a qualidade dos testes, refletidas pela cobertura e pela qualidade interna do código, aumentam com a maturidade do projeto?**

- **Métrica 3.1: Evolução da Cobertura de Testes:** Medir a porcentagem de cobertura de testes no momento de cada release principal.
- **Métrica 3.2: Densidade de `Test Smells` por Release:** No momento de cada release, calcular o número de `test smells` por mil linhas de código de teste (KLOC).  - TestSmellDector

**RQ4: Qual a relação entre o esforço de teste e o esforço de manutenção dentro de um ciclo de release?**

- **Métrica 4.1: Contagem de Commits de Teste por Ciclo:** No período entre duas releases, contar o número de commits que introduzem ou modificam significativamente o código de teste.
- **Métrica 4.2: Contagem de Commits de Correção de Bugs por Ciclo:** No mesmo período, contar o número de commits cujas mensagens indicam uma correção de bug (ex: contêm "fix", "bug", "patch", etc.).
- **Métrica 4.3: Análise de Correlação:** Ao longo de todos os ciclos de release de um projeto, realizar uma análise de correlação estatística entre a Métrica 4.1 e a Métrica 4.2 para verificar se um maior esforço em testes em um ciclo se relaciona com um menor esforço de correção no ciclo seguinte, ou outras relações.

- Cresceu conta

## 2. Metodologia

### **2.1 Filtros de Seleção (Critérios de Inclusão)**

- **Relevância/Popularidade:** Mínimo de 500 estrelas.
- **Longevidade:** Mínimo de 5 anos de histórico de commits. - REVISAR
- **Atividade Recente:** Mínimo de **20 commits no ramo principal no último ano**.
- **Intenção do Projeto:**
    - Exclusão por palavras-chave (ex: `course`, `tutorial`, `example`).
    - Mínimo de **5 contribuidores únicos**.
    - Criar o padrão
- **Originalidade:** Não ser um fork.
- **Histórico de Releases:** Mínimo de 10 releases marcadas com tags do Git.
- **Ecossistema Tecnológico:** Linguagem principal ser Java, Python ou JavaScript.
- **Validação Robusta de Testes:** O projeto deve atender **a todos** os seguintes sub-critérios:
    - **a)** Possuir um diretório de testes padrão **E** uma dependência de framework de teste.
    - **b)** O diretório de testes deve conter no mínimo **5 arquivos de teste**.
    - **c)** O sistema de build deve ter um **comando de teste configurado**.

### **2.3 Passo a Passo**

- **Mapear Releases:**
    - Chamar a API de **Tags** (`/repos/{owner}/{repo}/tags`) para obter uma lista cronológica de todas as `tags` de release.
- **Iterar sobre as Releases:**
    - Para cada `tag` da lista:
    a. Chamar a API de **Árvores Git** (`/git/trees/{hash}?recursive=1`) para obter a lista completa de arquivos daquela release.
    b. Filtrar essa lista para manter apenas os **arquivos de teste** (baseado no caminho e nome).
- **Classificar Arquivos:**
    - Para cada **arquivo de teste** da lista filtrada:
    a. Chamar a API de **Conteúdo** (`/contents/{path}?ref={tag}`) para buscar seu código-fonte.
    b. Analisar os `imports` do código para classificar o arquivo como **Unitário, Integração ou E2E**.

→ Usar dataset de repositórios como `awesome-testing`, `awesome-java-testing`, `awesome-python-testing` 

- **Calcular e Armazenar a Proporção:**
    - Calcular os totais de cada categoria.
    - Calcular a proporção (%) de cada tipo de teste.
    - Armazenar o resultado associado à `tag` da release.

**RQ2:** *Como o ecossistema de ferramentas de teste evolui ao longo das releases?*

1. **Mapear Ciclos de Release:**
    - Chame a API de **Tags** (`/repos/{owner}/{repo}/tags`) para obter a lista cronológica de releases.
    - Crie uma lista de pares de releases consecutivas (ex: `(v1.0, v1.1)`, `(v1.1, v1.2)`).
2. **Iterar sobre os Ciclos:**
    - Para cada par `(release_anterior, release_atual)`:
    a. Chame a API de **Comparação** (`/compare/{release_anterior}...{release_atual}`) para obter a lista de arquivos modificados no ciclo.
    b. Filtre essa lista para manter apenas os arquivos de build (ex: `pom.xml`, `package.json`).
3. **Analisar Mudanças nas Ferramentas:**
    - Para cada arquivo de build modificado:
    a. Use a API de **Conteúdo** para buscar a versão do arquivo na `release_anterior` e na `release_atual`.
    b. Faça o "parse" de ambas as versões, extraia as dependências de teste e compare as listas para identificar **ferramentas adicionadas, removidas ou atualizadas**.
4. **Registrar Eventos:**
    - Armazene os eventos de mudança encontrados, associando-os ao ciclo de release correspondente (ex: "Junit migrou de 4.x para 5.x no ciclo v2.3 -> v2.4").

---

**RQ3:** *A sofisticação e a qualidade dos testes aumentam com a maturidade do projeto?*

1. **Identificar Serviço de Cobertura:**
    - Analise o `README.md` e os arquivos de CI/CD (ex: `.github/workflows`) do projeto para encontrar menções a serviços como **Codecov** ou **Coveralls**.
2. **Coletar Dados via API do Serviço:**
    - Para os projetos que usam um desses serviços, utilize a **API específica do serviço** (não a do GitHub) para buscar o histórico de relatórios de cobertura.
3. **Mapear com Releases:**
    - Associe os dados de cobertura obtidos com as `tags` de release do Git, com base na data ou no hash do commit.
    - **Nota:** A coleta desta métrica é um desafio e pode não ser viável para todos os projetos, uma dificuldade já documentada na literatura.

**RQ4:** *Qual a relação entre o esforço de teste e de manutenção dentro de um ciclo de release?*

ou mais diretamente a ideia de mais testes hoje = menos bugs amanhã

Qual a relação entre o esforço dedicado a atividades de teste e o esforço subsequente de manutenção corretiva entre os ciclos de release?

1. **Mapear Ciclos de Release:**
    - Criar a lista de pares de releases consecutivas (ex: `(v1.0, v1.1)`), como na RQ2.
2. **Analisar Commits do Ciclo:**
    - Para cada par `(release_anterior, release_atual)`:
    a. Chamar a API de **Comparação** (`/repos/{owner}/{repo}/compare/{release_anterior}...{release_atual}`). 
    
    A resposta JSON incluirá uma lista de todos os `commits` feitos nesse ciclo.
    
    b. Inicialização de dois contadores: `commits_de_teste = 0` e `commits_de_correcao = 0`.
3. **Classificar Cada Commit:**
    - Para cada `commit` na lista obtida:
    a. Analisar a `commit.message`. obtendo os detalhes do commit individualmente para ver os arquivos modificados:
    `(/repos/{owner}/{repo}/commits/{commit_sha})`
    b. **Se** a mensagem contiver palavras-chave como `test`, `testing`, `refactor test`, incrementar `commits_de_teste`.
    c. **Senão, se** a mensagem contiver `fix`, `bug`, `patch`, `hotfix`, incrementar `commits_de_correcao`.
4. **Armazenar Contagens e Correlacionar:**
    - Salvar os totais de `commits_de_teste` e `commits_de_correcao` para cada ciclo.
    - Após coletar os dados de todos os ciclos, usar:
        - **Pandas:** Para carregar, manipular e agregar os dados do seu CSV.
        - **Matplotlib / Seaborn:** Para visualização de dados (essencial para entender as relações).
        - **SciPy / Statsmodels:** Para realizar os testes estatísticos de correlação e regressão.
