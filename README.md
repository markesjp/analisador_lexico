# Analisador léxico

Implementação do primeiro checkpoint de Construção de Compiladores: especificação léxica e analisador léxico manual em Java para a linguagem didática Lumen.

## Autores

- João Pedro de Oliveira Marques
- Victor Hugo Borges

Repositório: [markesjp/analisador_lexico](https://github.com/markesjp/analisador_lexico).

## Linguagem

A linguagem Lumen é case-sensitive e usa alfabeto ASCII. Identificadores, palavras reservadas, strings, caracteres, números, operadores, delimitadores e comentários estão definidos em [especificacao-lexica.md](especificacao-lexica.md).

## Estrutura

```text
analisador_lexico/
├── especificacao-lexica.md
├── automatos/
│   ├── afd-identificador.svg
│   ├── afd-string.svg
│   └── README.md
├── src/scanner/
│   ├── ErrorLexico.java
│   ├── Main.java
│   ├── Scanner.java
│   ├── TipoToken.java
│   └── Token.java
├── test/ScannerTest.java
├── test/evidencia-execucao.txt
└── examples/programa.lumen
```

## Requisitos

- JDK 17 ou superior.
- PowerShell, Bash ou outro terminal capaz de executar `javac` e `java`.

## Compilar e testar

No diretório raiz do projeto:

```bash
mkdir -p out
javac -d out src/scanner/*.java test/ScannerTest.java
java -cp out ScannerTest
```

No PowerShell, o equivalente é:

```powershell
New-Item -ItemType Directory -Force out | Out-Null
javac -d out src/scanner/*.java test/ScannerTest.java
java -cp out ScannerTest
```

## Executar o scanner

```bash
java -cp out scanner.Main examples/programa.lumen
```

Cada token é impresso como `TIPO('lexema') @ linha:coluna`. Erros léxicos são impressos em `linha:coluna`, e o scanner continua após erros recuperáveis.

## Decisões importantes

- O scanner é escrito à mão; não usa JFlex, regex de biblioteca para reconhecer tokens nem outro gerador.
- A regra de desambiguação é maximal munch.
- Strings não podem atravessar uma quebra de linha e aceitam `\"`, `\\`, `\n`, `\r` e `\t`.
- Literais `char` têm exatamente um caractere ASCII imprimível ou um escape permitido.
- Comentários de bloco não são aninhados. Um comentário não fechado até EOF é erro léxico.
- A implementação mantém a lista de erros para que os testes possam verificar posição e recuperação.

Os desenhos em [automatos/](automatos/) documentam os estados de identificadores e strings. A saída da validação está em [test/evidencia-execucao.txt](test/evidencia-execucao.txt).

O relatório explicativo é entregue separadamente e não integra os arquivos atuais deste repositório.
