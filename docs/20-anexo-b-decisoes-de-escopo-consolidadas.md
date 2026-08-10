# 20 - Anexo B - Decisões de escopo consolidadas

> Fonte: [Projeto FieldOps no Notion](https://glaucotodesco.notion.site/Projeto-FieldOps-3af5d52a81f580c0b92ec261c686abb4)
>
> Página Notion: 3af5d52a-81f5-8167-9af5-ee9509189b8b

# Anexo B — Decisões de escopo consolidadas

1. O produto possui três aplicações: mobile, administrativa web e API REST.

1. A interface administrativa faz parte do MVP e não será substituída pelo Swagger.

1. O mobile é prioritariamente Android.

1. O aplicativo deverá operar offline após baixar uma inspeção.

1. O SQLite é persistência operacional, não cache descartável.

1. A sincronização utilizará operações idempotentes.

1. O modelo de inspeção será versionado.

1. Cada inspeção preservará um snapshot dos itens.

1. O MVP terá um técnico responsável por inspeção.

1. O portal do cliente ficará fora do MVP.

1. Fotografias serão a evidência principal.

1. QR Code e localização serão recursos obrigatórios do projeto mobile.

1. A coleta de localização será pontual.

1. A revisão será realizada pela interface administrativa.

1. As 16 semanas efetivas são o limite para planejamento do MVP.

1. As semanas institucionais restantes não serão consideradas reserva de desenvolvimento.

1. A avaliação combinará produto da equipe e domínio individual.

