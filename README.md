# RELATORIOS

Web App em Google Apps Script para geração do Relatório Analítico CRP com dados em Google Sheets.

A interface atual usa um layout de dashboard executivo minimalista: filtros compactos, KPIs no topo, relatório A4 para PDF e tela de Administração operacional para configurar a base sem alterar código.

## Estrutura da planilha

A planilha do relatório deve conter, no mínimo:

- `BASE_DADOS(NÃOEDITAR)`: base principal CRP com cabeçalhos de `A` até `AQ`.
- `CRO`: base futura/alternativa para CRO, quando disponível.
- `CRP_REL_CONFIG`: aba criada automaticamente pelo app para guardar configurações editáveis do relatório.
- `CRP_REL_CONFIG_LOG`: aba criada automaticamente para histórico de alterações administrativas.

## Filtros disponíveis

O relatório CRP suporta filtros por:

- ano;
- mês;
- setor/unidade;
- eixo;
- categoria;
- satisfação;
- status da avaliação.

A comissão `CRO` permanece visível como recurso futuro, mas desativada na interface.

## Administração do relatório

A aba `CRP_REL_CONFIG` armazena:

- meta institucional;
- ID da planilha do relatório;
- nome da aba da base CRP;
- termos de classificação (`Conforme`, `Não conforme`, `Não se aplica`);
- nomes editáveis dos indicadores da CRP;
- dados da última atualização.

A edição recomendada é pela tela **Administração do relatório**, mas a aba existe na própria planilha para auditoria, manutenção e portabilidade.

Use **Recarregar da planilha** no Admin quando alguém editar `CRP_REL_CONFIG` diretamente. Esse fluxo chama `api=configrel&refresh=1`, invalida o cache e força uma nova leitura da aba.

## Publicação no Apps Script

Arquivos principais:

- `Code.gs`: backend Apps Script, rotas JSON, leitura da planilha, cache e processamento do relatório.
- `index.html`: interface do Web App e exportação PDF.
- `appsscript.json`: manifesto mínimo com runtime V8 e fuso `America/Fortaleza`.

Para publicar manualmente, copie os arquivos para o projeto Apps Script vinculado ao Web App e faça um novo deploy.

Se usar `clasp`, configure o `.clasp.json` local apontando para o Script ID do projeto e execute:

```bash
clasp push
clasp deploy
```

Não versionar `.clasp.json` se ele contiver identificadores de ambiente que não devem ser compartilhados.
