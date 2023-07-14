# Arquitetura

**Antes de mais:** `Alguns termos estarão em inglês, pois como programadores de lingua portuguesa, devemos sempre escrever os nomes das variáveis, classes... em inglês e existem termos que simplesmente não tem tradução.`

- [Model](#model)
  - [Naming](#naming)
  - [Best practices](#best-practices)
  - [Structure](#structure)
- [Controller](#controller)
  - [Naming](#naming-1)
  - [Types](#types)
  - [Best practices](#best-practices-1)
  - [Namespacing](#namespacing)
- [Request](#request)
  - [Naming](#naming-2)
- [Configuration](#configuration)
  - [Best practices](#best-practices-2)
- [Action](#action)
  - [Naming](#naming-3)
  - [Best practices](#best-practices-3)
- [Support](#support)
  - [Naming](#naming-4)
- [Routing](#routing)
  - [Route Types](#route-types)
  - [Best practices](#best-practices-4)
- [Middleware](#middleware)
  - [Usage example](#usage-example)
- [Observer](#observer)
  - [Naming](#naming-5)
  - [Usage example](#usage-example-1)
- [Event](#event)
  - [Dispatching events](#dispatching-events)
  - [Listeners](#listeners)
- [Command](#command)
  - [Scheduling commands](#scheduling-commands)

---

<a name="model"></a>

# Model

Models são classes que representam as tabelas das bases de dados. Permitindo interagir com os dados correspondentes utilizando sintax orientada a objetos.

Noutras palavras, models permitem uma maneira fácil de pesquisar, inserir, atualizar e apagar dados da tabela na base de dados. [Ler Mais](https://laravel.com/docs/eloquent)
**Comando para Criar:** `php artisan make:model Product`


```php
class Product extends Model
{
    use HasFactory;
}
```

<a name="naming"></a>

## Nomenculatura

Singular sem o sufixo "Model" (`User`, `Product`, `Category`...)

<a name="best-practices"></a>

## Boas práticas

- Seguir a estrutura definida.
- Models devem conter apenas coisas Nativas do Laravel (relações, contexto(scope)...) e código relacionado com a base de dados.
  - Lógicas de negócio complexa devem ser escritas nas classes `Support` ou `Action`.
- Usar [mass assignment](https://laravel.com/docs/eloquent#mass-assignment) onde for possível.
- Preferir `$fillable` em vez de `$guarded` pois traz mais segurança.

<a name="structure"></a>

## Estrutura

- **Atributos** - `$fillable`, `$casts`…
- **Métodos de Ciclo de vida** - `boot()`, `booted()`
- **Relações** - belongs, has, morphs…
- **Métodos de acesso e alteração**
- **Outros métodos**

<a name="controller"></a>

# Controller

Controllers lidam com pedidos e respostas. Na verdade, eles são intermediários entre o banco de dados, as visualizações e a lógica de negócios. [Ler Mais](https://laravel.com/docs/controllers)

**Comando para Criar:** `php artisan make:controller UserController`

```php
class UserController extends Controller
{
  //
}
```

<a name="naming-1"></a>

## Nomenculatura

Singular com o sufixo "Controller" (`UserController`, `ProductController`, `CategoryController`...)

<a name="types"></a>

## Tipos

- **resource** - Contém métodos para todas as operações `CRUD` e também contém métodos que apresentam os modelos HTML como `grid`, `create` e `edit` (tabela, criar e editar)
- **api** - Contém métodos para todas as operações `CRUD`, mas não contém métodos que apresentem modelos HTML
- **invokable** - controladores para ações únicas que não correspondam a recursos

<a name="best-practices-1"></a>

## Boas práticas

- Manter os métodos controladores limpos.
  - Não devem conter lógica de negócio complexa.
  - São principalmente resposáveis por uma coisa - retornar uma resposta.
- Para operações `CRUD` deve-se usar [resource controllers](https://laravel.com/docs/controllers#resource-controllers)

<a name="namespacing"></a>

## Namespacing

Quando temos diferentes tipos de controladores (para `API`, para Admin...), deve-se agrupar da seguinte forma:

```php
Admin/UserController.php
Api/Admin/UserController.php
Api/UserController.php
UserController.php
```
