# ADR-0001: Stack mobile para o app de catálogo e pedidos da loja de roupas

## Status

`Aceito`

**Data:** 2026-09-23
**Autor:** Allainn Christiam

## Contexto

- **Produto:** app de catálogo (fotos, tamanhos, cores, estoque) e pedidos (carrinho, checkout, acompanhamento) para uma loja de roupas local
- **Público:** clientes da loja que hoje chegam principalmente por Instagram e WhatsApp; lançamentos de coleção e promoções são o principal motivo de retorno
- **Time:** 3 devs, todos com experiência em JavaScript/TypeScript e React para web, nenhum com Kotlin ou Swift
- **Restrição:** lançar em **2 meses** nas lojas (Android e iOS), com manutenção feita pelo mesmo time depois
- **O que não pode dar errado:** perder o prazo de lançamento ou depender de uma tecnologia que o time ainda precisa aprender do zero

## Decisão

Adotaremos **React Native com Expo** (TypeScript), porque reaproveita o conhecimento em React do time e entrega um app nas duas lojas com um único código dentro do prazo.

## Alternativas consideradas

| Alternativa | Prós | Contras |
|---|---|---|
| **React Native + Expo** (escolhida) | Mesma linguagem e modelo de componentes que o time já usa (React/TS); 1 código para Android e iOS; Expo simplifica build, publicação e push; EAS Update permite corrigir JS sem esperar revisão da loja | Camada extra entre o código e a plataforma; recursos nativos muito específicos exigem módulo nativo; dependência do ecossistema Expo/Meta |
| PWA | Sem loja, lançamento mais rápido de todos; link direto do Instagram/WhatsApp abre o catálogo; o time já domina a stack web | Push no iOS só funciona depois de "Adicionar à Tela de Início" (iOS 16.4+), o que reduz o alcance das campanhas; sem presença nas lojas; instalação pouco intuitiva para o cliente leigo |
| Flutter | UI consistente e alta performance; 1 código para as duas plataformas | Time precisaria aprender Dart e o modelo de widgets do zero: curva de aprendizado incompatível com 2 meses |
| Nativo (Kotlin + Swift) | Melhor performance e acesso total às APIs de cada plataforma | 2 codebases e 2 linguagens novas para um time de 3 devs web; inviável no prazo |

## Consequências

**Positivas:**
- O time começa a produzir no primeiro dia, sem troca de linguagem, e divide telas e regras de negócio num único repositório
- Correções e ajustes de catálogo em JS chegam aos usuários via EAS Update, sem depender da revisão da App Store
- Push notifications nativas nas duas plataformas para avisar de coleção nova e status do pedido

**Negativas:**
- O time ainda precisa aprender o essencial de mobile (ciclo de publicação nas lojas, contas Apple/Google, permissões, assinatura de build); esse tempo entra no cronograma
- Se o app precisar de um recurso nativo sem biblioteca pronta (ex.: provador com câmera/AR), será necessário escrever módulo nativo, fora da zona de conforto do time
- A camada de abstração cobra um custo quando o app cresce muito: Airbnb (2018) e Shopify (2026) voltaram para o nativo em escala muito maior. **Gatilho de revisão:** reabrir este ADR se o app passar a exigir recursos nativos pesados ou se o time crescer e ganhar especialistas em mobile

## Referências

- REACT NATIVE. *Getting Started* (documentação oficial). Meta Open Source. Disponível em: https://reactnative.dev/docs/getting-started. Acesso em: 23 set. 2026.
- EXPO. *Expo Documentation* (documentação oficial). Disponível em: https://docs.expo.dev. Acesso em: 23 set. 2026.
- CHARLAND, A.; LEROUX, B. Mobile Application Development: Web vs. Native. *Communications of the ACM*, v. 54, n. 5, p. 49-53, 2011. DOI: 10.1145/1941487.1941504.
- PEAL, G. *React Native at Airbnb* / *Sunsetting React Native*. Airbnb Engineering (Medium), 2018. Disponível em: https://medium.com/airbnb-engineering/react-native-at-airbnb-f95aa460be1c. Acesso em: 23 set. 2026.
- SHOPIFY ENGINEERING. *Native is now the future of mobile at Shopify*, 2026. Disponível em: https://shopify.engineering/back-to-native. Acesso em: 23 set. 2026.
- NYGARD, M. *Documenting Architecture Decisions*. Cognitect Blog, 2011. Disponível em: https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions. Acesso em: 23 set. 2026.
