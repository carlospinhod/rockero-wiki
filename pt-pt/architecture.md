# Arquitetura

**Antes de mais:** `Alguns termos estarão em inglês, pois como programadores de lingua portuguesa, devemos sempre escrever os nomes das variáveis, classes... em inglês e existem termos que simplesmente não tem tradução.`

- [Model](#model)
  - [Nomenculatura](#naming)
  - [Boas Práticas](#best-practices)
  - [Estrutura](#structure)
- [Controller](#controller)
  - [Nomenculatura](#naming-1)
  - [Types](#types)
  - [Boas Práticas](#best-practices-1)
  - [Namespacing](#namespacing)
- [Request](#request)
  - [Nomenculatura](#naming-2)
- [Configuration](#configuration)
  - [Boas Práticas](#best-practices-2)
- [Action](#action)
  - [Nomenculatura](#naming-3)
  - [Boas Práticas](#best-practices-3)
- [Support](#support)
  - [Nomenculatura](#naming-4)
- [Routing](#routing)
  - [Route Types](#route-types)
  - [Boas Práticas](#best-practices-4)
- [Middleware](#middleware)
  - [Exemplo prático](#usage-example)
- [Observer](#observer)
  - [Nomenculatura](#naming-5)
  - [Boas Práticas](#usage-example-1)
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

<a name="request"></a>

# Request

Requests são objetos encapsulam os dados de input de um pedido HTTP.

Fornecendo uma forma conveniente de validar e processar os dados inseridos antes de os utilizar na aplicação. [Ler Mais](https://laravel.com/docs/validation#form-request-validation)

**Comando para Criar::** `php artisan make:request StoreUserRequest`

```php
class StoreUserRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     */
    public function rules()
    {
        return [
          //
        ];
    }
}
```

<a name="naming-2"></a>

## Nomenculatura

Nome do método no singular, nome do Model e o sufixo "Request" (`StoreUserRequest`, `StoreProductRequest`, `UpdateCategoryRequest`...)

<a name="configuration"></a>

# Configurations

As configurações são sempre guardadas na diretoria `config`.

> **Warning:** Nunca se deve aceder ao método `env()` diretamente na aplicação. Deve-se usar apenas nos ficheiros de configuração com o método `config()`.

<a name="best-practices-2"></a>

## Boas Práticas

- As definições da aplicação personalizadas devem ser guardadas em `project.php`.
- Chaves de API de serviços externos devem ser guardada em `services.php`.

<a name="action"></a>

# Action

Ações são classes responsáveis por apenas uma tarefa. O código deve ser limpo e simples.

**Comando para Criar:** `php artisan make:action VerifyUserAction`

```php
class VerifyUserAction
{
    /**
     * Run the action.
     */
    public function run(): void
    {
        //
    }
}
```

> **Dica:** Este comando não faz parte da framework Laravel, instale a nossa package `rockero-cz/laravel-starter-kit` para obter um pouco de magia.

<a name="naming-3"></a>

## Nomenculatura

Singular com o sufixo "Action" (`VerifyUserAction`, `CreateProductAction`, `ReorderCategoryAction`...)

<a name="best-practices-3"></a>

## Boas Práticas

- Ações devem conter apenas um método público com o nome `run()`.
- Métodos de ajuda de uma única ação devem ser privados ou protegidos.
  - Vários métodos de ajuda devem ser convertidos em classes `Support` classes.

<a name="support"></a>

# Support

Classes de Suporte são um forma de agrupar funções de lógica relacionadas numa classe. Permitem a reutilização simples do código e ajuda a organizar o código da aplicação.

Tipicamente elas são usadas para disponibilizar funcionalidades que não estão relacionadas a apenas um Model ou Controller, e são utilizadas em várias partes da aplicação.

They are typically used to provide functionality that is not specific to a single model or controller, but rather is used across multiple parts of an application.

It is simpler alternative than using services.

**Comando para Criar:** `php artisan make:class Support/Cart`

```php
class Cart
{
  //
}
```

> **Tip:** Este comando não faz parte da framework Laravel, instale a nossa package `rockero-cz/laravel-starter-kit` para obter um pouco de magia.

<a name="naming-4"></a>

## Nomenculatura

Singular sem o sufixo "Support" (`Cart`, `OpeningHours`, `Table`...)

<a name="routing"></a>

# Routing

<a name="route-types"></a>

## Route Types (Tipos de Rotas)

- **web** - Rotas que gerem pedidos e repostas HTTP via web…
- **api** - Rotas que gerem pedidos e repostas via API…
- **channels** - Rotas que gerem canais de transmissão em tempo real que usam websockets…
- **console** - Rotas que gerem comandos que podem ser executados através da linha de comandos…

<a name="best-practices-4"></a>

## Boas Práticas

- Os URLs devem ser escritos no plural.
- Cada rota deve ter um nome.
- As rotas devem ser agrupadas por entidades.

```php
Route::middleware('auth')->group(function () {
  Route::name('users.')->group(function () {
    Route::get('/', UserIndex::class)->name('index');
    Route::get('/{user}', UserShow::class)->name('show');
  });
});
```

<a name="middleware"></a>

# Middleware

O Middleware é uma forma de filtrar e modificar pedidos HTTP recebidos na aplicação, que permitem realizar varias tarefas como autenticação, autorização e gestão de sessões.
Deve-se utilizar um middleware em casos em que seja preciso lógica espicifica para um grupo de rotas. [Ler Mais](https://laravel.com/docs/middleware)

**Comando para Criar:** `php artisan make:middleware HandleLocale`

```php
class HandleLocale
{
  /**
   * Handle an incoming request.
   */
  public function handle(Request $request, Closure $next): Response
  {
    return $next($request);
  }
}
```

<a name="usage-example"></a>

## Exemplo de uso

```php
Route::prefix('/admin')->name('admin.')->middleware(HandleLocale::class)->group(function () {
  Route::resource('users', UserController::class)->name('users');
  Route::resource('orders', OrderController::class)->name('orders');
});
```

<a name="observer"></a>

# Observer

Classes de observação são usadas para "ouvir" eventos especificos que ocorrem nos models tais como `created`, `updated`, ou `deleted` entre outros...

Ao utilizarmos classes de observação, podemos manter a classe do model limpa e focada no seu proposito evitando enche-la de lógica desnecessária. [Ler Mais](https://laravel.com/docs/eloquent#observers)

> **Avio:** Atualizações em massa do Eloquent não ativam as classes de observação. Isto acontece porque as classes model não são carregadas quando é feita uma atualização em massa, apenas uma Query SQL.

**Comando para Criar:** `php artisan make:observer UserObserver --model=User`

```php
class UserObserver
{
  /**
   * Handle the User "created" event.
   */
  public function created(User $user): void
  {
    //
  }
}
```

<a name="naming-5"></a>

## Nomenculatura

Nome do model no singular com o sufixo "Observer" (`UserObserver`, `ProductObserver`, `CategoryObserver`...)

<a name="usage-example-1"></a>

## Exemplo de uso

```php
// Recalculate invoice when the invoice item is saved.
public function saved(InvoiceItem $invoiceItem): void
{
  $invoiceItem->invoice()->recalculate();
}

// Set default state when the order is created.
public function creating(Order $order): void
{
  $order->state = OrderState::NEW;
}

// Delete relations before the model is deleted.
public function deleting(Order $order): void
{
  $order->products()->delete();
}
```

<a name="event"></a>

# Event

Classes de Evento são uma forma de ativar e manipular ações que ocorrem durante a execução da aplicação. São usadas em combinação com listeners.

When an event is dispatched, Laravel will notify all registered listeners for that event, giving them a chance to perform any necessary actions. [Read more](https://laravel.com/docs/events)

> We primarily use actions instead of events because they are more simple and understandable. However, we use still events for some notifications or in combination with [websockets](https://laravel.com/docs/broadcasting).

**Create command:** `php artisan make:event OrderCreated`

```php
class OrderCreated
{
    use Dispatchable, InteractsWithSockets, SerializesModels;

    /**
     * Create a new event instance.
     */
    public function __construct()
    {
        //
    }

    /**
     * Get the channels the event should broadcast on.
     */
    public function broadcastOn(): Channel|array
    {
        return new PrivateChannel('channel-name');
    }
}
```

<a name="dispatching-events"></a>

## Dispatching events

```php
OrderCreated::dispatch();

// or

event(new OrderCreated());
```

<a name="listeners"></a>

## Listeners

**Create command:** `php artisan make:listener SendOrderCreatedNotification --event=OrderCreated`

```php
class SendOrderCreatedNotification
{
    /**
     * Create the event listener.
     */
    public function __construct()
    {
        //
    }

    /**
     * Handle the event.
     */
    public function handle(OrderCreated $event): void
    {
        //
    }
}
```

<a name="command"></a>

# Command

Commands are a powerful tool for automating some tasks in your application. Laravel also provides a set of built-in commands that you can use out of the box.

With commands, you can easily perform repetitive actions like running tests or executing database migrations, with minimal effort. [Read more](https://laravel.com/docs/artisan#writing-commands)

**Create command:** `php artisan make:command FetchUsers`

```php
class FetchUsers extends Command
{
    protected $signature = 'command:name';
    protected $description = 'Command description';

    /**
     * Execute the console command.
     */
    public function handle(): int
    {
        return Command::SUCCESS;
    }
}
```

<a name="scheduling-commands"></a>

## Scheduling commands

You can easily run commands at specified intervals or times using [Scheduler](https://laravel.com/docs/scheduling).

```php
// app/Console/Kernel.php
protected function schedule(Schedule $schedule)
{
    $schedule->command('telescope:prune')->daily();
}
```
