## Princípios fundamentais

### I. Plataforma e framework

A API DEVE ser desenvolvida para execução em Node.js, utilizando o framework Express para definir as rotas HTTP e processar as requisições.

As versões adotadas do Node.js e do Express DEVEM ser documentadas no `plan.md` e refletidas na configuração do projeto.

### II. Dependências e execução

A aplicação DEVE conter um arquivo `package.json` na sua raiz, declarando todas as dependências externas necessárias à execução, incluindo o Express.

A execução de `npm install`, na raiz da aplicação, DEVE instalar todas as dependências declaradas, sem exigir a instalação global de pacotes adicionais.

O `package.json` DEVE disponibilizar os seguintes scripts:

- `start`: iniciar a API na porta determinada pela variante da prova.
- `test`: executar a suíte de testes por meio de `node --test`.

### III. Precisão monetária

Valores monetários DEVEM utilizar representação decimal de ponto fixo, com unidade e escala explicitamente definidas.

Cálculos monetários NÃO DEVEM utilizar ponto flutuante binário para representar frações monetárias.

Os cálculos intermediários DEVEM preservar a precisão necessária até a aplicação da regra de arredondamento monetário. A escala do resultado final, o modo de arredondamento e o momento de sua aplicação DEVEM estar definidos no `spec.md`.

A estratégia utilizada para implementar essa representação DEVE ser documentada no `plan.md`.

### IV. Testes automatizados

Todos os testes automatizados da aplicação DEVEM ser concentrados em um único arquivo: `tests/api.test.js`.

O arquivo DEVE utilizar o módulo nativo `node:test` para definir os testes e `node:assert/strict` para realizar as asserções.

O script `test` do `package.json` DEVE executar o comando `node --test tests/api.test.js`, permitindo executar toda a suíte por meio de `npm test`, na raiz da aplicação.

A suíte DEVE conter casos individuais para verificar:

- Abertura e encerramento de bilhetes.
- Listagem de bilhetes ativos.
- Emissão do relatório diário.
- Regras de tolerância, frações de cobrança, teto e arredondamento monetário.
- Validações e respostas de erro definidas no contrato da API.

Os testes DEVEM isolar os dados utilizados e controlar os horários dos cenários, garantindo resultados determinísticos e independentes da ordem de execução.

Os testes NÃO DEVEM exigir a instalação de um executor externo ao Node.js.