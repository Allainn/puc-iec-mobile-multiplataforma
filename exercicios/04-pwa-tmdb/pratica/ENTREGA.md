# ENTREGA — Atividade 4 (PWA)

> Preencha e faça commit junto com o código. Esta parte vale 3 pts (avaliação manual no Canvas).

**Nome:** Allainn Christiam Jacinto Tavares · **GitHub:** Allainn

---

## 1. Site comum × PWA — o que EU vi (3 diferenças)

Comparei o PWA do professor (`https://pwa-tmdb-aula4.vercel.app`) com o site do TMDB (`https://www.themoviedb.org`), no Edge/Chrome do Windows, com DevTools aberto.

| # | Site comum (themoviedb.org) | PWA (pwa-tmdb-aula4) | Print/evidência |
|---|---|---|---|
| 1 | **Sem rede + recarregar:** o navegador mostra a própria página de erro ("Sem internet"). Nada do site carrega. | O app abre normalmente: o Service Worker entrega o app shell do precache e a lista vem do cache (`tmdb-api` + IndexedDB), com os pôsteres (`tmdb-images`). | [01](evidencias/01-site-comum-offline.png) · [02](evidencias/02-pwa-offline.png) |
| 2 | **Application → Service Workers / Cache Storage:** nenhum SW registrado (o HTML não registra um e `/sw.js` dá 404) e nenhum cache do app. | SW `sw.js` **activated and running**; Cache Storage com `workbox-precache`, `tmdb-api` e `tmdb-images`. | [03](evidencias/03-pwa-service-worker.png) |
| 3 | **Fechar e reabrir (Network):** cada reabertura busca o HTML e os scripts no servidor; sem rede, nem o atalho instalado abre. | Na reabertura, o app shell vem do SW (coluna **Size** = `(ServiceWorker)`). O manifest (nome, ícones 192/512, `display: standalone`) permite **Instalar**: abre em janela própria, sem barra de endereço, e funciona sem rede. | [04](evidencias/04-pwa-reabrir-network.png) · [05](evidencias/05-pwa-instalado.png) |

## 2. Prova do offline no SEU build

Build local: `npm run build && npm run preview` → http://localhost:4173, recarregado 1× para o SW assumir a página.

- [ ] Print do **DevTools → Application → Cache Storage** com `tmdb-api` e `tmdb-images` (com itens): [06-cache-storage.png](evidencias/06-cache-storage.png)
- [ ] Print do app em **modo avião** carregando a lista: [07-modo-aviao.png](evidencias/07-modo-aviao.png) (DevTools → Network → Offline → recarregar)
- [x] Print do aviso **"Nova versão disponível"**: [claude-04-nova-versao.png](evidencias/claude-04-nova-versao.png) → depois de **Atualizar**, a página recarrega com o build novo ([claude-05-atualizado.png](evidencias/claude-05-atualizado.png), título de teste "Filmes Populares v2", revertido depois)

Também conferido no meu build (prints `claude-*`, ver seção 4):

| O que | Resultado | Print |
|---|---|---|
| 1ª instalação do SW (TODO 3) | aviso "✅ App pronto para uso offline" | [claude-01](evidencias/claude-01-pronto-offline.png) |
| Cache Storage depois de rolar a lista (TODO 2) | `tmdb-api`: 2 respostas · `tmdb-images`: 28 pôsteres · precache: 8 arquivos | — (contagem lida com `caches.keys()`) |
| Rede desligada (página **e** SW) + recarregar | 20 filmes e 20 pôsteres carregados, todos vindos do cache | [claude-02](evidencias/claude-02-offline-inicio.png) |
| Aba ★ Favoritos offline (TODOs 4 e 5) | favorito listado + faixa "📡 Offline — exibindo dados do cache" | [claude-03](evidencias/claude-03-offline-favoritos.png) |

Testes: `npm test` com **19/19** verdes: 8 modelos do professor (`01-MovieCard` e `03-useFavorites`), 2 do TODO 1, 1 caso extra de `page` no `02-tmdb-service` e 8 autorais (`04-useOfflineStatus` e `05-favorites-tab`).

## 3. Reflexão (5–8 linhas)

`autoUpdate` × `prompt`: qual você escolheria para um app de **checkout** e para um app de **notícias**, e por quê? O que pode dar errado em cada escolha?

**Checkout → `prompt`.** No `autoUpdate` o SW novo assume sozinho e a página recarrega; no meio de um pagamento isso pode apagar o formulário, reenviar um pedido ou trocar o JavaScript entre o "Confirmar" e a resposta do servidor. Com `prompt`, o usuário termina a compra e atualiza quando quiser. O risco é o contrário: quem sempre clica "Depois" continua numa versão antiga, que pode quebrar quando a API mudar ou ficar sem uma correção de segurança. Eu mitigaria versionando a API e exigindo a atualização quando a versão do cliente não for mais aceita. **Notícias → `autoUpdate`.** Não há transação para perder, o conteúdo já vem da rede (NetworkFirst) e vale mais todo leitor receber logo a correção de layout ou de bug. O que pode dar errado: o recarregamento automático tira o leitor do ponto do artigo em que estava, ou o deixa por um instante com HTML de uma versão e JS de outra.

## 4. Declaração de uso de IA

Usei IA? Onde e como? (uso transparente é incentivado)

Sim. Usei o **Claude Code** (Anthropic, modelo Claude Opus 5.5) como par de programação:

- **Código:** implementou os TODOs 1 a 5, com um commit por TODO, e escreveu os testes extras (caso 3 de `02-tmdb-service` e os arquivos `04-useOfflineStatus` e `05-favorites-tab`). Também rodou o autograder do professor localmente (12/12).
- **Verificação:** conferiu o build num Edge headless pelo Chrome DevTools Protocol: Cache Storage, rede desligada na página e no Service Worker, aviso de nova versão. Os prints `evidencias/claude-*.png` saíram dessa verificação.
- **Texto:** rascunhou este `ENTREGA.md`.

Eu revisei o código e as decisões (hosts e estratégias de cache, `cacheableResponse` com status 0 para os pôsteres opacos, `prompt` no lugar de `autoUpdate`), rodei `npm test`, `npm run build` e `npm run preview` na minha máquina, tirei os prints do DevTools (01 a 07) e revisei a reflexão.

## Referências

- GOOGLE. **Workbox: caching strategies overview**. Chrome for Developers. Disponível em: https://developer.chrome.com/docs/workbox/caching-strategies-overview/. Acesso em: 19 out. 2026.
- GOOGLE. **Learn PWA**. web.dev. Disponível em: https://web.dev/learn/pwa. Acesso em: 19 out. 2026.
- VITE PWA. **Automatic reload** e **Prompt for update**. Disponível em: https://vite-pwa-org.netlify.app/guide/auto-update.html. Acesso em: 19 out. 2026.
- MOZILLA. **Service Worker API**. MDN Web Docs. Disponível em: https://developer.mozilla.org/pt-BR/docs/Web/API/Service_Worker_API. Acesso em: 19 out. 2026.
