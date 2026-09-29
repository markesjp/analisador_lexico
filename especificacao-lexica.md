# Especificação léxica da linguagem Lumen

## 1. Objetivo e decisões globais

Lumen é uma linguagem didática para programas com variáveis dos tipos `int`, `double`, `bool`, `char` e `string`, seleção `if/else`, repetição `while` e funções/procedimentos. Esta especificação é o contrato do scanner e define exatamente os tokens aceitos nesta etapa.

O alfabeto de entrada é ASCII imprimível (`U+0020`–`U+007E`) mais tabulação (`U+0009`), LF (`U+000A`) e CR (`U+000D`). Fora de strings e comentários, os caracteres reconhecidos são letras `A-Z` e `a-z`, dígitos `0-9`, sublinhado, espaços, aspas simples e duplas, barra invertida em escapes, os caracteres dos operadores `= ! < > + - * / % & |`, ponto e os delimitadores `(` `)` `{` `}` `[` `]` `,` `;` `:`. Outros caracteres geram erro léxico. A linguagem é case-sensitive: `int`, `Int` e `INT` são lexemas diferentes, e apenas o primeiro é palavra reservada.

## 2. Tokens

| Categoria | Notação | Exemplos válidos | Decisão da linguagem |
|---|---|---|---|
| Identificador | `[A-Za-z_][A-Za-z0-9_]*` | `total`, `_aux`, `contaItens2` | Sublinhado é permitido no início e no corpo. Não há tamanho máximo imposto pelo scanner. |
| Palavra reservada | Identificador pertencente à lista fechada abaixo | `int`, `while`, `return` | A comparação é case-sensitive. O scanner classifica o lexema como `KEYWORD`. |
| String | `" (caractere permitido ou escape válido)* "` | `"ok"`, `"linha\n2"`, `"ele disse \"oi\""` | Strings são delimitadas por aspas duplas, não atravessam linha e aceitam `\"`, `\\`, `\n`, `\r` e `\t`. String sem fechamento até fim da linha ou EOF é erro. |
| Caractere | `' (caractere ASCII imprimível ou escape válido) '` | `'a'`, `'\n'` | Exatamente um caractere ou escape. Aceita `\'`, `\"`, `\\`, `\n`, `\r` e `\t`. |
| Operador | Um ou dois caracteres da tabela de operadores | `=`, `==`, `<=`, `!=`, `&&` | O scanner aplica maximal munch: tenta a forma composta antes da simples. `&` e `|` isolados são inválidos. |
| Literal numérico | `[0-9]+(\.[0-9]+)?` | `0`, `10`, `3.14` | Inteiro e decimal são tokens distintos. Notação científica, hexadecimal e sinal no literal não fazem parte desta etapa. |
| Delimitador | Um caractere de `(){}[],;:` | `(`, `{`, `;`, `:` | Delimitadores são reconhecidos individualmente. |

### Palavras reservadas

```text
int double bool char string void
if else while
fn return
true false
```

`true` e `false` permanecem classificados como `KEYWORD`, pois pertencem à tabela fechada de palavras reservadas desta versão.

### Operadores

| Forma | Lexemas |
|---|---|
| Com dois caracteres | `==`, `!=`, `<=`, `>=`, `&&`, `||` |
| Com um caractere | `=`, `!`, `<`, `>`, `+`, `-`, `*`, `/`, `%` |

O ponto só é aceito como parte de um literal decimal com pelo menos um dígito antes e depois dele. Portanto, `3.14` é um literal; `3.` produz o literal `3` e depois um erro para `.`.

## 3. Espaços, comentários e EOF

Espaço, tabulação, `\n` e `\r` fora de tokens são ignorados. Comentários de linha começam com `//` e terminam antes de `\n` ou EOF. Comentários de bloco começam com `/*` e terminam no primeiro `*/`; não são aninhados. Comentário de bloco não fechado até EOF é erro léxico.

Comentários não geram tokens. O scanner conserva linha e coluna do início de cada token. A posição começa em linha 1, coluna 1; `\n` incrementa a linha e reinicia a coluna em 1, e `\r\n` conta como uma única quebra de linha.

## 4. Recuperação de erros

Caractere fora do alfabeto, escape desconhecido, string ou caractere não fechado até fim de linha/EOF, `&` isolado, `|` isolado e comentário de bloco não fechado são erros léxicos. Cada erro registra mensagem, linha e coluna e o scanner continua procurando o próximo token quando há texto restante. Exceções não tratadas não são usadas para sinalizar erros esperados.

## 5. Correspondência com o AFD

Os desenhos em `automatos/` usam os mesmos nomes de estados empregados no código:

- identificadores e palavras reservadas: `START`, `ID_BODY`, `ACCEPT_IDENTIFIER`;
- strings: `STRING_BODY`, `ESCAPE`, `ACCEPT_STRING`.

O código também implementa estados equivalentes para números, operadores e comentários, descritos nos comentários dos métodos de `Scanner.java`. A tabela e os desenhos devem ser atualizados junto com o código se o grupo mudar a linguagem.
