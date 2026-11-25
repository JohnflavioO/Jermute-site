# Projeto Jermute Moraes Advocacia — Ponto de partida

> Status: **planejamento** (desenvolvimento ainda não iniciado)
> Atualizado: 24/11/2026
> Referência de identidade: [`docs/referencias/JMM_Conceito_Visual_Home_1.pdf`](referencias/JMM_Conceito_Visual_Home_1.pdf) (conceito para aprovação, não publicado)

## 1. Contexto

Migração dos sites do Dr. Jermute Moraes, hoje em WordPress (domínio principal
+ subdomínios por área), para um **site único** em `jermute.adv.br`, com
visual, estrutura e banco de dados novos.

| Site atual | Destino no novo site |
|---|---|
| https://jermute.adv.br/ | Início |
| https://familiaesucessoes.jermute.adv.br/ | Família e Patrimônio |
| https://trabalhista.jermute.adv.br/ | Página interna (conteúdo revisto) |
| https://direitoeleitoral.jermute.adv.br/ | Página interna (conteúdo revisto) |
| Demais `*.jermute.adv.br` | Levantar lista; mapear cada URL para a página equivalente |

## 2. Stack

- **Código:** GitHub (repositório novo, ex.: `jermute-site`)
- **Banco/Auth/Storage:** Supabase (região São Paulo — `sa-east-1`)
- **Deploy:** Vercel
- **Front-end:** Next.js (App Router) + TypeScript + Tailwind

## 3. Arquitetura — site único (definido pelo conceito)

> "O domínio é a casa da marca."

**Mapa do site:**

- `/` — Início
- `/familia-e-patrimonio` — partilhas, disputas familiares, sucessões, organização patrimonial
- `/recursos-e-tribunais` — análise de decisões, viabilidade recursal, estratégia de contencioso
- `/trabalhista` — página interna (conteúdo revisto)
- `/eleitoral` — página interna (conteúdo revisto)
- `/artigos` e `/artigos/[slug]`
- `/sobre`
- `/contato`
- `/metodo` (ou âncora na Home: "Entender o caso. Explicar as opções. Definir os próximos passos.")

**Navegação (desktop):** Atuação · Método · Artigos · [Contato] (botão com contorno)
**Navegação (celular):** marca + "Menu"

**Subdomínios antigos:** redirecionamento 301 página a página para o novo
endereço — só depois do mapa de URLs aprovado. Os subdomínios ficam apontados
para a Vercel apenas para servir os redirects.

**Formulário:** pode ficar em sistema separado (ex.: Supabase + função), mas
com a mesma aparência e navegação.

## 4. Identidade visual (guia do conceito)

**Direção:** editorial, não tema pronto. Poucos elementos por tela.
**Proibido:** carrossel, vídeo automático, pop-up de contato.

### Paleta de partida

| Token | Cor | Uso |
|---|---|---|
| `navy` (Azul-noturno) | `#13283C` | Hero, contraste, texto principal |
| `ivory` (Marfim) | `#F8F5EF` | Áreas de leitura |
| `gold` (Dourado fosco) | `#D6B782` | Acento: botões e linhas sobre fundo escuro |
| `gold-ink` (a validar) | `~#7A5C2E` | Rótulos/eyebrows pequenos sobre fundo claro |
| `sand` (fundo externo) | `~#E8E3DB` | Fundo de apoio (visto no PDF) |

**Contraste medido (WCAG 2.2 AA exige 4,5:1 para texto normal):**

- Azul-noturno × Marfim: **13,8:1** ✅
- Dourado × Azul-noturno: **7,8:1** ✅
- Dourado × Marfim: **1,8:1** ❌ — confirma a regra do guia: dourado nunca em texto sobre fundo claro
- Os rótulos em caixa-alta sobre marfim no PDF usam um dourado escurecido; `#7A5C2E` dá 5,7:1 ✅ (confirmar o tom com o Dr. Jermute)

### Tipografia

- **Títulos:** serifada (no PDF parece Noto Serif — confirmar)
- **Texto:** sem serifa (parece Open Sans / Noto Sans — confirmar)
- **Rótulos:** caixa-alta, peso bold, espaçamento amplo, numeração `01`, `02`

### Elementos da Home

1. Cabeçalho: "JERMUTE MORAES / ADVOCACIA" (**provisório** — aplicar arquivo oficial do símbolo JM e validar em tamanho pequeno)
2. Hero azul-noturno: rótulo "ADVOCACIA / ATENDIMENTO DIRETO", título "Decisões que pedem leitura cuidadosa.", subtítulo, botão dourado "Como atendemos →", ilustração em linhas (arcos concêntricos dourados)
3. Áreas de atuação: 01 Família e Patrimônio · 02 Recursos e Tribunais (linha superior, título serifado, seta)
4. Método de trabalho: "Entender o caso. Explicar as opções. Definir os próximos passos."

## 5. Modelo de dados inicial (rascunho)

- `areas` — slug, nome, descrição curta, ordem, ativo
- `paginas` — slug, título, conteúdo, SEO, publicado
- `artigos` — area_id, slug, título, resumo, conteúdo, capa, publicado_em
- `faqs` — area_id, pergunta, resposta, ordem
- `leads` (contato) — nome, telefone, e-mail, área, mensagem, origem, criado_em
- `redirects` — url_antiga, url_nova, status (mapa de migração)
- `configuracoes` — telefone, WhatsApp, endereços, OAB, redes sociais

RLS em todas as tabelas: leitura pública só do que está publicado; `leads`
com insert público e leitura apenas autenticada.

## 6. Implantação (regras do conceito)

- **Primeiro a Home. Depois a migração.**
- Backup completo dos WordPress antes de qualquer mudança
- Versão de teste (preview Vercel) antes de publicar
- **Não trocar layout, sistema e endereços ao mesmo tempo**
- Mapear cada URL antiga → página equivalente antes dos 301
- Preservar endereços antigos e acompanhar indexação (Google Search Console)
- **Nenhuma mudança de domínio ou contato sem aprovação**

## 7. Critérios de aceite (o que John deverá demonstrar)

- [ ] Celular, tablet e desktop conferidos
- [ ] Navegação por teclado e foco visível
- [ ] Contraste WCAG 2.2 AA
- [ ] Todos os botões testados
- [ ] Formulário testado com dados fictícios
- [ ] Desempenho medido antes e depois (Core Web Vitals / Lighthouse)
- [ ] Cada exceção ao layout registrada e submetida antes de publicar

## 8. Pendências

- [ ] Arquivo oficial do símbolo JM (SVG)
- [ ] Confirmar fontes e tom do dourado para texto pequeno
- [ ] Lista completa de subdomínios e inventário de páginas
- [ ] Exportar conteúdo do WordPress (posts, páginas, mídias)
- [ ] Acesso ao DNS de `jermute.adv.br`
- [ ] Integrações atuais (Analytics/Tag Manager, Pixel, WhatsApp)
- [ ] Textos de Sobre, Método e páginas Trabalhista/Eleitoral revistos
- [ ] Dados de contato, OAB, endereços, redes sociais

## 9. Fases

1. Infra: repositório, Supabase, Vercel (preview)
2. Design system (tokens, tipografia, componentes)
3. **Home** → aprovação
4. Páginas de área, Artigos, Sobre, Contato
5. Migração de conteúdo + mapa de URLs
6. Publicação + 301 dos subdomínios + acompanhamento de indexação
