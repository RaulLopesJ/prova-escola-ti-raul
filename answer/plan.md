Plano de implementação — Zona Azul Digital
Stack
- Node.js como ambiente de execução.
- Express para as rotas HTTP.
- npm para instalação de dependências e execução dos scripts.
Arquivos
Arquivo	Responsabilidade
server.js	Configurar o Express, implementar as rotas e regras da API e iniciar o serviço na porta 8002.
tests.js	Concentrar todos os testes automatizados em um único arquivo.
dados.json	Armazenar os bilhetes em formato JSON.
package.json	Declarar o Express e os scripts de inicialização e testes.


Persistência
O server.js deve carregar os bilhetes de dados.json ao iniciar. Se o arquivo não existir, deve criá-lo com uma lista vazia ([]). Após criar ou encerrar um bilhete, deve atualizar o arquivo. Os dados devem ser recuperados nas próximas inicializações.
Testes
Criar tests.js utilizando node:test e node:assert/strict. Cada cenário deve ser um teste independente, incluindo caminho feliz, formatos inválidos, limites de campos e dados vazios. Os testes devem usar dados isolados dos bilhetes do serviço.
Execução
- npm install: instalar as dependências.
- npm start: executar node server.js.
- npm test: executar node --test tests.js.