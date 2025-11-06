# 🧠 JavaScript Intermediário — Arrays, Objetos, Funções e Loops (README)

## Visão geral

Este material segue a sequência de conteúdos já trabalhados (arrays, objetos e funções). O objetivo desta unidade (4 horas) é consolidar o uso de arrays e objetos combinados com funções e estruturas de repetição, com atenção especial aos laços `for...in` e `for...of`. O texto foi escrito passo a passo para alunos com dificuldade em abstração — cada conceito tem exemplos claros, explicações lineares e exercícios práticos.

---

## Sumário

1. Arrays — lembrete e operações básicas
2. Objetos — lembrete e acesso às propriedades
3. Funções — parâmetros, retorno e modularização
4. Loops básicos — `for` e `while` (revendo)
5. Laços específicos — `for...in` e `for...of` (explicação detalhada)
6. Exemplo integrado: sistema de pedidos (modo texto)
7. Exercícios com gabarito (respostas comentadas)


---

## 1) Arrays — lembrete rápido

### O que é

Array é uma lista ordenada de valores. Cada posição tem um índice (começa em 0).

### Criar e acessar

```js
const frutas = ["maçã", "banana", "laranja"];
console.log(frutas[0]); // "maçã"
console.log(frutas.length); // 3
```

### Operações básicas

```js
// adicionar no final
frutas.push("uva");

// remover do final
const ultimo = frutas.pop();

// adicionar no início
frutas.unshift("morango");

// remover do início
const primeiro = frutas.shift();
```

---

## 2) Objetos — lembrete rápido

### O que é

Objeto é uma coleção de pares chave:valor. Serve para representar entidades com propriedades.

### Criar e acessar

```js
const produto = {
  nome: "Coxinha",
  preco: 6.0,
  emEstoque: true
};

console.log(produto.nome);        // "Coxinha"
console.log(produto["preco"]);    // 6.0
```

### Alterar propriedades

```js
produto.preco = 6.5;
produto.categoria = "Salgados";
delete produto.emEstoque;
```

---

## 3) Funções — parâmetros e retorno (boas práticas)

### Declaração de função

```js
function soma(a, b) {
  return a + b;
}

const resultado = soma(3, 5); // 8
```

### Expressão de função e arrow function

```js
const multiplica = function(a, b) {
  return a * b;
};

const subtrai = (a, b) => a - b;
```

### Funções com responsabilidade única

* Cada função deve executar **apenas uma** tarefa.
* Funções pequenas são fáceis de entender e testar.

Exemplo:

```js
function calcularTotal(produtos) {
  let total = 0;
  for (let i = 0; i < produtos.length; i++) {
    total += produtos[i].preco;
  }
  return total;
}

function formatarPreco(valor) {
  return `R$ ${valor.toFixed(2)}`;
}
```

---

## 4) Loops básicos — `for` e `while` (revisão)

### `for` (quando sabemos quantas repetições)

```js
for (let i = 0; i < 5; i++) {
  console.log(i); // 0,1,2,3,4
}
```

### `while` (quando dependemos de condição)

```js
let contador = 1;
while (contador <= 5) {
  console.log(contador);
  contador++;
}
```

---

## 5) Laços específicos: `for...in` e `for...of` — explicação detalhada

### Diferença essencial (resumo)

* `for...in` → itera **chaves** (propriedades) de um objeto ou índices de um array (retorna strings).
* `for...of` → itera **valores** de um objeto iterável (array, string, Map, Set) — retorna os elementos reais.

### `for...in` — quando usar e como entender

* Use `for...in` para percorrer **propriedades de um objeto**.
* Quando usado em arrays, itera **os índices** (como strings `"0"`, `"1"`), não é ideal para obter valores diretos.

Exemplos:

```js
// Percorrendo um objeto
const pessoa = { nome: "Ana", idade: 20, cidade: "SP" };

for (const chave in pessoa) {
  console.log(chave);            // "nome", "idade", "cidade"
  console.log(pessoa[chave]);    // "Ana", 20, "SP"
}

// Percorrendo um array (retorna índices)
const arr = ["a", "b", "c"];
for (const i in arr) {
  console.log(i);      // "0", "1", "2"  (note: são strings)
  console.log(arr[i]); // "a", "b", "c"
}
```

**Cuidados com `for...in` em arrays**

* A ordem é geralmente crescente dos índices, mas não é garantida para arrays com propriedades inválidas ou quando o protótipo foi modificado.
* Se alguém adicionou propriedades ao protótipo do array ou propriedades não-numéricas ao próprio array, `for...in` também as retornará.
* Por isso, para arrays modernos prefira `for`, `for...of` ou métodos como `forEach` (quando permitidos).

### `for...of` — quando usar e como entender

* Use `for...of` para percorrer **valores** de arrays, strings, Maps, Sets, NodeLists, etc.
* `for...of` devolve os elementos em si, de forma direta.

Exemplos:

```js
const arr = ["x", "y", "z"];
for (const valor of arr) {
  console.log(valor); // "x", "y", "z"
}

// Percorrendo uma string
const texto = "JS";
for (const letra of texto) {
  console.log(letra); // "J", "S"
}

// Com array de objetos
const produtos = [
  { nome: "Coxinha", preco: 6 },
  { nome: "Suco", preco: 4 }
];

for (const produto of produtos) {
  console.log(produto.nome); // "Coxinha", "Suco"
}
```

**Vantagens do `for...of`**

* Código mais limpo quando precisamos só do valor.
* Não traz índices indesejados ou propriedades herdadas.
* Ideal para arrays e coleções iteráveis.

### Quando usar cada um — regras práticas

* Se você precisa **das chaves/propriedades** de um objeto → `for...in`.
* Se você precisa **dos valores** de um array ou iterável → `for...of`.
* Se você precisa **do índice** e do valor ao mesmo tempo, prefira o `for` tradicional ou usar `arr.entries()` com `for...of`:

```js
for (const [i, valor] of arr.entries()) {
  console.log(i, valor);
}
```

---

## 6) Exemplo integrado — Mini sistema de pedidos (modo texto, sem DOM)

### Objetivo

Montar funções que manipulam arrays de objetos (produtos e pedidos), usar `for`, `for...of` e `for...in` quando apropriado, e consolidar boas práticas.
> vamos fazer um sistema simples de pedidos para uma lanchonete juntos.

### Observações sobre o exemplo

* Usamos `for...of` para percorrer arrays de objetos (`produtos` e `pedido`) pois precisamos dos valores.
* Construímos um objeto `resumo` e usamos `for...in` para iterar sobre as chaves desse objeto (nomes dos produtos).
* A lógica está separada em funções com responsabilidade única: listar, adicionar e resumir.

---

## 7) Exercícios (pra sala) e gabarito

### Exercício 1

Criar uma função `somaArray(numeros)` que recebe um array de números e retorna a soma.



### Exercício 2

Dado o array de objetos `alunos = [{nome: "A", nota: 7}, {nome: "B", nota: 5}]`, criar função `aprovados(alunos)` que retorna um array com os nomes dos alunos com nota >= 6.

### Exercício 3

Com base no `pedido` do exemplo integrado, criar função `contarItens(pedido)` que retorna um objeto com a quantidade por nome (mesmo formato do `resumo`).



