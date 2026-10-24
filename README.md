# Mini-projeto: Sistema de Gerenciamento de E-commerce

**Estudante:** Leonardo Schlichmann  
**Turma:** Desenvolvedor Back-End Node [T6] 
**Módulo:** Desenvolvedor Back-End Node — Módulo 1, Semana 07

## 1. Objetivo

Aplicação de linha de comando desenvolvida em TypeScript e Node.js para demonstrar as principais operações de um pequeno e-commerce: catálogo de produtos, cadastro de clientes, carrinhos, finalização de pedidos, atualização de estoque e relatórios básicos de vendas.

## 2. Funcionalidades

- Cadastro, listagem, consulta por ID, busca parcial por nome, filtro por categoria, atualização e remoção de produtos.
- Validação de ID único, nome e categoria obrigatórios, preço maior que zero e estoque inteiro não negativo.
- Cadastro e consulta de clientes, com validação de campos, formato de e-mail, ID único e e-mail sem duplicidade (sem diferenciar maiúsculas/minúsculas).
- Criação de carrinho por cliente; inclusão, remoção e alteração de quantidade de itens.
- Atualização da linha quando o mesmo produto é adicionado novamente, sem duplicar o item.
- Cálculo de subtotal por item e total do carrinho.
- Finalização de pedidos com ID gerado, data, cliente, itens, quantidades e preços unitários históricos.
- Validação completa antes da alteração do estoque, impedindo pedido parcial em caso de erro.
- Consulta de todos os pedidos e dos pedidos por cliente.
- Relatório de faturamento, quantidade de pedidos, unidades vendidas, ticket médio, produtos mais vendidos e produtos com estoque baixo.
- Demonstração no terminal de quatro cenários inválidos, além de testes automatizados.

## 3. Tecnologias utilizadas

- Node.js 20 ou superior (recomendado).
- TypeScript 5.8 ou superior.
- Testes com o módulo nativo `node:test`, sem framework adicional de testes.
- Arrays para armazenamento em memória; não há banco de dados nem dependências de execução externas.

## 4. Estrutura dos arquivos

```text
Miniprojeto_Ecommerce_Leonardo_Schlichmann_Turma/
├── src/
│   ├── types.ts        # Tipos e interfaces do domínio
│   ├── validacoes.ts   # Validações compartilhadas e cálculos monetários
│   ├── produtos.ts     # Cadastro e gerenciamento do catálogo
│   ├── clientes.ts     # Cadastro e consulta de clientes
│   ├── carrinho.ts     # Itens, quantidades e total do carrinho
│   ├── pedidos.ts      # Finalização e consulta de pedidos
│   ├── relatorios.ts   # Consolidação dos resultados de vendas
│   └── main.ts         # Demonstração executável
├── tests/
│   └── ecommerce.test.cjs # Testes automatizados
├── dist/               # JavaScript compilado, gerado pelo build
├── package.json
├── tsconfig.json
├── .gitignore
└── README.md
```

## 5. Como instalar, compilar e executar

É necessário ter Node.js instalado. No terminal, dentro da pasta do projeto, execute:

```bash
npm install
npm run build
npm start
```

O comando `npm start` executa a demonstração no terminal. Ela inclui cadastro e consulta de dados, operações no carrinho, cálculo de total, criação de um pedido, verificação do estoque e relatório de vendas.

Para compilar novamente após alterações no código, execute `npm run build`.

## 6. Como executar os testes automatizados

```bash
npm test
```

O comando compila o TypeScript e executa os testes com `node:test`. A suíte verifica cadastro e validação de produtos/clientes, carrinho, cálculo monetário, finalização, histórico de preços, consistência de estoque e relatórios.

## 7. Exemplos das principais operações

As operações estão demonstradas em `src/main.ts`. Exemplo de uso dos módulos:

```typescript
import { Produto } from './types';
import { cadastrarProduto } from './produtos';
import { criarCarrinho, adicionarItemCarrinho, calcularTotalCarrinho } from './carrinho';

const produtos: Produto[] = [];
cadastrarProduto(produtos, {
  id: 'P001',
  nome: 'Teclado Mecânico',
  categoria: 'Periféricos',
  preco: 199.90,
  estoque: 10,
});

const carrinho = criarCarrinho('C001');
adicionarItemCarrinho(carrinho, produtos, 'P001', 2);
console.log(calcularTotalCarrinho(carrinho, produtos)); // 399.8
```

O `main.ts` apresenta ainda a alteração da quantidade e a tentativa de quatro operações inválidas. As falhas esperadas são capturadas e apresentadas no terminal sem interromper a demonstração.

## 8. Regras de negócio e decisões de implementação

1. Os IDs de produtos e clientes não podem se repetir dentro de seus cadastros.
2. O preço deve ser um número finito maior que zero; o estoque e as quantidades são inteiros não negativos/positivos, conforme a operação.
3. Os e-mails são armazenados em minúsculas e não podem se repetir.
4. A quantidade consolidada de um produto no carrinho não pode exceder o estoque disponível.
5. O carrinho não pode conter mais de uma linha para o mesmo produto quando utilizado pelas funções do módulo.
6. A finalização exige cliente existente e carrinho com itens. Todos os itens são verificados antes de qualquer baixa de estoque.
7. O pedido guarda nome, categoria e preço unitário no momento da compra; uma mudança posterior no catálogo não altera o histórico.
8. Valores monetários são calculados em centavos durante as somas para reduzir erros de ponto flutuante.
9. A remoção de um produto do catálogo não apaga registros já feitos em pedidos, pois cada item do pedido mantém uma cópia dos dados relevantes.
10. O relatório considera estoque baixo como cinco unidades ou menos por padrão; esse limite pode ser informado como argumento.

## 9. Limitações conhecidas

- Os dados são mantidos apenas em memória e são perdidos quando o processo é encerrado.
- Não há API REST, interface web, autenticação, banco de dados, pagamento, frete, cupons ou impostos, pois não fazem parte dos requisitos mínimos deste desafio.
- O projeto não implementa persistência nem concorrência de compras entre processos.
- Se um produto for removido do catálogo enquanto estiver em um carrinho existente, esse item deverá ser removido do carrinho antes da finalização. Pedidos já registrados preservam seus dados históricos.
- A validação de e-mail verifica uma estrutura básica, mas não confirma a existência da caixa postal.

