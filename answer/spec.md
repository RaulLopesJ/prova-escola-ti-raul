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
