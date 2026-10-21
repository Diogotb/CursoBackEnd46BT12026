## 9. LISTA DE EXERCÍCIOS DE FIXAÇÃO E PRÁTICA

### Parte A: Questões Teóricas de Fixação

1. **Procedural vs. OO:** Quais são os três principais riscos técnicos de manter regras financeiras críticas implementadas puramente através de arrays associativos e scripts procedurais?

R. Os três principais riscos são a ausência de validação de tipos e chaves `(Type Safety)`, o espalhamento da regra de negócio com quebra do encapsulamento e a alta rigidez acompanhada de baixa testabilidade. Em arrays associativos, erros de digitação em chaves criam entradas nulas sem alertas de sintaxe, corrompendo cálculos. Além disso, qualquer trecho do código pode alterar os dados sem passar por validações financeiras, e qualquer mudança na estrutura do array exige refatorar manualmente todo o sistema.

2. **Ciclo de Vida do Objeto:** O que ocorre na memória Heap do servidor quando a instrução `new ContaCorrente(...)` é executada? Qual é o papel do método `__construct()` nesse processo?
R. Quando a instrução `new ContaCorrente(...)` é executada, o servidor aloca um bloco de memória na Heap para armazenar os atributos e métodos da instância, retornando um ponteiro/referência para a variável, e o método `__construct()` roda em seguida para inicializar o objeto com um estado válido.

3. **A Pseudo-Variável `$this`:** Explique por que o comando `$this->saldo` é obrigatório para acessar um atributo dentro da classe, diferenciando-o de uma variável local `$saldo`.
R. A pseudo-variável `$this` representa o objeto atual dentro de um método não estático. Ela permite acessar os atributos e métodos pertencentes àquela instância.

Já `$saldo` é uma variável local, que existe no escopo do método e não representa automaticamente o atributo do objeto.

Exemplo:

```php
<?php
declare(strict_types=1);

class ContaCorrente
{
    private float $saldo = 0.0;

    public function depositar(float $valor): void
    {
        $saldo = $valor; // Variável local.
        $this->saldo += $saldo; // Atributo do objeto.
    }
}
```

Nesse exemplo, `$saldo` recebe o valor localmente, enquanto `$this->saldo` representa o saldo armazenado na conta.

4. **Encapsulamento e Invariantes:** Por que um desenvolvedor sênior de back-end nunca deve criar um método `public function setSaldo(float $novoSaldo)` para uma conta bancária?

R. Porque esse método permitiria alterar o saldo diretamente, sem validar a origem do dinheiro ou registrar operações. O ideal é modificar o saldo por métodos controlados, como `depositar()`, `sacar()` e `transferir()`, preservando as regras de negócio.

5. **Modificadores de Acesso:** Qual é a diferença prática entre os modificadores `private` e `protected` quando subclasses entram em cena através do `extends`?
R.  **`private` e `protected`:** Uma propriedade `private` só pode ser acessada pela classe que a declarou. Uma propriedade `protected` também pode ser acessada pelas classes filhas. Nenhuma das duas fica diretamente acessível por código externo à classe e à sua hierarquia.

6. **Constructor Property Promotion:** Como o recurso de promoção de propriedades introduzido no PHP 8 reduz o *boilerplate* e melhora a legibilidade do código?
R. É um atalho visual moderno. Em vez de escrever a mesma variável três vezes na classe para conseguir inicializá-la, você escreve o atributo uma única vez direto no argumento do construtor.

7. **Propriedades `readonly`:** Explique o mecanismo de segurança proporcionado pela palavra-chave `readonly` em atributos de classes de valor como `Titular` ou `CPF`.
R.  A palavra-chave readonly impede que uma propriedade seja modificada novamente após sua inicialização permitida. Ela ajuda a proteger informações importantes, como os dados de identificação de um titular.


8. **Classes Abstratas:** Por que faz todo sentido no modelo bancário que a classe `ContaBancaria` seja declarada como `abstract` enquanto `ContaCorrente` e `ContaPoupanca` são classes concretas?
R. ContaBancaria é abstrata porque é apenas um conceito. Você não abre uma conta genérica no mundo real; você abre uma ContaCorrente ou ContaPoupanca (que são as classes reais e concretas).


9. **Polimorfismo:** Como o método `transferir(float $valor, ContaBancaria $destino)` consegue operar transferências entre quaisquer tipos de contas filhas sem precisar saber qual é a classe exata da conta de destino?
R.
O método abaixo aceita qualquer objeto que seja uma `ContaBancaria` ou uma subclasse dela:

```php
public function transferir(float $valor, ContaBancaria $destino): void {
    $this->sacar($valor);
    $destino->depositar($valor);
}
```

Ele não precisa saber se `$destino` é `ContaCorrente`, `ContaPoupanca` ou outra filha. O PHP resolve, em tempo de execução, qual versão de `depositar()` deve ser chamada — esse é o polimorfismo.

Assim, se cada tipo de conta tiver uma regra diferente de depósito, a transferência continua funcionando sem precisar de `if ($destino instanceof ContaCorrente)`.

10. **Exceções de Domínio:** Por que é uma péssima prática retornar `false` ou `null` quando uma operação de saque é rejeitada por falta de saldo?
R. 
Retornar `false` ou `null` pode dificultar a identificação da causa de um erro, como saldo insuficiente, conta bloqueada ou limite excedido.

Lançar uma exceção específica, como `SaldoInsuficienteException`, permite informar melhor o motivo da falha e interromper o fluxo normal da operação. Assim, o código que chama o método pode tratar o erro explicitamente usando `try` e `catch`.
