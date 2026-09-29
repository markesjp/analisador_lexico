# AFDs do scanner

Os arquivos SVG são desenhos dos autômatos usados para as duas categorias que o enunciado exige explicitamente.

## Identificador e palavra reservada

Estados:

- `START`: início; exige letra ou `_`.
- `ID_BODY`: consome letras, dígitos e `_`.
- `ACCEPT_IDENTIFIER`: estado final. A tabela de palavras reservadas é consultada depois da aceitação; o mesmo lexema pode virar `KEYWORD`.

O scanner implementa esse fluxo em `Scanner.scanIdentifierOrKeyword()` usando os mesmos nomes nos comentários do código.

## String

Estados:

- `STRING_BODY`: consome caracteres comuns até aspas, escape, quebra de linha ou EOF.
- `ESCAPE`: verifica se o caractere após `\\` pertence à lista de escapes válidos.
- `ACCEPT_STRING`: estado final ao consumir a aspas de fechamento.

O scanner implementa esse fluxo em `Scanner.scanString()`.
