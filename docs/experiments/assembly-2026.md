# Assembly 2026

Esse documento descreve o funcionamento de um processador fictício sem nome e o conjunto de instruções suportado. Ele dispõe de controle de fluxo e 16 registradores para dados, além de uma pilha.

Clonar: `git clone https://github.com/andrewnationdev/assembly_2026.git`
Executar: `ruby asm.rb [nome_do_arquivo_codigo_fonte]`

Requisitos: Ruby

---

## 1. Arquitetura e Componentes

### Registradores
A máquina possui 16 registradores de uso geral disponíveis:
* `r1`, `r2`, `r3`, `r4`, `r5`, `r6`, etc.
* Armazenam valores inteiros durante a execução.

### Flags de Estado
* **`$zero_flag`**: Booleano (`true`/`false`) atualizado pelas instruções de comparação (`CMP`), utilizado pelos desvios condicionais.

### Pilha (Stack)
* Uma pilha LIFO global (`$stack`) que permite salvar e recuperar o estado dos registradores dinamicamente usando `PUSH` e `POP`.

---

## 2. Conjunto de Instruções (Instruction Set)

### Manipulação de Dados
* **`MOV reg valor_ou_reg`**
  Move um valor direto ou o valor contido em outro registrador para o registrador de destino.

### Operações Aritméticas
* **`ADD reg valor_ou_reg`**
  Soma o valor ao registrador e guarda o resultado nele.
* **`SUB reg valor_ou_reg`**
  Subtrai o valor do registrador.
* **`MUL reg valor_ou_reg`**
  Multiplica o registrador pelo valor especificado.
* **`DIV reg valor_ou_reg`**
  Divide o valor do registrador pelo operando.
* **`INC reg`**
  Incrementa o registrador em `1`.
* **`DEC reg`**
  Decrementa o registrador em `1`.

### Gerenciamento de Pilha
* **`PUSH reg`**
  Empilha o valor atual do registrador no topo da pilha.
* **`POP reg`**
  Desempilha o último valor da pilha e armazena no registrador.

### Controle de Fluxo e Comparações
* **`CMP reg valor_ou_reg`**
  Subtrai o segundo operando do primeiro internamente. Se o resultado for `0`, ativa a `$zero_flag` (`true`); caso contrário, desativa (`false`).
* **`JMP linha`**
  Salta incondicionalmente para o número da linha especificada.
* **`JNZ linha`**
  Salta para a linha se a `$zero_flag` for falsa (`false` / valores diferentes).
* **`JEQ linha`**
  Salta para a linha se a `$zero_flag` for verdadeira (`true` / valores iguais).
* **`JNE linha`**
  Sinônimo funcional para saltar quando não há igualdade.

### E/S e Execução
* **`CALL 10`**
  Rotina de sistema atual usada para imprimir no console o valor contido em `r1` (ou a saída configurada).
* **`HLT`**
  Encerra a execução do programa imediatamente (`exit`).

---

## 3. Exemplo de Programa: Fatorial de 5

```assembly
; ==========================================
; Programa: Cálculo de Fatorial de 5
; Resultado esperado: 120
; ==========================================

MOV r1 5
MOV r2 1
PUSH r1
MUL r2 r1
POP r3
DEC r1
CMP r1 1
JNE 3
MOV r1 r2
CALL 10
HLT
```

Observação: Instruções são sempre em maiúsculas
