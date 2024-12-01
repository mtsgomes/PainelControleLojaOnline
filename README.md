Descrição do Projeto

Este projeto envolve a criação de um painel de controle para a gestão das informações de uma loja online, utilizando dados fornecidos pela API Fake Store. O painel oferece funcionalidades para visualizar e manipular informações relacionadas a produtos, categorias, carrinhos (pedidos) e usuários. Foi desenvolvido utilizando Vue.js 3 e Vite, proporcionando uma experiência interativa e otimizada.
Funcionalidades do Painel

Indicadores Principais:

    Total de Produtos: Exibe o número total de produtos registrados.
    Número de Categorias: Mostra o total de categorias disponíveis.
    Quantidade de Pedidos: Indica o número total de carrinhos/pedidos.
    Número de Usuários: Apresenta a quantidade de usuários cadastrados na plataforma.

Gerenciamento de Produtos:

    Visualizar Produtos: Permite exibir todos os produtos cadastrados.
    Detalhes de um Produto: Exibe informações detalhadas sobre um produto específico.
    Filtrar e Ordenar Produtos: Funcionalidade para filtrar e ordenar os produtos conforme critérios definidos.
    Adicionar, Editar e Remover Produtos: Permite adicionar novos produtos, atualizar informações existentes ou remover produtos.

Gerenciamento de Carrinhos:

    Listar Carrinhos: Exibe uma lista de todos os carrinhos de compras.
    Visualizar Detalhes de um Pedido: Permite ver as informações de um pedido/carrinho específico.
    Filtrar Carrinhos por Data ou Usuário: Filtros para visualizar carrinhos com base em data ou usuário.
    Adicionar, Editar e Remover Carrinhos: Funcionalidade para gerenciar os carrinhos de compras.

Gerenciamento de Usuários:

    Listar Usuários: Exibe todos os usuários registrados.
    Ver Detalhes de um Usuário: Apresenta informações detalhadas sobre um usuário específico.
    Buscar Usuários pelo Nome: Funcionalidade de pesquisa por nome de usuário.
    Adicionar, Editar e Remover Usuários: Permite gerenciar as informações dos usuários.

Autenticação:

    Tela de Login: Requer login para acessar o painel de controle. A autenticação é feita com o e-mail e os últimos 4 dígitos do telefone do usuário.

Tecnologias Utilizadas

    Vue.js 3: Framework JavaScript para criar interfaces dinâmicas e reativas.
    Vite: Ferramenta moderna para construção e desenvolvimento rápido.
    Axios: Biblioteca para realizar requisições HTTP de maneira simples e eficiente.
    Vue Router: Biblioteca para gerenciamento de rotas no Vue.js.

Executar o projeto

    Clone o repositório

git clone https://github.com/mtsgomes/PainelControleLojaOnline
cd PainelControleLojaOnline

    Instalar dependências.

npm install

    Iniciar servidor de desenvolvimento.

npm run dev

    Acessar o localhost fornecido pelo terminal

Login para teste

Para fazer login, deve-se usar um e-mail de um usuário registrado na API e os 4 últimos dígitos do telefone do mesmo. Exemplo:

Usuário: john@gmail.com
Senha: 7033

