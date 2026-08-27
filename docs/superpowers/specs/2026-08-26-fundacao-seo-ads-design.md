# Fundação: correções críticas, conformidade e rastreamento — dicasinstax.com.br

**Data:** 2026-08-26
**Autora do site:** Amanda Santiago (CNPJ 42.131.027/0001-63)
**Status:** proposto

## Contexto

O site `dicasinstax.com.br` é um guia comparativo de câmeras instantâneas (Instax e
Polaroid), publicado como página estática única (`index.html`) num repositório
GitHub + deploy Vercel. Monetização é 100% via links de afiliado (Mercado Livre e
Amazon). A dona do site pretende (1) melhorar o SEO orgânico para o termo "câmera
instantânea" e correlatos, e (2) rodar campanhas pagas de Google Ads (Search)
direcionando tráfego para essa mesma página.

Uma auditoria do HTML publicado e do repositório identificou problemas que
comprometem tanto a monetização atual quanto a viabilidade de rodar Google Ads:

- **Metade dos links de afiliado da Amazon estão quebrados.** Usam o domínio
  `link.amazon/{código}`, que não é um domínio real da Amazon (o encurtador oficial
  é `amzn.to`; associados.amazon.com.br não reconhece esse domínio) e os "códigos"
  não têm formato válido de ASIN. Cliques nesses botões não geram comissão.
- **Todas as imagens do site (hero + 6 câmeras) são hospedadas em
  `deepseek.com.br`**, domínio da própria autora mas fora do domínio principal do
  site — ponto único de falha e sem benefício de SEO para `dicasinstax.com.br`.
- **Não existe Política de Privacidade nem identificação do responsável pelo
  site** — exigência de conformidade tanto para Google Ads quanto para LGPD.
- **O aviso de link de afiliado está pouco visível** (rodapé, fonte 0.7rem).
- **Nenhum script de rastreamento está instalado** (GA4 / Google Ads), apesar de a
  autora já ter as contas configuradas (tag do Google `G-RZNXE3PW3G`, destino GA4
  "Dicas Instax home", conversão Google Ads `AW-11562516053`).
- Arquivo solto `sitem.txt` (vazio, resquício).

