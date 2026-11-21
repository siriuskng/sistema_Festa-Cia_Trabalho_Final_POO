# Festa e Cia

Sistema desktop para gerenciamento de clientes, inventário e locações de itens para festas.
Projeto Final de POO.

**Integrantes:** (Jairo e Raphael)

## Descrição do sistema
Uma locadora de itens para festas controla clientes, pedidos e estoque de forma manual, o que gera
perda de dados e erros de disponibilidade. O Festa e Cia centraliza o cadastro de clientes, o
inventário e os pedidos de locação, garantindo que um pedido só seja aceito se houver estoque e que
o saldo seja atualizado automaticamente. A interface prioriza alto contraste, fontes grandes e
navegação óbvia, pensada para atendentes de qualquer idade, incluindo usuários idosos.



## Tecnologias
Java 21 · JavaFX 21 · MySQL 8 · JDBC · JasperReports · Maven · Git

## Como configurar o banco de dados
1. Instale e inicie o MySQL 8.0.16 ou superior.
2. Execute o script `schema.sql` (MySQL Workbench, ou `mysql -u root -p < schema.sql` no terminal).
   Isso cria o banco `festa_e_cia`, as tabelas e alguns dados de exemplo.
3. Ajuste usuário e senha em `src/main/resources/db.properties`.

## Como executar 
execute o launcher
## Como gerar o instalador
1. Rode `build-installer.bat` (Windows) ou `./build-installer.sh` (Linux/macOS) na raiz do projeto.
2. O script compila o projeto, junta todas as dependências em um único jar e usa o `jpackage`
   (que já vem com o JDK 17 ou mais recente) para montar o programa pronto para instalar, na pasta
   `instalador/Festa e Cia`.
3. Essa pasta já inclui o Java dentro dela — para instalar em outro computador, basta copiá-la
   inteira; não é preciso instalar Java nem JavaFX lá. O MySQL continua sendo necessário à parte
   (veja "Como configurar o banco de dados").
4. Por padrão, o script gera uma pasta de programa (`--type app-image`), que funciona em qualquer
   Windows/Linux/macOS sem ferramentas extras. Para gerar um instalador de verdade com atalho e
   desinstalador (`.msi` no Windows ou `.deb` no Linux), é preciso instalar o WiX Toolset (Windows,
   wixtoolset.org) ou o pacote `dpkg-dev` (Linux), e então trocar `--type app-image` por `--type msi`
   ou `--type deb` dentro do script.

## Funcionalidades implementadas
- Menu principal acessível (alto contraste, fontes grandes, navegação por botões) com verificação
  da conexão com o banco.
- CRUD de Itens (estoque): cadastro, busca, alteração e exclusão, com validações e controle de
  quantidade disponível.
- CRUD de Clientes: cadastro, busca, alteração e exclusão (bloqueada para clientes que já possuem
  pedidos, para preservar o histórico).
- CRUD de Pedidos: registro de pedidos com seleção de cliente e de itens, cálculo automático do
  valor total e baixa de estoque em uma única transação (com verificação de linha travada no banco,
  para não permitir dois pedidos aprovarem a mesma última unidade ao mesmo tempo); mudança de status
  (Agendado → Entregue → Devolvido, ou Cancelado a partir de Agendado), com devolução automática dos
  itens ao estoque; exclusão restrita a pedidos ainda agendados.
- Consultas personalizadas: entregas no período selecionado (cliente, itens e valor) e ranking de
  itens mais alugados no período, acessíveis pelo botão "Consultas e Relatórios".
- Relatórios JasperReports em PDF: Agenda de Entregas e Itens Mais Alugados, gerados a partir das
  consultas acima (botões na mesma tela). Os PDFs são salvos na pasta `relatorios/` e abertos
  automaticamente se o computador tiver um leitor de PDF associado.
- Validações e mensagens de sucesso/erro em todas as telas, e confirmação antes de qualquer exclusão.

## Ainda falta
- Testar a instalação (pasta gerada pelo `jpackage`) em um computador diferente do usado no
  desenvolvimento, como pede o checklist da disciplina.

