# Controle de Almoxarifado e Estoque

Sistema web completo para gerenciamento de almoxarifado, estoque, requisições de materiais e controle de equipamentos.

## Funcionalidades

### 📊 Dashboard
- Tempo médio de atendimento das últimas 100 requisições
- Gráfico de consumo mensal por item (com busca integrada)
- Gráfico de requisições por mês (com seletor de ano)
- Drill-down diário: clique em um mês para ver o detalhamento por dia

### 📋 Requisições
- Criação de requisições de materiais com solicitante, item, quantidade e justificativa
- Busca de itens por código ou descrição com dropdown autocomplete
- Listagem com filtros por status (Pendente, Rejeitado, Entregue) e por solicitante
- Paginação (20 requisições por página)
- Atualização de status com modal de confirmação
- Seleção de endereço de estoque para entrega
- Badge de pendências em destaque
- Exportação para Excel (`.xlsx`)

### 📦 Estoque
- Painel de estoque por endereço com indicação visual de estoque abaixo do mínimo
- **Entrada manual**: busca por código de barras (leitura de scanner) ou seleção manual
- **Transferência de estoque**: move quantidades entre endereços
- **Baixa de estoque**: registra saída de itens

### 🗂️ Controle de Equipamentos (Kanban)
- Quadro Kanban com três colunas: **Disponível**, **Em Uso** e **Em Manutenção**
- Drag-and-drop para mover equipamentos entre colunas
- Registro de retirada com seleção do responsável
- Adição, edição e exclusão de equipamentos

### 🏷️ Gerador de Etiquetas
- Geração de etiquetas com código de barras (formato CODE128)
- Três tamanhos disponíveis: 10×3 cm, 10×8 cm (2×) e 15×10 cm (3×)
- Pré-visualização antes da impressão
- Impressão em lote via arquivo `.csv` ou `.txt` (formato: `código,descrição`)
- Impressão direta pelo navegador

### 🗃️ Cadastro de Itens
- Cadastro manual com código, descrição, estoque de segurança e foto do produto
- Importação em lote via planilha Excel (`.xlsx`/`.xls`) com colunas: Código, Descrição, Estoque de Segurança, Endereço e Quantidade
- Edição e exclusão de itens
- Ativação/inativação de itens
- Ferramenta de limpeza de registros duplicados

### 👥 Cadastro de Funcionários
- Cadastro de funcionários com nome e departamento
- Edição e exclusão

### 🖼️ Catálogo de Produtos
- Visualização em grade com foto, código e descrição
- Filtro para exibir itens inativos
- Visualização ampliada de fotos ao clicar

## Tecnologias

| Tecnologia | Uso |
|---|---|
| [Firebase Firestore](https://firebase.google.com/docs/firestore) | Banco de dados em tempo real |
| [Firebase Authentication](https://firebase.google.com/docs/auth) | Autenticação (login/logout + anônimo) |
| [Firebase Storage](https://firebase.google.com/docs/storage) | Armazenamento de fotos de produtos |
| [Tailwind CSS](https://tailwindcss.com/) | Estilização responsiva |
| [Chart.js](https://www.chartjs.org/) | Gráficos do Dashboard |
| [SheetJS (xlsx)](https://sheetjs.com/) | Importação e exportação de planilhas Excel |
| [JsBarcode](https://github.com/lindell/JsBarcode) | Geração de códigos de barras |

## Como usar

### Pré-requisitos
- Navegador moderno com suporte a ES Modules (Chrome, Firefox, Edge, Safari)
- Conexão com a internet (dependências carregadas via CDN)

### Executar localmente
Basta abrir o arquivo `index.html` em um servidor web local. Exemplo com Python:

```bash
python -m http.server 8080
```

Acesse `http://localhost:8080` no navegador.

> ⚠️ Não abra o arquivo diretamente pelo sistema de arquivos (`file://`), pois os módulos ES e as requisições ao Firebase exigem um servidor HTTP.

### Configuração do Firebase
O projeto já está configurado com um projeto Firebase. Para usar seu próprio projeto:

1. Crie um projeto no [Firebase Console](https://console.firebase.google.com/)
2. Ative **Firestore**, **Authentication** (método anônimo + e-mail/senha) e **Storage**
3. Substitua o objeto `firebaseConfig` no `index.html` pelas suas credenciais:

```js
const firebaseConfig = {
    apiKey: "SUA_API_KEY",
    authDomain: "SEU_PROJETO.firebaseapp.com",
    projectId: "SEU_PROJETO",
    storageBucket: "SEU_PROJETO.appspot.com",
    messagingSenderId: "SEU_SENDER_ID",
    appId: "SEU_APP_ID"
};
```

### Acesso e permissões
- **Sem login**: visualização de requisições e criação de novas requisições
- **Com login (administrador)**: acesso completo a todas as abas — Dashboard, Estoque, Kanban, Etiquetas, Cadastro de Itens, Funcionários e Catálogo

## Estrutura do Firestore

| Coleção | Descrição |
|---|---|
| `requisitions` | Requisições de materiais |
| `items` | Cadastro de itens/produtos |
| `employees` | Cadastro de funcionários |
| `stockLocations` | Estoque por endereço |
| `kanbanEquipments` | Equipamentos do quadro Kanban |

## Formato do arquivo de importação em lote (Etiquetas)

Arquivo `.csv` ou `.txt`, uma etiqueta por linha:

```
CODIGO001,Descrição do Produto Um
CODIGO002,Descrição do Produto Dois
```

## Formato da planilha de importação de itens (Excel)

Arquivo `.xlsx` com as seguintes colunas (na ordem):

| Coluna | Descrição |
|---|---|
| Código | Código único do item |
| Descrição | Descrição do item |
| Estoque de Segurança | Quantidade mínima desejada |
| Endereço | Localização no almoxarifado (ex: `Corredor A, Prat. 2`) |
| Quantidade | Quantidade inicial em estoque |
