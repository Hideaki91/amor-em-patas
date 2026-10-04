# Amor em Patas — sistema de adoção

Aplicação web responsiva em português com backend Node.js/Express, banco SQLite e upload de fotos.

## Requisitos
- Node.js 18 ou superior
- npm

## Como executar
1. Extraia o ZIP.
2. Abra um terminal dentro da pasta `amor_em_patas`.
3. Execute `npm install`.
4. Copie `.env.example` para `.env` e altere `ADMIN_USER` e `ADMIN_PASSWORD` para valores seus, usando uma senha forte e exclusiva.
5. Execute `npm start`.
6. Abra `http://localhost:3000` no navegador.

O arquivo `adocao.db` é criado automaticamente na primeira execução. Fotos enviadas ficam em `public/uploads/`.

## Recursos
- Cadastro de animais com espécie, sexo, idade, porte, descrição, doador e foto.
- Listagem e busca de animais.
- Cadastro, edição e exclusão de doadores e adotantes.
- Solicitações de adoção com status pendente, aprovada ou recusada.
- Banco de dados SQLite persistente.

## Acesso de administrador
As credenciais são lidas pelo servidor a partir das variáveis de ambiente ou do arquivo `.env` local (carregado por `dotenv`). Nunca coloque a senha em `public/app.js` ou em arquivos do navegador. Para configuração local, copie `.env.example` para `.env` e altere os valores. O arquivo `.env` está listado no `.gitignore` e não deve ser compartilhado nem enviado ao repositório. Também é possível definir as variáveis diretamente no ambiente. No Linux/macOS:
```bash
ADMIN_USER=admin ADMIN_PASSWORD='troque-por-uma-senha-forte' npm start
```
No PowerShell do Windows:
```powershell
$env:ADMIN_USER="admin"
$env:ADMIN_PASSWORD="troque-por-uma-senha-forte"
npm start
```
Abra **Acesso administrador** no site e entre com essas credenciais. Não existe cadastro público de administradores. Somente uma sessão autenticada pode cadastrar/editar/excluir animais e cadastros de pessoas, consultar solicitações e aprovar/recusar/excluir solicitações. A autorização é conferida no servidor; ocultar os botões não é a única proteção. A sessão expira após 8 horas e é invalidada ao reiniciar o servidor.

Use uma senha longa e exclusiva. Em produção, use HTTPS (o cookie recebe a flag Secure quando `NODE_ENV=production`), segredos via gerenciador de ambiente e proteções adicionais como limitação de tentativas de login, logs de auditoria e backups. Como há dados pessoais, publique uma política de privacidade e trate os dados conforme a LGPD. Não cadastre dados reais sensíveis em uma instalação de demonstração.

## Exclusão de animais
Na galeria de animais, use o botão **Excluir animal** no cartão desejado. O sistema pede confirmação antes de excluir o registro, remove as solicitações de adoção relacionadas e tenta apagar a foto armazenada. A exclusão é protegida por autenticação administrativa no servidor.

### Permissões de animais
Os botões **Editar animal** e **Excluir animal** são exibidos somente quando há sessão de administrador. As rotas de criação (`POST /api/animals`), edição (`PUT /api/animals/:id`) e exclusão (`DELETE /api/animals/:id`) também exigem autenticação no servidor.

## Formulários públicos
O cadastro de adotante e o formulário de solicitação de adoção ficam disponíveis sem login. A solicitação recebe os dados do interessado e cria/usa o cadastro pelo e-mail, sem expor a lista de adotantes publicamente. A lista completa de adotantes e solicitações, bem como decisões e exclusões, permanece restrita ao administrador. O número da solicitação é exibido após o envio; para consultar atualizações, o interessado deve entrar em contato com a equipe informando esse número.

### Visibilidade administrativa
As seções completas de cadastro de animais e de doadores, incluindo seus títulos, tabelas e navegação, ficam ocultas para visitantes e aparecem apenas após login administrativo. O servidor também exige autenticação nas rotas de criação, edição, exclusão e listagem de doadores, além das rotas de cadastro/edição/exclusão de animais. O cadastro de adotantes e a solicitação de adoção permanecem públicos.
