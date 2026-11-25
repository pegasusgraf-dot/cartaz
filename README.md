# Gerador de Cartazes

Aplicação Flask para criar cartazes promocionais em PDF, com pré-visualização, fundos prontos e importação de planilhas. Preparada para execução local e deploy na Vercel.

## Deploy na Vercel

1. Importe `pegasusgraf-dot/cartaz` na Vercel (ou conecte o repositório GitHub).
2. A Vercel identifica `vercel.json` e instala as dependências de `requirements.txt`.
3. Em **Settings → Environment Variables**, cadastre estas três variáveis em Production (e Preview, se necessário):
   - `CARTAZ_LOGIN`: usuário de acesso.
   - `CARTAZ_PASSWORD`: senha forte e exclusiva.
   - `CARTAZ_SECRET_KEY`: segredo aleatório longo usado para assinar a sessão. Gere um com `python -c "import secrets; print(secrets.token_urlsafe(48))"`.
4. Faça um novo deploy após salvar as variáveis.

A tela de login é a entrada da aplicação. As rotas do gerador exigem uma sessão autenticada; a sessão expira após 12 horas. Não coloque credenciais no código nem no repositório. Sem as três variáveis, o sistema não permite autenticação.

Na Vercel, arquivos temporários e PDFs são gravados em `/tmp`, pois o diretório do deploy é somente leitura e o armazenamento temporário não é permanente. Fundos dentro de `fundos_prontos/` são incluídos no deploy; para adicionar ou alterar fundos, faça commit dos arquivos no GitHub e redeploy.

> A Vercel executa funções serverless: arquivos enviados e resultados salvos em `/tmp` não ficam disponíveis em outro pedido nem após uma nova execução. Baixe os PDFs gerados ao recebê-los. A geração em lote pode ser limitada pelo tempo e memória do plano da Vercel.

## Execução local

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
source .venv/bin/activate
pip install -r requirements.txt
```

Defina `CARTAZ_LOGIN`, `CARTAZ_PASSWORD` e `CARTAZ_SECRET_KEY` no ambiente (o arquivo `.env.example` serve como referência; não contém credenciais válidas) e inicie:

```bash
python app.py
```

Abra `http://localhost:2000`. As credenciais são as mesmas variáveis de ambiente usadas no deploy.
