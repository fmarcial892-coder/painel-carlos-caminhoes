# Painel administrativo — Carlos Caminhões

Aplicação independente da loja para consultar pedidos, clientes, estado de pagamento e contatos enviados por integrações. Este repositório não altera o checkout, o cálculo dos valores nem o PIX da loja.

## Funcionalidades
- Login administrativo com cookie HttpOnly assinado e sessão de 8 horas.
- Limite de tentativas de login e comparação segura de segredos.
- PostgreSQL persistente para pedidos e contatos.
- Pesquisa por cliente, e-mail, telefone ou ID; filtro de pagamento.
- Detalhes de cliente, endereço, itens, origem/campanha e data de pagamento.
- Indicadores de pedidos, pagos, pendentes, valor recebido e contatos.
- Exportação CSV dos pedidos.
- Endpoints privados para receber pedidos, atualizações de pagamento e contatos.

## Publicação na Render
1. Crie um banco PostgreSQL novo para este painel. Não reutilize banco de outro sistema.
2. Crie um Web Service a partir deste repositório: https://github.com/fmarcial892-coder/painel-carlos-caminhoes
3. Root Directory: vazio (raiz). Build Command: `npm install`. Start Command: `npm start`.
4. Configure no serviço do painel: `DATABASE_URL` (Internal Database URL), `ADMIN_PASSWORD` (senha forte), `ADMIN_SESSION_SECRET` (segredo longo aleatório), `ADMIN_INGEST_TOKEN` (outro segredo longo aleatório), `NODE_ENV=production`.
5. No serviço da loja, configure `ADMIN_PANEL_URL` com a URL HTTPS do painel e `ADMIN_INGEST_TOKEN` com o mesmo token. Não altere outras variáveis da loja.
6. Faça um pedido de teste controlado e confira o registro e a confirmação de pagamento da Elite PAYbr antes de confiar no painel para operação diária.

Nunca coloque senhas ou tokens no GitHub ou no navegador. Configure segredos apenas nas variáveis de ambiente da Render.

## Integração e limites
- `POST /api/ingest/order`, `POST /api/ingest/payment` e `POST /api/ingest/lead` exigem o cabeçalho `x-admin-ingest-token`.
- A loja precisa estar configurada para enviar pedidos. O painel não recupera pedidos antigos que não tenham sido enviados.
- Cliques do Google Ads, eventos do GA4 e abertura do WhatsApp não revelam automaticamente nome ou telefone. Um formulário ou integração autorizada precisa enviar os dados do lead.
- O estado do pagamento depende dos status reais enviados pela integração. Valide com teste real antes de uso operacional.
