# Resumo do que aprendi em C# e ASP.NET Core

Durante esse exercício eu aprendi vários conceitos básicos de C# e também comecei a entender melhor como funciona uma API com ASP.NET Core.

---

## Sumário

1. [Classes, tipos e propriedades](#1-classes-tipos-e-propriedades)
2. [Variáveis e tipos](#2-variáveis-e-tipos)
3. [Métodos, retorno e parâmetros](#3-métodos-retorno-e-parâmetros)
4. [Operador `+=`](#4-operador-)
5. [Modificadores: public, private, internal, static e readonly](#5-modificadores-public-private-internal-static-e-readonly)
6. [Listas com `List<T>`](#6-listas-com-listt)
7. [Controller, atributos e herança](#7-controller-atributos-e-herança)
8. [IActionResult e interfaces](#8-iactionresult-e-interfaces)
9. [Endpoints GET e POST](#9-endpoints-get-e-post)
10. [Testando com arquivo `.http`](#10-testando-com-arquivo-http)
11. [Arquitetura em camadas](#11-arquitetura-em-camadas)
12. [Service e injeção de dependência](#12-service-e-injeção-de-dependência)
13. [Endpoint PUT, `var`, LINQ e lambda](#13-endpoint-put-var-linq-e-lambda)
14. [Verbos HTTP](#14-verbos-http)
15. [Resumo final](#15-resumo-final)

---

## 1. Classes, tipos e propriedades

Primeiro eu criei uma classe chamada `Funcionario`. Essa classe representa um tipo dentro do sistema. Quando eu crio uma classe como `Funcionario`, eu também estou criando um **novo tipo** que pode ser usado em variáveis, parâmetros e listas.

```csharp
public class Funcionario
{
    public int Id { get; set; }
    public string Nome { get; set; }
    public decimal Salario { get; set; }
}
```

- `Funcionario` é a **classe** e também o **tipo**.
- `Id`, `Nome` e `Salario` são **propriedades** da classe.
- `get` permite **ler** o valor da propriedade.
- `set` permite **alterar** o valor.

> ⚠️ `{ get; set; }` **não é um construtor**. Um construtor é normalmente um método com o mesmo nome da classe, usado para inicializar o objeto.

---

## 2. Variáveis e tipos

```csharp
Funcionario funcionario;
```

- `Funcionario` (maiúsculo) é o **tipo**.
- `funcionario` (minúsculo) é apenas o **nome da variável**.

Eu poderia chamar essa variável de `joao`, `maria`, `func`, `batata` ou qualquer outro nome válido:

```csharp
Funcionario joao = new Funcionario();
```

| Parte                | Significado                          |
|----------------------|--------------------------------------|
| `Funcionario`        | Tipo                                 |
| `joao`               | Variável                             |
| `new Funcionario()`  | Cria uma nova instância da classe    |

### Tipos nativos x tipos criados por mim

Tipos que já vêm com o C#:

- `int`
- `string`
- `decimal`
- `bool`
- `double`

E tipos que eu mesmo crio, como `Funcionario`.

```csharp
decimal porcentagem
```

Aqui `decimal` é o tipo e `porcentagem` é o nome da variável.

---

## 3. Métodos, retorno e parâmetros

Métodos são funções que ficam **dentro de uma classe**.

```csharp
public void AumentarSalario(Funcionario funcionario, decimal porcentagem)
{
}
```

- `AumentarSalario` é o **método**.
- `public` é o **modificador de acesso** — o método pode ser acessado de fora da classe.
- `void` significa que o método executa uma ação, mas **não devolve nenhum valor**.

### Métodos com retorno

Se o método precisar devolver um valor, no lugar de `void` coloco o tipo do retorno:

```csharp
public decimal CalcularSalario()
{
    return 4500;
}
```

Nesse caso, o método retorna um `decimal`.

### Parâmetros

```csharp
public void AumentarSalario(Funcionario funcionario, decimal porcentagem)
```

O método precisa receber **duas informações**:

1. `Funcionario funcionario` → um objeto do tipo `Funcionario`, que dentro do método será chamado de `funcionario`.
2. `decimal porcentagem` → um número decimal, que dentro do método será chamado de `porcentagem`.

Chamando o método:

```csharp
service.AumentarSalario(maria, 10);
```

- `maria` entra no parâmetro `funcionario`.
- `10` entra no parâmetro `porcentagem`.

### Cálculo do aumento salarial

```csharp
funcionario.Salario += funcionario.Salario * porcentagem / 100;
```

---

## 4. Operador `+=`

O operador `+=` significa **pegar o valor atual e somar mais alguma coisa**.

```csharp
funcionario.Salario += 400;
```

É equivalente a:

```csharp
funcionario.Salario = funcionario.Salario + 400;
```

---

## 5. Modificadores: public, private, internal, static e readonly

| Modificador | Significado |
|-------------|-------------|
| `public`    | Outras classes podem acessar aquele membro. |
| `private`   | Apenas a própria classe consegue acessar diretamente. |
| `internal`  | O acesso fica limitado ao mesmo projeto ou assembly. |
| `static`    | O membro pertence à **classe**, e não a uma instância específica. |
| `readonly`  | Depois de receber um valor, a referência não pode ser trocada. |

Exemplo:

```csharp
private static readonly List<Funcionario> funcionarios = new();
```

- Existe **uma única lista** de funcionários compartilhada pela classe (`static`).
- Não posso trocar a referência por outra lista (`readonly`).
- **Porém**, ainda posso modificar o **conteúdo** da lista:

```csharp
funcionarios.Add(funcionario); // funciona normalmente
```

---

## 6. Listas com `List<T>`

```csharp
List<Funcionario> funcionarios = new List<Funcionario>();
```

Isso cria uma lista que aceita objetos do tipo `Funcionario`.

```csharp
funcionarios.Add(funcionario);
```

O método `Add` vem da própria classe `List<T>` do .NET e serve para **adicionar um item** à lista.

---

## 7. Controller, atributos e herança

Na API eu criei uma `FuncionarioController`:

```csharp
[ApiController]
[Route("api/[controller]")]
public class FuncionarioController : ControllerBase
{
}
```

- `[ApiController]` e `[Route]` são **atributos**. Em C#, atributos usam **colchetes**.
- `FuncionarioController : ControllerBase` significa que `FuncionarioController` **herda** de `ControllerBase`.
- O caractere `:` nesse caso representa **herança**.

O `ControllerBase` fornece recursos úteis para uma API, como:

- `Ok()`
- `NotFound()`
- `BadRequest()`

---

## 8. IActionResult e interfaces

```csharp
public IActionResult Get()
```

`IActionResult` é uma **interface** usada para representar diferentes tipos de resposta HTTP. O mesmo método pode retornar:

```csharp
return Ok();
```

ou:

```csharp
return NotFound();
```

### O que é uma interface?

Uma interface funciona como um **contrato**. Ela define **o que** uma classe deve oferecer, enquanto a classe concreta implementa **como** aquilo será feito.

---

## 9. Endpoints GET e POST

### GET

```csharp
[HttpGet]
public IActionResult Get()
{
    return Ok(funcionarios);
}
```

O `[HttpGet]` informa que esse método responde a requisições **HTTP GET**.

Ao acessar `/api/Funcionario`, a API retornou:

```json
[]
```

Isso significava que o endpoint estava funcionando, mas a lista ainda estava vazia.

### POST

```csharp
[HttpPost]
public IActionResult Post(Funcionario funcionario)
{
    funcionarios.Add(funcionario);

    return Ok(funcionario);
}
```

O POST recebe um objeto `Funcionario`, adiciona na lista e devolve o funcionário na resposta.

---

## 10. Testando com arquivo `.http`

O arquivo `.http` do Visual Studio pode ser usado para testar uma API sem precisar obrigatoriamente do Postman.

```http
POST {{CadastroApi_HostAddress}}/api/Funcionario
Content-Type: application/json

{
  "id": 1,
  "nome": "Gilson",
  "salario": 4500
}
```

> 💡 Cada POST recebe **um JSON por vez**, porque o endpoint recebe apenas um `Funcionario`. Para cadastrar três funcionários, faço três requisições separadas.

---

## 11. Arquitetura em camadas

| Camada         | Responsabilidade |
|----------------|------------------|
| **Model**      | Representa os dados |
| **Controller** | Recebe as requisições HTTP |
| **Service**    | Contém as regras de negócio |
| **Repository** | Responsável pelo acesso aos dados (quando existir) |

Fluxo completo:

```
Cliente
   ↓
Controller
   ↓
Service
   ↓
Repository
   ↓
Banco de Dados
```

No exercício atual ainda não existe banco de dados nem Repository:

```
Cliente
   ↓
Controller
   ↓
Service
   ↓
Funcionario
```

---

## 12. Service e injeção de dependência

Criamos um `FuncionarioService` para manter a regra de aumento salarial **fora da Controller**:

```csharp
public class FuncionarioService
{
    public void AumentarSalario(Funcionario funcionario, decimal porcentagem)
    {
        funcionario.Salario += funcionario.Salario * porcentagem / 100;
    }
}
```

### Campo privado na Controller

```csharp
private readonly FuncionarioService _funcionarioService;
```

- `FuncionarioService` é o **tipo**.
- `_funcionarioService` é o **nome do campo** da classe.

> O underline `_` não tem comportamento especial. É apenas uma **convenção** em C# para indicar campos privados.

### Construtor

```csharp
public FuncionarioController(FuncionarioService funcionarioService)
{
    _funcionarioService = funcionarioService;
}
```

O ASP.NET **entrega** um objeto `FuncionarioService` para a Controller, e eu guardo esse objeto no campo `_funcionarioService`.

### Registro no `Program.cs`

```csharp
builder.Services.AddScoped<FuncionarioService>();
```

Isso permite que o sistema de injeção de dependência do ASP.NET saiba **como criar** um `FuncionarioService`.

---

## 13. Endpoint PUT, `var`, LINQ e lambda

```csharp
[HttpPut("{id}/aumentar-salario")]
public IActionResult AumentarSalario(int id, decimal porcentagem)
{
    var funcionario = funcionarios.FirstOrDefault(f => f.Id == id);

    if (funcionario == null)
    {
        return NotFound("Funcionário não encontrado.");
    }

    _funcionarioService.AumentarSalario(funcionario, porcentagem);

    return Ok(funcionario);
}
```

### `var`

```csharp
var funcionario = ...
```

O `var` permite que o **compilador descubra automaticamente o tipo** da variável analisando o valor que está sendo atribuído.

### LINQ e `FirstOrDefault`

```csharp
funcionarios.FirstOrDefault(f => f.Id == id);
```

`FirstOrDefault` é um método do **LINQ**. Ele procura o **primeiro item** da lista que satisfaça a condição. Se não encontrar, retorna `null`.

### Expressão lambda

```csharp
f => f.Id == id
```

O `f` é apenas o nome da variável temporária que representa cada funcionário durante a busca. Poderia ser qualquer nome:

```csharp
funcionarios.FirstOrDefault(funcionario => funcionario.Id == id);
funcionarios.FirstOrDefault(batata => batata.Id == id);
```

O importante é a **condição**:

```csharp
f.Id == id
```

> Procure um funcionário cuja propriedade `Id` seja igual ao `id` que veio pela rota.

### Exemplo prático

```http
PUT /api/Funcionario/2/aumentar-salario?porcentagem=10
```

- O parâmetro `id` vale `2`.
- O LINQ procura `f.Id == 2`.
- Se encontrar, retorna aquele funcionário.
- Se não encontrar, retorna `null` → por isso o `if` com `NotFound`.

### Fluxo completo do PUT

```
PUT enviado pelo cliente
        ↓
FuncionarioController
        ↓
procura funcionário pelo Id
        ↓
FuncionarioService
        ↓
aplica regra do aumento
        ↓
Controller retorna 200 OK
```

---

## 14. Verbos HTTP

| Verbo  | Uso comum |
|--------|-----------|
| `GET`  | Consultar dados |
| `POST` | Criar dados |
| `PUT`  | Alterar dados existentes |

---

## 15. Resumo final

Esse exercício me ajudou a entender conceitos que eu usava ou via no código, mas ainda não compreendia completamente:

- Diferença entre classe, objeto, tipo e variável
- Parâmetros e métodos
- `void`
- Propriedades com `get` e `set`
- `static`, `readonly`, `private`, `internal`
- `IActionResult` e interfaces
- Herança
- `List<T>`
- LINQ, `FirstOrDefault` e expressões lambda
- Controller e Service
- Injeção de dependência

### Responsabilidades principais

| Conceito       | Papel |
|----------------|-------|
| **Model**      | Representa os dados |
| **Controller** | Recebe a requisição HTTP |
| **Service**    | Executa as regras de negócio |
| **Repository** | Acessa/persiste os dados |
| **Interface**  | Define um contrato |
| **DTO**        | Representa os dados de entrada e saída da API |

### Fluxo de uma aplicação organizada

**Ida (requisição):**

```
HTTP Request
     ↓
Controller
     ↓
Service
     ↓
Repository
     ↓
Banco de Dados
```

**Volta (resposta):**

```
Banco
  ↓
Repository
  ↓
Service
  ↓
Controller
  ↓
HTTP Response
```

---

> 📌 **Conclusão:** entender C# não é apenas decorar sintaxe. É entender o que cada parte representa, de onde cada variável vem e qual responsabilidade cada classe deve ter.
