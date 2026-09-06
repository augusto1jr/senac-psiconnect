# PsiConnect

Plataforma web para conectar pacientes a psicólogos de acordo com suas necessidades, especialidades e abordagens de atendimento. O sistema permite a busca de profissionais, aplicação de filtros, visualização de perfil e agendamento de consultas, com foco em acessibilidade e atendimento presencial ou remoto.

> **Repositório:** https://github.com/augusto1jr/senac-psiconnect

---

## Integrantes do projeto

| Integrante | GitHub |
|---|---|
| Melissa Ferreira Santos | [@smelissasantos10-create](https://github.com/smelissasantos10-create) |
| Rayane Lima Pimentel | [@rayanepimentel](https://github.com/rayanepimentel) |
| Eduardo Dorea de Oliveira Macario Muniz | [@munizeduardo](https://github.com/munizeduardo) |
| Natalia Arruda | [@NataliaArruda87](https://github.com/NataliaArruda87) |
| Natani Gabriele Sales Moraes | [@NataniMoraes](https://github.com/NataniMoraes) |
| Leticia Honma Franco | [@LeticiaHF](https://github.com/LeticiaHF) |
| Augusto Rosário Gomes Júnior | [@augusto1jr](https://github.com/augusto1jr) |

---

## Sobre o projeto

O PsiConnect foi desenvolvido como uma plataforma de atendimento psicológico com o objetivo de aproximar pacientes e psicólogos por meio de uma busca orientada por necessidades, especialidades e abordagens profissionais.

A aplicação contempla:

- Cadastro e autenticação de pacientes e psicólogos;
- Busca e filtragem de psicólogos;
- Exibição de perfil profissional;
- Informações sobre especialidades e abordagens;
- Recomendação de psicólogos;
- Agendamento de consultas;
- Organização de consultas para o psicólogo;
- Avaliações de profissionais;
- Suporte a atendimento presencial e remoto;
- Regra de preço diferenciada para beneficiários de assistência social.

A implementação disponibilizada no projeto prioriza a jornada principal do paciente: **buscar um psicólogo → aplicar filtros → visualizar o perfil → agendar uma consulta**.

---

## Arquitetura

O projeto utiliza uma arquitetura em três camadas:

```text
┌──────────────────────────────────────────────┐
│                  FRONT-END                   │
│          Next.js + React + JavaScript       │
│              http://localhost:3000          │
└───────────────────────┬──────────────────────┘
                        │ HTTP / REST
                        ▼
┌──────────────────────────────────────────────┐
│                  BACK-END                    │
│        Java + Spring Boot + Spring JPA      │
│             Spring Security + BCrypt        │
│              http://localhost:8080          │
└───────────────────────┬──────────────────────┘
                        │ JDBC
                        ▼
┌──────────────────────────────────────────────┐
│                  BANCO DE DADOS              │
│                 PostgreSQL 17                │
│             localhost:5432/psiconnect       │
└──────────────────────────────────────────────┘
```

### Tecnologias utilizadas

**Front-end**
- Next.js 15.3.2
- React 19
- JavaScript
- CSS
- npm

**Back-end**
- Java 23
- Spring Boot 3.4.3
- Spring Web
- Spring Data JPA / Hibernate
- Spring Security
- BCrypt
- Lombok
- Maven

**Banco de dados**
- PostgreSQL
- Scripts SQL para criação e carga inicial

As versões do back-end podem ser conferidas no `back-end/pom.xml`, enquanto as principais dependências do front-end estão em `front-end/package.json`.

---

## Estrutura do projeto

```text
psiconnect/
│
├── back-end/
│   ├── src/
│   ├── .mvn/
│   ├── pom.xml
│   ├── mvnw
│   └── mvnw.cmd
│
├── front-end/
│   ├── app/
│   ├── docs/
│   ├── public/
│   ├── screens/
│   ├── package.json
│   ├── package-lock.json
│   └── next.config.mjs
│
├── database/
│   ├── mysql/
│   └── postgresql/
│       ├── schema.sql
│       └── populate.sql
│
└── README.md
```

---

# Requisitos

Antes de executar o projeto, instale:

- Git
- PostgreSQL 17
- pgAdmin 4 (opcional, mas recomendado para administrar o banco)
- Java 23
- Node.js 20 ou compatível com as dependências do projeto
- npm
- Maven (opcional quando utilizado o Maven Wrapper)

O projeto **não depende do WAMP** para sua execução atual. O banco utilizado pelo back-end é o PostgreSQL.

---

# Configuração do banco de dados

## 1. Iniciar o PostgreSQL

Inicie o serviço do PostgreSQL e confirme que o servidor está disponível na porta padrão:

```text
localhost:5432
```

Abra o pgAdmin e conecte-se ao servidor PostgreSQL.

## 2. Criar o banco

Crie um banco chamado:

```text
psiconnect
```

Configuração utilizada pelo back-end:

```text
Host: localhost
Porta: 5432
Banco: psiconnect
Usuário: postgres
```

A senha deve corresponder à configuração local do arquivo `application.properties`.

## 3. Criar as tabelas

Na versão atual do projeto, os scripts PostgreSQL estão em:

```text
database/postgresql/schema.sql
database/postgresql/populate.sql
```

No pgAdmin, abra o Query Tool conectado ao banco `psiconnect`.

Execute primeiro:

```text
database/postgresql/schema.sql
```

e, depois, execute:

```text
database/postgresql/populate.sql
```

A ordem é importante: primeiro a estrutura do banco, depois os dados.

> **Observação:** o back-end está configurado com `spring.jpa.hibernate.ddl-auto=update`. Isso permite que o Hibernate atualize a estrutura das tabelas a partir das entidades. Ainda assim, os scripts SQL versionados no repositório são a referência para preparar o banco inicial do projeto.

---

# Configuração do back-end

O arquivo de configuração utilizado é:

```text
back-end/src/main/resources/application.properties
```

A configuração atual esperada é semelhante a:

```properties
spring.application.name=psiconnect

spring.datasource.url=jdbc:postgresql://localhost:5432/psiconnect
spring.datasource.username=postgres
spring.datasource.password=root

spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.show-sql=true

server.port=8080
```

**Importante:** não publique senhas reais ou outros segredos no GitHub. Caso a senha local seja diferente de `root`, altere apenas o arquivo de configuração da sua máquina.

---

# Executando o back-end

Abra um terminal na raiz do projeto:

```powershell
cd back-end
```

## Compilar

No Windows:

```powershell
.\mvnw.cmd clean package
```

Em caso de sucesso, será exibido:

```text
BUILD SUCCESS
```

## Iniciar o servidor

```powershell
.\mvnw.cmd spring-boot:run
```

O servidor ficará disponível em:

```text
http://localhost:8080
```

Mantenha esse terminal aberto enquanto o front-end estiver sendo executado.

---

# Executando o front-end

Abra **outro terminal**.

Entre na pasta:

```powershell
cd front-end
```

Instale as dependências:

```powershell
npm install
```

Depois, inicie o servidor de desenvolvimento:

```powershell
npm run dev
```

A aplicação estará disponível em:

```text
http://localhost:3000
```

---

# Ordem completa de inicialização

Para executar o projeto localmente:

```text
1. Iniciar PostgreSQL
        ↓
2. Abrir pgAdmin e conferir o banco psiconnect
        ↓
3. Criar/carregar schema.sql e populate.sql
        ↓
4. Iniciar o Spring Boot
        ↓
5. Iniciar o Next.js
        ↓
6. Abrir http://localhost:3000
```

Os servidores utilizados são:

```text
PostgreSQL     → localhost:5432
Spring Boot    → localhost:8080
Next.js        → localhost:3000
```

---

# Funcionalidades principais

## Paciente

- Cadastro;
- Login;
- Consulta de psicólogos;
- Busca por filtros;
- Visualização do perfil do psicólogo;
- Visualização de avaliações;
- Agendamento de consulta;
- Consulta de informações relacionadas ao atendimento.

## Psicólogo

- Cadastro;
- Login;
- Visualização do perfil;
- Organização de consultas;
- Visualização de agenda;
- Visualização de avaliações recebidas.

---

# Segurança

As senhas de usuários não são armazenadas em texto puro. O projeto utiliza **Spring Security e BCrypt** para realizar a codificação das senhas.

Não versionar no Git:

```text
.env.local
.env.*
senhas reais
tokens
credenciais de banco de produção
```

Também não é necessário versionar dependências ou arquivos gerados:

```text
node_modules/
.next/
target/
```

---

# Desenvolvimento

## Front-end

Para desenvolvimento local:

```powershell
cd front-end
npm install
npm run dev
```

Outros scripts disponíveis no projeto:

```powershell
npm run build
npm run start
npm run lint
npm run doc
```

## Back-end

Para desenvolvimento local:

```powershell
cd back-end
.\mvnw.cmd spring-boot:run
```

Para gerar o pacote:

```powershell
.\mvnw.cmd clean package
```

---

# Banco de dados

O diretório PostgreSQL do projeto contém:

```text
database/postgresql/
├── schema.sql
└── populate.sql
```

`schema.sql` contém a definição da estrutura do banco.

`populate.sql` contém a carga inicial de dados para testes e demonstração.

O repositório também possui scripts relacionados a MySQL em:

```text
database/mysql/
```

Para a execução atual do sistema, utilize a versão PostgreSQL.

---

# Solução de problemas

## Maven informa que não encontrou o POM

Certifique-se de executar o Maven dentro da pasta:

```text
back-end/
```

e não diretamente na raiz do repositório.

Correto:

```powershell
cd back-end
.\mvnw.cmd spring-boot:run
```

## O back-end não conecta ao PostgreSQL

Verifique:

1. Se o serviço PostgreSQL está em execução;
2. Se o banco `psiconnect` existe;
3. Se a porta utilizada é `5432`;
4. Se usuário e senha estão corretos no `application.properties`;
5. Se o `schema.sql` e o `populate.sql` foram executados quando necessário.

## O front-end não inicia

Na pasta `front-end`, execute:

```powershell
npm install
npm run dev
```

Se necessário, verifique se existe conflito com uma instalação antiga de `node_modules`.

## A aplicação abre, mas não carrega os dados

Verifique se:

```text
http://localhost:8080
```

está disponível e se o back-end está executando.

Também confira as configurações do front-end relacionadas à URL da API.

---

# Jornada principal da aplicação

A jornada principal implementada no projeto pode ser representada por:

```text
Paciente
   ↓
Login / Cadastro
   ↓
Home
   ↓
Busca de psicólogos
   ↓
Aplicação de filtros
   ↓
Seleção do psicólogo
   ↓
Visualização do perfil
   ↓
Agendamento da consulta
```

---

# Status do projeto

O projeto foi desenvolvido como uma **Prova de Conceito (PoC) funcional**, priorizando a implementação da jornada principal do paciente e a integração entre front-end, back-end e banco de dados.

O objetivo da PoC é demonstrar o funcionamento da plataforma e validar o fluxo central da solução.

---

## Referências

- Repositório original: https://github.com/augusto1jr/psiconnect
- Spring Boot: https://spring.io/projects/spring-boot
- Next.js: https://nextjs.org/
- React: https://react.dev/
- PostgreSQL: https://www.postgresql.org/

---

## Licença

Projeto acadêmico desenvolvido para fins educacionais.
