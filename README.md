# Prisma Guide

Este guia fornece uma visão geral de como usar o Prisma, um ORM (Object-Relational Mapping) moderno para Node.js e TypeScript.

## Sumário

1. [O que é o Prisma?](#o-que-é-o-prisma)
2. [Migrações com Prisma](#migrações-com-prisma)
3. [Deploy de Migrações](#deploy-de-migrações)
4. [Resetando o Banco de Dados](#resetando-o-banco-de-dados)
5. [Recuperação de Dados](#recuperação-de-dados)
6. [Fazendo Backup](#fazendo-backup)
7. [Backup para SQL e Outras Opções](#backup-para-sql-e-outras-opções)
8. [Aplicando Migrações Específicas](#aplicando-migrações-específicas)

## O que é o Prisma?

Prisma é um ORM de próxima geração que consiste em três ferramentas principais:

- Prisma Client: Um construtor de consultas auto-gerado e type-safe para Node.js e TypeScript
- Prisma Migrate: Um sistema de migração de banco de dados
- Prisma Studio: Uma interface de usuário para visualizar e editar dados no seu banco de dados

## Migrações com Prisma

Migrações são usadas para gerenciar mudanças no esquema do banco de dados ao longo do tempo.

### Como gerar uma migração

Para gerar uma migração, siga estes passos:

1. Faça alterações no seu arquivo `schema.prisma`.
2. Execute o comando:

   \`\`\`
   npx prisma migrate dev --name nome_da_sua_migracao
   \`\`\`

Este comando irá:

- Gerar um novo arquivo de migração SQL baseado nas mudanças do seu schema
- Executar a migração no seu banco de dados
- Gerar o Prisma Client atualizado [^1]

### Para que servem as migrações?

Migrações servem para:

- Manter o histórico de mudanças do seu banco de dados
- Facilitar a colaboração em equipe
- Permitir rollbacks para versões anteriores do schema
- Automatizar atualizações de banco de dados em diferentes ambientes [^2]

## Deploy de Migrações

Para fazer deploy de suas migrações em um ambiente de produção, use o comando:

\`\`\`
npx prisma migrate deploy
\`\`\`

Este comando aplicará todas as migrações pendentes ao seu banco de dados de produção de forma segura [^3].

## Resetando o Banco de Dados

Para resetar completamente seu banco de dados:

\`\`\`
npx prisma migrate reset
\`\`\`

Este comando irá:

1. Deletar todos os dados e tabelas no banco de dados
2. Recriar todas as tabelas aplicando todas as migrações
3. Executar seed scripts (se configurados) [^4]

## Recuperação de Dados

Para recuperar dados após uma alteração acidental:

1. Restaure um backup anterior (veja a seção de backup abaixo)
2. Use o comando \`prisma db pull\` para atualizar seu schema Prisma com base no estado atual do banco de dados:

   \`\`\`
   npx prisma db pull
   \`\`\`

3. Gere o Prisma Client atualizado:

   \`\`\`
   npx prisma generate
   \`\`\`

## Fazendo Backup

Prisma não fornece uma ferramenta de backup integrada. Para fazer backup do seu banco de dados:

1. Use ferramentas específicas do seu banco de dados (por exemplo, pg_dump para PostgreSQL)
2. Configure backups automáticos através do seu provedor de hospedagem de banco de dados
3. Para projetos menores, você pode usar o Prisma Studio para exportar dados como JSON

Exemplo de backup com pg_dump (PostgreSQL):

\`\`\`
pg_dump -U seu_usuario -d seu_banco_de_dados > backup.sql
\`\`\`

Lembre-se de configurar backups regulares e testar o processo de restauração periodicamente.

## Backup para SQL e Outras Opções

Para bancos de dados SQL, você pode usar ferramentas específicas de cada sistema de gerenciamento de banco de dados (SGBD) para realizar backups. Aqui estão alguns exemplos:

### MySQL

Para fazer backup de um banco de dados MySQL:

\`\`\`
mysqldump -u seu_usuario -p seu_banco_de_dados > backup.sql
\`\`\`

Para restaurar:

\`\`\`
mysql -u seu_usuario -p seu_banco_de_dados < backup.sql
\`\`\`

### PostgreSQL

Para fazer backup de um banco de dados PostgreSQL:

\`\`\`
pg_dump -U seu_usuario -d seu_banco_de_dados > backup.sql
\`\`\`

Para restaurar:

\`\`\`
psql -U seu_usuario -d seu_banco_de_dados < backup.sql
\`\`\`

### SQLite

Para fazer backup de um banco de dados SQLite:

\`\`\`
sqlite3 seu_banco_de_dados .dump > backup.sql
\`\`\`

Para restaurar:

\`\`\`
sqlite3 novo_banco_de_dados < backup.sql
\`\`\`

## Aplicando Migrações Específicas

O Prisma permite aplicar migrações específicas ou reverter para uma migração específica. Isso pode ser útil em cenários de desenvolvimento ou quando você precisa fazer rollback de mudanças específicas.

### Aplicar uma migração específica

Para aplicar uma migração específica, você pode usar o comando \`prisma migrate resolve\`:

\`\`\`
npx prisma migrate resolve --applied "nome_da_migracao"
\`\`\`

Este comando marca a migração como aplicada sem realmente executá-la. Isso é útil se você aplicou a migração manualmente ou se está resolvendo conflitos.

### Reverter para uma migração específica

O Prisma não fornece um comando direto para reverter migrações, mas você pode seguir estes passos:

1. Identifique o ID da migração para a qual você quer reverter.
2. Crie uma nova migração que desfaz as mudanças das migrações que você quer reverter.
3. Aplique esta nova migração.

Por exemplo:

\`\`\`
npx prisma migrate dev --name revert_to_specific_migration
\`\`\`

Então, edite o arquivo de migração gerado para incluir as operações SQL necessárias para reverter as mudanças.

### Dica de Segurança

Sempre faça um backup do seu banco de dados antes de aplicar ou reverter migrações, especialmente em ambientes de produção.

Lembre-se de que manipular migrações manualmente pode levar a inconsistências entre seu schema Prisma e o estado real do banco de dados. Use essas operações com cautela e sempre teste em um ambiente de desenvolvimento primeiro [^5].

---

Este README fornece uma visão geral básica do Prisma e suas funcionalidades relacionadas a migrações e gerenciamento de banco de dados. Para informações mais detalhadas, consulte a [documentação oficial do Prisma](https://www.prisma.io/docs/).

[^1]: [Prisma Client | Prisma Docs](https://www.prisma.io/docs/reference/api-reference/prisma-client-reference)
[^2]: [Prisma Migrate | Prisma Docs](https://www.prisma.io/docs/concepts/components/prisma-migrate)
[^3]: [Prisma Migrate deploy | Prisma Docs](https://www.prisma.io/docs/reference/cli-reference/migrate-deploy)
[^4]: [Prisma Migrate reset | Prisma Docs](https://www.prisma.io/docs/reference/cli-reference/migrate-reset)
[^5]: [Prisma Migrate | Prisma Docs](https://www.prisma.io/docs/concepts/components/prisma-migrate)
