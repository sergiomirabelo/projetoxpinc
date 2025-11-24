# 232 EDU --- Aplicativo de Educação Financeira Gamificada

------------------------------------------------------------------------

1. Contexto

A educação financeira para crianças e adolescentes ainda é pouco
abordada nas escolas tradicionais, apesar de ser um tema essencial para
o desenvolvimento pessoal e profissional de jovens. Ao mesmo tempo,
dispositivos móveis e aplicativos educativos têm se tornado ferramentas
fundamentais para aprendizado moderno, especialmente quando combinam
elementos de gamificação e interatividade.

O 232 Edu surge como uma solução inovadora, oferecendo uma jornada
de aprendizado divertida, leve e progressiva. Através de trilhas com
vídeos, textos, exercícios, desafios e simulação de investimentos, o app
busca criar hábitos de responsabilidade financeira desde cedo.

------------------------------------------------------------------------

2. Objetivo

Desenvolver um aplicativo educativo gamificado que oriente crianças e
adolescentes sobre conceitos essenciais de educação financeira,
utilizando trilhas progressivas, recompensas, desafios e simulações
reais de investimentos. O objetivo é tornar o aprendizado acessível,
envolvente e significativo.

------------------------------------------------------------------------

3. Arquitetura do Sistema

O diagrama a seguir ilustra o fluxo de dados e a arquitetura geral do sistema,
envolvendo o Frontend Mobile, o Backend de Lógica e Regras, e a camada de
Persistência e Integrações.

