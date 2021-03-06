# Cadastrando uma disciplina

> **\[info\] Objetivos do capítulo:**
>
> * demonstrar o uso de formulários e _two-way data binding_
> * implementar o requisito "Cadastrar disciplina"
>
> **Branches**: [livro-cadastrando-disciplina](https://github.com/jacksongomesbr/angular-escola/tree/livro-cadastrando-disciplina)

Neste capítulo vamos começar pelo final: primeiro, veja a figura que ilustra como o software estará ao final do capítulo.

![Tela do software apresentando a lista das disciplinas e o formulário de cadastro](/assets/software-cadastrando-disciplina-browser.png)

A tela do aplicativo tem duas funções: na primeira parte, apresenta a lista das disciplinas; na segunda parte, apresenta um formulário que permite o cadastro de uma disciplina. A primeira parte você já conhece do capítulo anterior. Agora, veremos os detalhes para o formulário de cadastro.

Antes de prosseguir, um fator importante: o projeto original criado pelo Angular CLI não suporta formulários. Isso mesmo que você leu. O Angular trata formulários de três formas diferentes, uma delas, a que vamos utilizar, chamada de **template driven forms** \(algo como formuários orientados a templates\).

Para habilitar o suporte a formulários é necessário importar o módulo `FormsModule` no **root module**:

```typescript
import {BrowserModule} from '@angular/platform-browser';
import {NgModule} from '@angular/core';
import {FormsModule} from '@angular/forms';
import {AppComponent} from './app.component';

@NgModule({
  ...
  imports: [
    BrowserModule,
    FormsModule
  ],
  ...
})
export class AppModule {
}
```

O módulo `FormsModule` está definido no pacote `@angular/forms` e é importado no `AppModule` por meio do atributo `imports` dos metadados da classe. A partir de então a utilização de formulários do tipo **template-driven forms** está garantida.

O formulário de cadastro tem dois campos: nome e descrição. O comportamento esperado pelo usuário é preencher os valores dos campos, clicar no botão "Salvar" e ver a lista de disciplinas atualizada, contendo a disciplina que acabou de ser cadastrada.

Para fazer isso, primeiro vamos ao Controller do `AppComponent`:

```typescript
...
export class AppComponent {
  selecionado = null;
  nome = null;
  descricao = null;
  disciplinas = [
    new Disciplina('Língua Portuguesa', 'O objetivo norteador da BNCC de ...'),
      ...
  ];

  salvar() {
    const d = new Disciplina(this.nome, this.descricao);
    this.disciplinas.push(d);
    this.nome = null;
    this.descricao = null;
  }
}
```

São adicionados dois atributos e um método:

* `nome`: está ligado ao campo nome do formulário por meio de **data binding**
* `descricao`: está ligado ao campo descricao do formulário por meio de **data binding**
* `salvar()`: é executado quando o usuário clicar no botão "Salvar"

Os dois atributos são inciados com `null`. O método `salvar()` cria uma instância da classe `Disciplina` usando os atributos `nome` e `descrição`, respectivamente. Posteriormente, adiciona esse objeto ao vetor `disciplinas` por meio do método `push()`. Por fim, atribui o valor `null` novamente aos atributos `nome` e `descricao`.

Mas... o que significa um atributo do Controller estar ligado a um campo do formulário via **data binding**? Você já sabe que data binding é o recurso do Angular que faz com que o valor de um atributo do Controller seja apresentado no Template por meio de interpolação. E quanto ao formulário? Vamos ao Template:

```html
<h1>Disciplinas</h1>
...
<h2>Cadastrar</h2>
<p>Use este formulário para cadastrar uma disciplina.</p>
<form #cadastro="ngForm" (submit)="salvar()">
    <div>
        <label for="nome">Nome</label><br>
        <input type="text" id="nome" name="nome" [(ngModel)]="nome" required>
    </div>
    <div>
        <label for="descricao">Descrição</label><br>
        <textarea id="descricao" name="descricao" [(ngModel)]="descricao"
                  cols="70" rows="5">
        </textarea>
    </div>
    <button type="submit" class="btn btn-primary" [disabled]="!cadastro.valid">
        Salvar
    </button>
</form>
```

Como a parte da lista das disciplinas não muda muito, vamos omitir essa parte e apresentar o formulário, começando pela tag `<form>`. Em termos de HTML, a tag possui um atributo `#cadastro`, com valor `ngForm`. Esse recurso, na verdade, é uma sintaxe do Angular para criar uma variável \(chamada `cadastro`\) vinculada ao formulário. Importante também destacar o evento `submit`. Você já entende a sintaxe `(submit)`, então o tratador do evento é uma chamada ao método `salvar()`.

Na sequência são definidos os campos do formulário. A parte mais importante está nas `tags` input e `textarea`, principalmente o que, em termos de HTML, parece um atributo: `[(ngModel)]`. Você já sabe a sintaxe para eventos \(entre parênteses\) e já viu que o Angular consegue alterar um atributo do HTML por meio de colchetes. Essa sintaxe, que mistura as duas anteriores, é a sintaxe do recurso **two-way data binding**. No campo para o nome, `[(ngModel)]` tem o valor `nome`, para o campo descrição, tem o valor `descricao`. Ambos são atributos do Controller. Assim, o **two-way data binding** significa que o valor do atributo pode ser modificado por meio de uma alteração do campo de formulário correspondente, e vice-versa:

* se o valor do atributo `nome` for modificado no Controller, o Template será atualizado, mostrando esse valor no `input`
* se o valor do atributo `nome` for modificado no Template, por uma alteração no `input`, esse valor estará disponível no Controller

Por isso o **two-way: duas vias**. A figura a seguir ilustra esse processo.

![Ilustração do two-way data binding, ligando formulário e Controller](/assets/formularios-two-way-data-binding.png)

O formulário tem mais uma característica: a entrada do usuário está passando por validação. O elemento `input` para o campo nome tem o atributo `required`, que o torna de preenchimento obrigatório. A partir de então há um comportamento muito interessante para o botão "Salvar": o elemento button possui um atributo `disabled`, que é modificado conforme a expressão `!cadastro.valid`. Lembra da variável de template `#cadastro`? Aqui ela é usada como uma referência ao formulário. Portanto, se o formulário de cadastro estiver válido, o botão "Salvar" estará habilitado para clique. Caso contrário, estará desabilitado.

Execute o software no browser para notar o comportamento citado no capítulo.

> **\[info\] Resumo do capítulo:**
>
> * Implementamos o requisito "Cadastrar uma disciplina"
> * Utilizamos o recurso two-way data binding
> * Utilizamos formulário para receber entrada do usuário



