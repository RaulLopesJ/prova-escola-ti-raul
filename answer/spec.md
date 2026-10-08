### UC1 — Validação do campo `placa` — regra proposta

O campo `placa` DEVE ser obrigatório e possuir tipo string.

Seu valor DEVE conter exatamente sete caracteres, obedecendo à seguinte sequência:

- Três letras maiúsculas de `A` a `Z`.
- Um dígito de `0` a `9`.
- Uma letra maiúscula de `A` a `Z`.
- Dois dígitos de `0` a `9`.

A validação DEVE verificar o comprimento e a correspondência integral com a expressão `^[A-Z]{3}[0-9][A-Z][0-9]{2}$`.

Valores ausentes, nulos, de outro tipo ou fora do formato DEVEM retornar HTTP **400 Bad Request**, com body `{"erro":"placa_invalida"}`. Nenhum bilhete DEVE ser criado nesse caso.

A aplicação NÃO DEVE remover espaços nem converter letras minúsculas automaticamente.

**Critérios de aceite:**

1. Dada uma requisição válida com `placa` igual a `ABC1D23`, quando processada, então a API retorna **201** com o bilhete criado.
2. Dada uma placa como `ABC1234`, `abc1d23` ou `ABC-1D23`, quando processada, então a API retorna **400** com `{"erro":"placa_invalida"}`.
3. Dado o campo `placa` ausente ou com valor `null`, quando processado, então a API retorna **400** com `{"erro":"placa_invalida"}`.
4. Dada uma placa válida e uma `entrada` com formato inválido, quando processadas, então a API retorna **422** com `{"erro":"entrada_invalida"}`.

### UC2 — Encerrar bilhete: regras de cobrança

**Operação:** `POST /bilhetes/{id}/encerramento`  
**Resposta de sucesso:** HTTP **200**, contendo `id`, `placa`, `entrada`, `saida`, `minutos` e `valor_centavos`.

**Parâmetros obrigatórios desta variante:**

- `TARIFA_HORA_CENTAVOS`: **450**.
- `FRACAO_MINUTOS`: **15**.
- `TOLERANCIA_MINUTOS`: **10**.
- `TETO_DIARIO_CENTAVOS`: **6000**.

**Fluxo de cálculo:**

1. Determinar a duração total do bilhete pela diferença entre `saida` e `entrada`. O cálculo DEVE preservar os segundos e milissegundos disponíveis, evitando truncamentos antecipados.

2. Para uma duração válida de até **10 minutos, inclusive**, definir `valor_centavos` como **0**.

3. Ultrapassados os 10 minutos, calcular a quantidade de frações sobre **todo o período desde a entrada**. A tolerância funciona como condição de gratuidade; ela não é descontada da duração cobrada.

4. Calcular a quantidade de frações de 15 minutos, arredondando para cima:

   `frações = teto(duração_total_em_milissegundos ÷ 900000)`

5. Cada fração corresponde exatamente a **112,5 centavos**, pois:

   `450 × 15 ÷ 60 = 112,5`

   Esse valor DEVE permanecer exato nos cálculos intermediários.

6. Se a quantidade calculada atingir **54 frações ou mais**, definir `valor_centavos` como **6000**, respeitando o teto por bilhete.

7. Para quantidades inferiores a 54 frações, arredondar o total uma única vez para centavos inteiros, levando meio centavo para cima. Nesta variante, o cálculo pode ser expresso por:

   `valor_centavos = piso((225 × frações + 1) ÷ 2)`

8. O resultado DEVE ser um inteiro entre **0 e 6000**, inclusive.

**Critérios de aceite, considerando o arredondamento monetário acima:**

| Duração total | Frações cobradas | `valor_centavos` |
|---|---:|---:|
| 10 minutos | 0 | 0 |
| 11 minutos | 1 | 113 |
| 15 minutos | 1 | 113 |
| 16 minutos | 2 | 225 |
| 30 minutos | 2 | 225 |
| 60 minutos | 4 | 450 |
| 95 minutos | 7 | 788 |
| 795 minutos | 53 | 5963 |
| 796 minutos | 54 | 6000 |

O teto DEVE limitar o valor total do bilhete, inclusive quando sua duração atravessar mais de uma data.