![Diagrama de Fluxo de Dados](https://private-us-east-1.manuscdn.com/sessionFile/knXmnrUJhpWVY1cs87iVKP/sandbox/99T2zkWWE0MEhXNlKMBOG6-images_1763966952567_na1fn_L2hvbWUvdWJ1bnR1L2Fzc2V0cy9kaWFncmFtYUZsdXhvRGFkb3M.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9wcml2YXRlLXVzLWVhc3QtMS5tYW51c2Nkbi5jb20vc2Vzc2lvbkZpbGUva25YbW5yVUpocFdWWTFjczg3aVZLUC9zYW5kYm94Lzk5VDJ6a1dXRTBNRWhYTmxLTUJPRzYtaW1hZ2VzXzE3NjM5NjY5NTI1NjdfbmExZm5fTDJodmJXVXZkV0oxYm5SMUwyRnpjMlYwY3k5a2FXRm5jbUZ0WVVac2RYaHZSR0ZrYjNNLnBuZyIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc5ODc2MTYwMH19fV19&Key-Pair-Id=K2HSFNDJXOU9YS&Signature=a6zYK98mDRH0w7oh1F2YsHs~7~8dgPt1ip2vr7ll5ObtuB1X~USiWY7fogl03vE3gQfiDKvg1vvmHZg6QsFWuMMy88cYGY81pVG3E1DPYLhLJkMVGx1jDK1NAErSrHNNqRAZkndpMyG9Tkbnwk8OJl4OOLmOCrh6Em7H4qYPiYB1s5QwCMIPt7nnOCuBskdTYQ52Jxq~EWy72olQbjY0Smxg3kqeUMCzcXo~2e1BxpP6B4426TC2qWjpDucMETppENda9rE2~ZoYAof7s5pCA~qVg3DL9p6UgkAbsu~mbLjWT2QBE8rGlvDq3Opq-6pO3NzogJXR2nqjJA0ZxIuR9Q__)

------------------------------------------------------------------------

4. Estrutura de Banco de Dados

A seguir, apresentamos a proposta de banco de dados para suportar
trilhas, usuários, progresso, missões e simulações de investimento.

### Diagrama ER (forma textual)

erDiagram

    USERS {
        UUID user_id PK
        VARCHAR name
        VARCHAR email
        VARCHAR password_hash
        INT age
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    LEVELS {
        UUID level_id PK
        VARCHAR name
        TEXT description
        INT order_index
        TIMESTAMP created_at
    }

    CONTENT {
        UUID content_id PK
        UUID level_id FK
        VARCHAR type
        TEXT url_or_text
        INT sequence
    }

    EXERCISES {
        UUID exercise_id PK
        UUID level_id FK
        TEXT question
        TEXT correct_answer
    }

    USER_PROGRESS {
        UUID progress_id PK
        UUID user_id FK
        UUID level_id FK
        BOOLEAN completed
        TIMESTAMP completed_at
    }

    MISSIONS {
        UUID mission_id PK
        TEXT description
        INT reward_credits
        BOOLEAN active
    }

    USER_MISSIONS {
        UUID user_mission_id PK
        UUID user_id FK
        UUID mission_id FK
        BOOLEAN completed
        TIMESTAMP completed_at
    }

    INVESTMENT_SIMULATIONS {
        UUID simulation_id PK
        UUID user_id FK
        DECIMAL initial_amount
        DECIMAL monthly_rate
        INT months
        DECIMAL final_amount
        TIMESTAMP created_at
    }

    USERS ||--o{ USER_PROGRESS : tracks
    USERS ||--o{ USER_MISSIONS : completes
    USERS ||--o{ INVESTMENT_SIMULATIONS : performs
    LEVELS ||--o{ CONTENT : contains
    LEVELS ||--o{ EXERCISES : includes

### Descrição das Entidades

**USERS**\
Armazena dados dos usuários do aplicativo, incluindo idade e
credenciais.

**LEVELS**\
Define os níveis da trilha: Iniciante, Médio e Avançado.

**CONTENT**\
Itens da trilha: vídeos, textos e conteúdos educativos.

**EXERCISES**\
Questões dos exercícios antes do desafio final.

**USER_PROGRESS**\
Acompanhamento da evolução do aluno nas trilhas.

**MISSIONS**\
Missões reais que geram créditos (como economizar valor real).

**USER_MISSIONS**\
Registra quais missões cada usuário completou.

**INVESTMENT_SIMULATIONS**\
Histórico das simulações feitas pelo usuário.

------------------------------------------------------------------------

5. Estrutura de Front-end

O front-end do aplicativo será desenvolvido em React Native, usando
NativeWind para estilização rápida e responsiva.

Tecnologias Principais

-   React Native\
-   NativeWind\
-   React Navigation\
-   Axios\
-   Recharts (para gráficos da simulação)

Estrutura de Pastas

src/\
├── components/\
│ ├── VideoPlayer.tsx\
│ ├── TextContent.tsx\
│ ├── Quiz.tsx\
│ ├── MissionsCard.tsx\
│ └── SimulationChart.tsx\
├── pages/\
│ ├── Login.tsx\
│ ├── Home.tsx\
│ ├── Levels.tsx\
│ ├── LevelContent.tsx\
│ ├── Missions.tsx\
│ └── Simulation.tsx\
├── hooks/\
│ ├── useAuth.ts\
│ ├── useLevels.ts\
│ └── useSimulation.ts\
├── services/\
│ ├── api.ts\
│ ├── userService.ts\
│ ├── missionService.ts\
│ └── simulationService.ts\
└── utils/\
├── formatters.ts\
└── validations.ts

------------------------------------------------------------------------

6. Fluxos e Imagens Ilustrativas

Esta seção apresenta as telas principais do aplicativo, ilustrando os fluxos de usuário.

### 6.1 Autenticação

As telas de Login e Cadastro são o ponto de entrada do usuário no aplicativo.

| Login | Cadastro |
| :---: | :---: |
| ![Tela de Login](https://private-us-east-1.manuscdn.com/sessionFile/knXmnrUJhpWVY1cs87iVKP/sandbox/99T2zkWWE0MEhXNlKMBOG6-images_1763966952568_na1fn_L2hvbWUvdWJ1bnR1L2Fzc2V0cy9Mb2dpbg.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9wcml2YXRlLXVzLWVhc3QtMS5tYW51c2Nkbi5jb20vc2Vzc2lvbkZpbGUva25YbW5yVUpocFdWWTFjczg3aVZLUC9zYW5kYm94Lzk5VDJ6a1dXRTBNRWhYTmxLTUJPRzYtaW1hZ2VzXzE3NjM5NjY5NTI1NjhfbmExZm5fTDJodmJXVXZkV0oxYm5SMUwyRnpjMlYwY3k5TWIyZHBiZy5wbmciLCJDb25kaXRpb24iOnsiRGF0ZUxlc3NUaGFuIjp7IkFXUzpFcG9jaFRpbWUiOjE3OTg3NjE2MDB9fX1dfQ__&Key-Pair-Id=K2HSFNDJXOU9YS&Signature=DwPhlbdGnBvwMnlqiaaOwE1RRRmLG8IjtsEW96skJw5E8NQoReLG58BL3BF2uHilsFR7goDHWWxr~jtqvQuGbersk6j37C0eRHo15dPUF-mlFoU~QoPOE4KA7xaqd1rbQcVPNvBsMgumgGeIcuwcfD6frEQcdNq0uYs7rWcoDwXyFvYlnMkStcdCHRo5kwcdCsTWsPiuTOkXVbIrUCLyzDI3hMpkWcq6~UkR7rVYQgA~HAmLKxVITurUxpRvTUFd-OWbotx8ojNkCG7hyMrXBVLwOIfPV8oqOMugWAxO8Vn-1Aug8hNkvZs7w1W~n9gSrVGvPbdg2fnFXMfvLmBexw__) | ![Tela de Cadastro](https://private-us-east-1.manuscdn.com/sessionFile/knXmnrUJhpWVY1cs87iVKP/sandbox/99T2zkWWE0MEhXNlKMBOG6-images_1763966952569_na1fn_L2hvbWUvdWJ1bnR1L2Fzc2V0cy9DYWRhc3Rybw.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9wcml2YXRlLXVzLWVhc3QtMS5tYW51c2Nkbi5jb20vc2Vzc2lvbkZpbGUva25YbW5yVUpocFdWWTFjczg3aVZLUC9zYW5kYm94Lzk5VDJ6a1dXRTBNRWhYTmxLTUJPRzYtaW1hZ2VzXzE3NjM5NjY5NTI1NjlfbmExZm5fTDJodmJXVXZkV0oxYm5SMUwyRnpjMlYwY3k5RFlXUmhjM1J5YncucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzk4NzYxNjAwfX19XX0_&Key-Pair-Id=K2HSFNDJXOU9YS&Signature=ddfNU4mqKHopVssPvKz14dAnifQmyQbdWTeZNFV2Kxw8klRrQ9nOYVzueUyi-N-lGxUCfpaHTa-uoRo8rdPIh2AEgAmiCpUGI3j20SoHJ9tcbtbOjooBJx4NhjU703b2ZLvdE~nBKEo~x71Zjc9y-LYRTCDkSRNgBWZPjs-RqxEfSfKHGCfI7IGabqj2IcLD1imV0WMxaoMcajbt9PMmJJa1NAaU~TobyAhOeJyRX9r~GbmRXXhjFw672OgJ4PDyVHxz5teB17eGGgv78EBdL9u-yrRJ1C4~kJ7SH3putry6NlWAuOVHMbSHvc-KzdVAL-3Hp9EOqYecnlAORa-afQ__) |

### 6.2 Tela Inicial e Trilha de Aprendizado

A tela inicial mostra o progresso e as missões diárias, enquanto a tela de trilha
apresenta o caminho gamificado de aprendizado.

| Tela Inicial (Home) | Trilha de Aprendizado |
| :---: | :---: |
| ![Tela Inicial](https://private-us-east-1.manuscdn.com/sessionFile/knXmnrUJhpWVY1cs87iVKP/sandbox/99T2zkWWE0MEhXNlKMBOG6-images_1763966952569_na1fn_L2hvbWUvdWJ1bnR1L2Fzc2V0cy9ob21l.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9wcml2YXRlLXVzLWVhc3QtMS5tYW51c2Nkbi5jb20vc2Vzc2lvbkZpbGUva25YbW5yVUpocFdWWTFjczg3aVZLUC9zYW5kYm94Lzk5VDJ6a1dXRTBNRWhYTmxLTUJPRzYtaW1hZ2VzXzE3NjM5NjY5NTI1NjlfbmExZm5fTDJodmJXVXZkV0oxYm5SMUwyRnpjMlYwY3k5b2IyMWwucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzk4NzYxNjAwfX19XX0_&Key-Pair-Id=K2HSFNDJXOU9YS&Signature=PK-g5TsXNzbcP7aHlEZotjY8-mW9iUBs~SRj9mFg0gxaumUKWkxkTdA-ArtbSayI-lZL8~62mxsXu9YHa~T3IYBfxojGgzifnwhmD-DnILPrXeGi5V~2i-pYQwh96AlKvBEPKtssHfRkbVJlwMx2SxOiTNRqjfcYocqzjFnu-avpVpPLGiwOFKv5edHIRlYlhViDScrRKVYgKP2jN6EqjVz9aj9PTUp2tjSXzq7Jdx1z5WsfHFzB4qsTM1y089RasEJeIWPtaLs7pxr2mae1XByFqeifvBlLaKsK8gJJtRuST3DeqpMyrPNN3frLShHfQucuY2N3SxNbmcTkGlrSpA__) | ![Tela da Trilha](https://private-us-east-1.manuscdn.com/sessionFile/knXmnrUJhpWVY1cs87iVKP/sandbox/99T2zkWWE0MEhXNlKMBOG6-images_1763966952569_na1fn_L2hvbWUvdWJ1bnR1L2Fzc2V0cy90cmlsaGE.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9wcml2YXRlLXVzLWVhc3QtMS5tYW51c2Nkbi5jb20vc2Vzc2lvbkZpbGUva25YbW5yVUpocFdWWTFjczg3aVZLUC9zYW5kYm94Lzk5VDJ6a1dXRTBNRWhYTmxLTUJPRzYtaW1hZ2VzXzE3NjM5NjY5NTI1NjlfbmExZm5fTDJodmJXVXZkV0oxYm5SMUwyRnpjMlYwY3k5MGNtbHNhR0UucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzk4NzYxNjAwfX19XX0_&Key-Pair-Id=K2HSFNDJXOU9YS&Signature=rS8cURw8Sv9-1etZGGj260NlVfRWrdVJOo8WdsDggZhmLA0UJXuoGZhrxtsqWrRrrRehu4NkKV7XQ8nHyb3DI643G0qdm7NebbnWpqF~Jwd5oztj~oixyy6NQ~n7jAQFtALdKdOtWPOu~C2u99nfQ9FKDZ8cR8~PQMtx4bC2ELHNWn7HU5M4LgkbpifsLZmv6~XY4UrLbxo7igXk26IbX2T7aL97EKCwhECpwfgpObTP9Va9Vshj7REvHeLpQMKnThdrdzKpvbdnCmCSWwQHqGWr6DU8Cp9BHKIne7Rkh0WWM0RpJFGZ2~O19jZTtt4o7Hy2JIbEZNkDS~QnMJtrLw__) |

### 6.3 Simulação de Investimentos

O fluxo de simulação permite ao usuário testar diferentes cenários de investimento.

| Simulação - Entrada de Dados | Simulação - Resultado |
| :---: | :---: |
| ![Simulação de Investimento - Tela 1](https://private-us-east-1.manuscdn.com/sessionFile/knXmnrUJhpWVY1cs87iVKP/sandbox/99T2zkWWE0MEhXNlKMBOG6-images_1763966952570_na1fn_L2hvbWUvdWJ1bnR1L2Fzc2V0cy9pbnZlc3RpbWVudG8xLjA.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9wcml2YXRlLXVzLWVhc3QtMS5tYW51c2Nkbi5jb20vc2Vzc2lvbkZpbGUva25YbW5yVUpocFdWWTFjczg3aVZLUC9zYW5kYm94Lzk5VDJ6a1dXRTBNRWhYTmxLTUJPRzYtaW1hZ2VzXzE3NjM5NjY5NTI1NzBfbmExZm5fTDJodmJXVXZkV0oxYm5SMUwyRnpjMlYwY3k5cGJuWmxjM1JwYldWdWRHOHhMakEucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzk4NzYxNjAwfX19XX0_&Key-Pair-Id=K2HSFNDJXOU9YS&Signature=WtyUM6J9ko-g~Jngs5viBYraM4z91u-F4J1kWeyMHuDlnEfGplZ~DFnjy4q0KF6-SzsjGhu29budcE4fteK5pIymhsykYR4VkgYymrC-F3RWaTjuYyRzjGvAjTt3gRwYXsE3HM1U-bmwTzD4i6ii-KPiaGAw~mXozXitjjRogbeIUgEW~uvfu~4L6XvkAvFHOV~V3vnlX-XuKZFu94hSXzDVPUZGmryrgm2WyA8MCpuj6e1qeiu3VeX7GPUOxkWVJ5gwocavlFFHiVsGweLIA149meIeGRkBq4VVInlA0FVHUoUrWeQNVW9tZskNFpuon9RwABBCFiIhRWwWh-Yg0w__) | ![Simulação de Investimento - Tela 2](https://private-us-east-1.manuscdn.com/sessionFile/knXmnrUJhpWVY1cs87iVKP/sandbox/99T2zkWWE0MEhXNlKMBOG6-images_1763966952570_na1fn_L2hvbWUvdWJ1bnR1L2Fzc2V0cy9pbnZlc3RpbWVudG8yLjA.png?Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9wcml2YXRlLXVzLWVhc3QtMS5tYW51c2Nkbi5jb20vc2Vzc2lvbkZpbGUva25YbW5yVUpocFdWWTFjczg3aVZLUC9zYW5kYm94Lzk5VDJ6a1dXRTBNRWhYTmxLTUJPRzYtaW1hZ2VzXzE3NjM5NjY5NTI1NzBfbmExZm5fTDJodmJXVXZkV0oxYm5SMUwyRnpjMlYwY3k5cGJuWmxjM1JwYldWdWRHOHlMakEucG5nIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNzk4NzYxNjAwfX19XX0_&Key-Pair-Id=K2HSFNDJXOU9YS&Signature=DkkJdG8pxzEDEkP5bAZYW6r5bcuokPv5tyfy4wTxCAjeW~Iul-7R2yt9bY0OHdk0xA2GGk5veSqI~mIuET0e9fSqGvrBn1RG7bTrhuSFmp3wZmrFZw8cq0kjN2dI-eKK9JFy0AZF-BaFbQmsbid2BoG2apgoFf7jQHonwccD-LpItVMKud1z7QMEvM3mhcE5GJpkwW-htmUSOvw3kUhPiJ0G98cu0vihh1ppCgEU7Y82jna3x2E80uO9RTh1MMSNzeaym~5TofSR1F4K2SvsRGCg0uBxBpTvsAOJ-XGxtpYwGdjt4P9K2XtxHIG-62udoSOSoHGyLKhPadkb~sd6-w__) |

------------------------------------------------------------------------

7. Código de Exemplo

Backend (Kotlin + Spring)

``` kotlin
@PostMapping("/simulate")
fun simulate(@RequestBody request: SimulationRequest): SimulationResponse {
    val finalAmount = request.initial * Math.pow(1 + request.rate, request.months.toDouble())
    return SimulationResponse(finalAmount)
}
```

------------------------------------------------------------------------

8. Considerações de Implementação

Requisitos Técnicos

-   Front-end: React Native\
-   Backend: Kotlin + Spring Boot\
-   Banco: PostgreSQL\
-   Gráficos: Recharts\
-   Autenticação: JWT\
-   Armazenamento de vídeos: S3 ou Firebase Storage

Segurança

-   Senhas com hash\
-   Criptografia em dados sensíveis\
-   Conexões HTTPS\
-   Proteção contra XSS e SQL Injection

KPIs do Projeto

-   Nível médio de conclusão das trilhas\
-   Quantidade de missões realizadas\
-   Total de simulações feitas\
-   Retenção semanal de usuários

------------------------------------------------------------------------

9. Próximos Passos

-   Implementar novos níveis e trilhas\
-   Adicionar novos tipos de investimentos\
-   Criar avatars e itens adicionais\
-   Integrar notificações push
