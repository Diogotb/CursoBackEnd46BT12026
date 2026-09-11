# Resolução das Atividades do Bloco A Exercícios Teóricos

## **Diferença Estrutural:** Explique a diferença física entre onde os dados são anexados em uma requisição `GET` e em uma requisição `POST`.

R.  A diferença física está no local da mensagem HTTP onde os dados trafegam. Na requisição *GET*, os dados são anexados diretamente na URL chamada também de Query String.Na requisição *POST*, a URL permanece limpa e os dados são enviados escondidos dentro do corpo (body) da requisição HTTP.

## **Segurança e Privacidade:** Por que senhas de usuário nunca devem ser enviadas via método `GET`? Cite pelo menos dois locais onde essa senha ficaria gravada de forma insegura.

**Resposta:** Senhas de usuário nunca devem ser enviadas pelo método GET porque os dados são colocados diretamente na URL, podendo ficar expostos e registrados em diferentes locais. A senha pode ser armazenada no histórico do navegador e também nos logs de acesso do servidor web ou de proxies. Além disso, URLs podem ser compartilhadas ou armazenadas por outros sistemas. A boa prática é utilizar o método POST junto com HTTPS para proteger os dados durante a transmissão.

## **Coalescência Nula:** Por que a instrução `$nome = $_POST['nome'];` dispara um `Warning` na primeira vez que a página é carregada no navegador? Como o operador `??` resolve isso?

3. Isso acontece porque a chave $_POST['nome'] ainda não existe quando a página carrega pela primeira vez. Por conta disso, o operador ?? verifica se o valor da variável é null, caso seja, ele atribui um valor vazio("").

## **Idempotência:** O que significa dizer que uma requisição `GET` é idempotente? Por que atualizar ou deletar dados no banco usando links `GET` é uma má prática de segurança?

Dizer que uma requisição **GET é idempotente** significa que realizar a mesma requisição várias vezes não deve causar alterações no estado dos dados.

GET deve ser utilizado principalmente para **consultar informações**.

Utilizar links GET para atualizar ou deletar dados de um banco de dados é uma má prática porque uma simples visita ou atualização da página poderia executar uma ação que altera os dados.

Por exemplo:

```text
produto.php?acao=deletar&id=10
```

Um link desse tipo poderia causar uma exclusão apenas ao ser acessado.

## **Validação Client vs Server:** Um desenvolvedor júnior afirma que o formulário dele é 100% seguro porque colocou `required` e `type="email"` em todas as tags HTML. Explique por que essa afirmação é falsa.

Essa afirmação é falsa porque validações feitas em HTML (required, type="email", etc.) acontecem apenas no Front-End. Essas validações podem ser facilmente ignoradas, o usuário pode desabilitar o JavaScript ou editar o HTML. Por isso, a validação no *BackEnd* é obrigatória, é a única parte que realmente garante a integridade dos dados, pois roda no servidor e não pode ser burlada pelo usuário.

##  6. **XSS e Sanitização:** Qual é o risco de exibir dados vindos de um `$_POST` diretamente na tela sem utilizar `htmlspecialchars()`?

**Resposta:** O principal risco é um ataque de XSS (Cross-Site Scripting). Nesse tipo de ataque, um usuário mal-intencionado pode inserir código HTML ou JavaScript em um formulário. Se esse conteúdo for exibido diretamente na página, o navegador pode interpretar o código como parte da página e executá-lo.

Isso pode permitir ações maliciosas, como redirecionamentos, alteração do conteúdo da página ou tentativa de acesso a informações da sessão do usuário. A função `htmlspecialchars()` ajuda a evitar esse problema ao converter caracteres especiais, como `<` e `>`, em entidades HTML, fazendo com que o conteúdo seja tratado como texto em vez de código.

## 7. **Sticky Forms:** O que é a técnica de *Sticky Forms* e qual é o seu impacto na experiência do usuário (UX)?

7. Sticky Forms é a técnica de manter os dados previamente digitados nos campos do formulário caso a página seja recarregada devido a um erro de validação. Isso melhora a experiência do usuário, pois evita que ele tenha o trabalho de preencher todo o formulário denovo do zero.

## **DevTools:** Como você utilizaria a aba *Network* do navegador para comprovar que um formulário foi enviado via `POST` e não via `GET`?

8. DevTools (Network) — Abrir F12 → Network → marcar "Preserve log" → enviar o formulário → localizar a requisição → conferir o campo "Method" (deve mostrar POST) e verificar que os dados aparecem em "Form Data", não na URL.