Esta é a primeira de três etapas combinadas com a autora (abordagem "fundação
primeiro, depois camadas"): (1) fundação — este documento, (2) otimização da
página para conversão de tráfego pago, (3) expansão para arquitetura multi-página
visando SEO orgânico de cauda longa. As etapas 2 e 3 serão especificadas
separadamente, depois que esta for implementada.

## Objetivo

Eliminar os vazamentos de receita e os bloqueadores de conformidade antes de
qualquer investimento em tráfego (pago ou orgânico), e instalar a medição
necessária para decisões futuras.

## Fora de escopo (nesta etapa)

- Otimizações de conversão / copy da landing page para tráfego pago.
- Criação de novas páginas de conteúdo (comparativos individuais, artigos de
  cauda longa).
- Banner de consentimento de cookies (CMP) — mencionado como possível melhoria
  futura, não bloqueante para o público brasileiro nas políticas atuais do
  Google, mas fica registrado como item em aberto.
- Alteração de layout visual, paleta de cores ou copy das seções existentes.

## Escopo detalhado

### A. Corrigir links de afiliado da Amazon

Substituir os 6 links quebrados por links reais no formato
`https://www.amazon.com.br/dp/{ASIN}?tag=mpi0f2-20`, usando o ID de afiliado da
autora (`mpi0f2-20`). ASINs identificados (produto avulso, sem kit/bundle, para
bater com a faixa de preço já anunciada no site):

| Produto | ASIN | Link final |
|---|---|---|
| Fujifilm Instax Mini 12 | B0BWNYBRNL | `https://www.amazon.com.br/dp/B0BWNYBRNL?th=1&linkCode=ll2&tag=mpi0f2-20&linkId=94e8bebb52dc3b5b246bee18c4ee4e28&ref_=as_li_ss_tl` |
| Fujifilm Instax Mini 41 | B0F2V7RKXH | `https://www.amazon.com.br/dp/B0F2V7RKXH?&linkCode=ll2&tag=mpi0f2-20&linkId=703d3fd3ae9b45af0eb3bdb9b5dc8819&ref_=as_li_ss_tl` |
| Fujifilm Instax Square SQ1 | B08G5YV1Q6 | `https://www.amazon.com.br/dp/B08G5YV1Q6?th=1&linkCode=ll2&tag=mpi0f2-20&linkId=722955d2515018d8143f04d3fa3ca183&ref_=as_li_ss_tl` |
| Fujifilm Instax Mini Evo | B09M4DKBQ9 | `https://www.amazon.com.br/dp/B09M4DKBQ9?th=1&linkCode=ll2&tag=mpi0f2-20&linkId=d431f0204a350df3ce0a853d60818642&ref_=as_li_ss_tl` |
| Fujifilm Instax Mini LiPlay | B07SH2S36Q | `https://www.amazon.com.br/dp/B07SH2S36Q?&linkCode=ll2&tag=mpi0f2-20&linkId=4b1686aa3e7bab83b18a7feab9a93370&ref_=as_li_ss_tl` |
| Polaroid Now Geração 2 | B0BVNMQ2XL | `https://www.amazon.com.br/dp/B0BVNMQ2XL?th=1&linkCode=ll2&tag=mpi0f2-20&linkId=33f6ba1901df8d269fcfbf0d8db4334b&ref_=as_li_ss_tl` |

**Observação para revisão da autora:** os ASINs acima apontam para variantes de
cor específicas (nem sempre a mesma cor da foto do card). Revisar antes de
publicar e trocar por outra variante se preferir.

Os links de Mercado Livre (`meli.la/...`) usam o domínio curto oficial deles.
Serão mantidos como estão, mas a autora deve confirmar que ainda carregam sua
tag de afiliado válida (isso não é verificável de fora do painel de afiliados).

Todos os links de afiliado (Amazon e Mercado Livre) recebem o atributo
`rel="sponsored noopener noreferrer"` (hoje só têm `noopener noreferrer`) — é a
marcação que o Google recomenda para links pagos/monetizados.

### B. Auto-hospedar as imagens

Baixar as 7 imagens atualmente em `deepseek.com.br/wp-content/uploads/2026/06/`
e commitar em `/images/` no repositório do site. Atualizar todos os `src` e o
`link rel="preload"` do hero para caminho relativo (`/images/nome-do-arquivo.webp`).
Manter `width`/`height`/`loading`/`fetchpriority` como estão.

### C. Página de Política de Privacidade

Nova página estática `politica-de-privacidade.html`, linkada no rodapé de
`index.html` e adicionada ao `sitemap.xml`. Conteúdo:

- Identificação: Amanda Santiago, CNPJ 42.131.027/0001-63.
- Contato: mand.santiago@gmail.com.
- Natureza do site: guia comparativo independente, monetizado por links de
  afiliado (Mercado Livre e Amazon).
- Dados coletados: Google Analytics (GA4) e cookies de conversão do Google Ads —
  nenhum dado pessoal é coletado via formulário (o site não tem formulários).
- Nota padrão de direitos do titular conforme a LGPD (acesso, correção, exclusão
  — direcionando para o e-mail de contato).
- Data da última atualização.

### D. Aviso de afiliado mais visível

Manter o texto no rodapé, mas aumentar a fonte para o padrão do restante do
rodapé (de 0.7rem para ~0.85rem). Adicionar uma segunda menção curta e visível
logo após o hero / antes do índice: "Este site contém links de afiliados —
podemos ganhar comissão em compras qualificadas, sem custo extra para você."

### E. Instalar rastreamento (GA4 + Google Ads)

Adicionar o snippet padrão `gtag.js` da tag do Google (`G-RZNXE3PW3G`) no
`<head>` de `index.html` (e da nova página de privacidade). Essa tag já está
configurada nas contas do Google para alimentar tanto o GA4 ("Dicas Instax
home") quanto a conversão do Google Ads (`AW-11562516053`) — não é necessário
snippet adicional por destino.

Adicionar rastreamento de evento de clique nos botões de afiliado (Mercado
Livre e Amazon), disparando `gtag('event', 'clique_afiliado', {...})` com
parâmetros de produto e loja, via delegação de evento (sem alterar o HTML de
cada botão individualmente).

### F. Faxina técnica

Remover o arquivo vazio `sitem.txt` do repositório.

## Critérios de aceite

- Os 6 links da Amazon abrem página de produto real na Amazon.com.br com a tag
  `mpi0f2-20` na URL.
- Todas as imagens carregam de `dicasinstax.com.br/images/...`, sem nenhuma
  referência a `deepseek.com.br` no HTML publicado.
- `politica-de-privacidade.html` acessível a partir do rodapé de todas as
  páginas e listada no `sitemap.xml`.
- Console do navegador sem erros 404 de imagem ou link.
- Evento do GA4 Realtime dispara ao carregar a página, e evento de clique
  dispara ao clicar num botão de afiliado (validável em GA4 DebugView).
- `sitem.txt` não existe mais no repositório.

## Riscos e observações em aberto

- ASINs escolhidos podem não ser exatamente a cor/variante desejada — revisão
  manual da autora recomendada antes do deploy.
- Links do Mercado Livre não foram auditados quanto à validade da tag de
  afiliado (fora do alcance desta análise).
- Banner de consentimento de cookies (LGPD) não incluído nesta etapa — considerar
  para uma iteração futura se a autora quiser reforçar conformidade.
