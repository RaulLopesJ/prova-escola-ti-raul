| Caso | Exemplo de envio | Resposta esperada |
|---|---|---|
| Caminho feliz | `{"placa":"ABC1D23"}` | 201; um bilhete aberto criado |
| Caminho feliz com instante explícito | `{"placa":"ABC1D23","entrada":"2026-10-07T12:00:00-03:00"}` | 201; preservar o instante informado |
| Ordem incorreta para a proposta Mercosul | `{"placa":"ABC1234"}` | 400; `placa_invalida` |
| Letras minúsculas | `{"placa":"abc1d23"}` | 400; `placa_invalida` |
| Comprimento inferior ao limite | `{"placa":"ABC1D2"}` | 400; `placa_invalida` |
| Comprimento superior ao limite | `{"placa":"ABC1D234"}` | 400; `placa_invalida` |
| String longa | Campo `placa` com 1000 caracteres `A` | 400; `placa_invalida`, com body dentro do limite HTTP |
| String vazia | `{"placa":""}` | 400; `placa_invalida` |
| Apenas espaços | `{"placa":"   "}` | 400; `placa_invalida` |
| Campo ausente | `{}` | 400; `placa_invalida` |
| Body ausente | POST sem corpo | 400; `placa_invalida` |
| Campo nulo | `{"placa":null}` | 400; `placa_invalida` |
| Tipo incorreto | `{"placa":1234567}` | 400; `placa_invalida` |
| Entrada vazia, com placa válida | `{"placa":"ABC1D23","entrada":""}` | 422; `entrada_invalida` |
| Entrada sem fuso, com placa válida | `{"placa":"ABC1D23","entrada":"2026-10-07T12:00:00"}` | 422; `entrada_invalida` |