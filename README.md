Estrutura Técnica

Gatilho (Google Sheets Trigger)
Evento: rowUpdate
Documento: DISPAROS WHATSAPP
Aba monitorada: EMPRESAS_ABERTAS
O acionamento ocorre sempre que uma linha é atualizada na planilha.

Fluxo de Processamento

Filtro condicional (Switch): separa os leads ainda não enviados (STATUS vazio) dos já processados.

Ordenação reversa (Code Node - envio reverso1): organiza as linhas por número de registro (ROW_NUMBER) em ordem decrescente, priorizando as mais recentes.

Laço de repetição (Split In Batches): processa um lead por vez, controlando o ritmo de execução.

Formatação de telefone (Code Node): remove caracteres não numéricos, adiciona o código internacional 55 e normaliza o formato do número.

Distribuição e Envio

Distribuição aleatória de instância (random1): sorteia uma das instâncias disponíveis no Evolution, atribuindo os campos “instancia” e “apikey” para cada lead.

Envio de mensagem (HTTP Request - evo1): realiza uma requisição POST para o endpoint “https://evolution.estrelarconsultoria.com/message/sendText/{{
 $json.instancia }}”, enviando um corpo JSON com as informações da empresa.

Atualização de status (Google Sheets - update): marca o lead como “ENVIADO” na coluna STATUS após o envio bem-sucedido.

Controle de Intervalo

Temporizador (Code + Wait): aplica um intervalo aleatório entre 30 e 60 segundos entre os envios, reduzindo o risco de bloqueio das contas e equilibrando a taxa de disparos.

Campos Principais da Planilha
ROW_NUMBER: número da linha no documento.
TELEFONE: número de contato tratado automaticamente.
RAZÃO SOCIAL: nome da empresa.
CNPJ: número de inscrição da empresa.
STATUS: indica se o lead já foi processado (vazio ou ENVIADO).

Integrações Utilizadas
Google Sheets (gatilho e atualização).
HTTP Request (integração com Evolution API).
Code Node (tratamento de dados em JavaScript).
Wait (controle de tempo).
Switch (roteamento condicional).

Requisitos
Conta n8n com autenticação Google OAuth e permissão para HTTP Requests.
Planilha compartilhada com a conta de autenticação.
Acesso à API Evolution (URL e API Keys válidas).

Objetivo
Automatizar o envio de mensagens para empresas recém-abertas, eliminando a necessidade de execução manual, garantindo distribuição equilibrada entre instâncias de envio e mantendo o controle de status em tempo real.
