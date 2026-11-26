# Design Doc: Expansão Fase 2 — Interior do Hospital Maria Amélia & Chefão Super-Vírus

**Data:** 2026-11-25
**Arquivo-alvo principal:** `index.html` (aplicação única HTML5/CSS/JS Canvas 2D)

## 1. Visão Geral e Propósito
Expandir o jogo de plataforma 2D *Super Enfermagem: Caminho para o Maria Amélia* mantendo toda a física, controles, áudio retrô via Web Audio API e identidade visual existentes da Fase 1, adicionando:
1. Melhorias de animação e feedback visual para moedas/corações, blocos `[?]` e power-ups em todas as fases.
2. Transição fluida ao concluir a Fase 1 para a nova **Fase 2: Interior do Hospital Maria Amélia Buarque de Hollanda**.
3. **Salas Informativas da Área da Saúde** (4 salas temáticas educativas e interativas ao longo dos corredores).
4. **Batalha Final (Boss Fight)** contra o **Super-Vírus Mutante** na ala de isolamento/laboratório, encerrando com vitória celebrando o Hospital Maria Amélia Buarque de Hollanda e a Enfermagem.

## 2. Resolução de Conflito com Código Legado
O arquivo `index.html` atual possui telas HTML não utilizadas de um rascunho anterior (`#stage1-clear-screen` "Voltar para casa com Ryan", `#victory-screen` "Lar Doce Lar" e a classe `RyanGarciaFollower` nunca instanciada), enquanto `Player.update()` atualmente chama `triggerVictory()` direto ao entrar na porta do hospital.
- **Decisão aprovada:** Substituir os textos/telas de "Voltar para casa / Lar Doce Lar" pela transição **"Fase 2: Alas do Hospital Maria Amélia"** e pela tela de **Vitória Final no Hospital Maria Amélia Buarque de Hollanda**. Remover ou reaproveitar o código morto (`RyanGarciaFollower`), mantendo Ryan Garcia como mentor de saúde (recepcionando na Fase 1 e na entrada da Fase 2).

## 3. Abordagem Arquitetural
- **Arquitetura escolhida:** Estender `Level`, `Block`, `HealthDrop`, `PowerUpItem`, `Player` e `GameEngine` dentro do mesmo `index.html` com suporte explícito a múltiplas fases (`stage = 1 | 2`).
- **Por que outras não servem:** Dividir em múltiplos módulos JS/HTML violaria o requisito de manter tudo em um único arquivo HTML5/JS funcional; duplicar o motor por fase causaria divergência de física e controles.
- **Preservação de estado entre fases:** Ao completar a Fase 1, o jogo armazena `stage1Score` (para exibição na transição e no resumo final) e preserva `score`, `hearts`, `lives` e `player.state` (`STATE_NORMAL`, `STATE_SUPER`, `STATE_SANITIZER`) ao iniciar a Fase 2.

## 4. Detalhamento dos Recursos

### 4.1 Animações e Feedback Visual (Todas as Fases)
- **`HealthDrop` (Corações/Moedas da Saúde):**
  - Flutuação senoidal contínua no eixo Y.
  - Rotação/giro 3D simulado no eixo X (`cos(spinPhase)`) com espessura lateral e brilho especular dourado/branco periódico.
- **Partículas de Coleta (`SparkleParticle` / `Particle` estendida):**
  - Suporte a formatos de estrela (`star`) e cruz médica (`cross`), além de círculos, subindo com leve desaceleração e fade-out ao coletar moedas/corações (verde/dourado) e power-ups.
  - Textos flutuantes rápidos (`+100`, `+VIDA!`, etc.).
- **Blocos `[?]` e Power-ups:**
  - Símbolo `?` com pulsação senoidal suave de escala e brilho.
  - Power-ups (`LAMP`: Lâmpada da Enfermagem / Kit Primeiros Socorros; `GEL`: Seringa / Álcool em Gel) emergem com interpolação suave (ease-out) e rastro de partículas brilhantes.

### 4.2 Transição e Cenário da Fase 2
- **Transição Fase 1 → Fase 2:**
  - Ao baixar a bandeira e entrar na porta do hospital na Fase 1, `Player.update()` aciona `gameState.triggerStageClear()` em vez de `triggerVictory()`.
  - Exibe overlay rápido **"Fase 2: Alas do Hospital Maria Amélia"** com pontuação da Fase 1 e barra automática de transição (~2.5s) + botão/atalho imediato (`ENTRAR NAS ALAS ▶`) para seguir para o interior do hospital.
- **Cenário Interno (`Level` com `stage === 2`):**
  - Fundo com paredes branco-gelo, teto com luminárias hospitalares, faixas sinalizadoras verde-esmeralda, janelas internas, macas decorativas, monitores cardíacos com traçado de ECG animado e placas de sinalização hospitalar.
  - Piso vinílico hospitalar e obstáculos internos (bancadas/armários médicos, cilindros de oxigênio, barreiras clínicas, inimigos `PurpleBacteria` e `SpinyBacteria`).

### 4.3 Salas Informativas da Área da Saúde
- **4 Salas Temáticas ao longo da Fase 2:**
  1. `Sala 1: Higienização e Prevenção`
  2. `Sala 2: Vacinação e Imunidade`
  3. `Sala 3: Cuidado Humanizado e Acolhimento`
  4. `Sala 4: Segurança do Paciente e Triagem`
- **Visual e Interação:**
  - Desenhadas no fundo com porta aberta, iluminação acolhedora, placa legível e totem informativo.
  - Ao passar pela porta/totem, sem travar a movimentação da jogadora:
    - Concede bônus único de `+500` pontos ("Conhecimento Adquirido"), partículas de brilho e áudio suave.
    - Exibe um painel informativo elegante e não intrusivo na parte superior da tela (abaixo do HUD principal) com título da sala, dica real curta de saúde/enfermagem e botão/atalho opcional para fechar antes do tempo automático (~7s).

### 4.4 Batalha Final: Chefão Super-Vírus (Boss Fight)
- **Entrada e Arena:**
  - Ao final do corredor da Fase 2 (`Laboratório / Ala de Isolamento`), quando Maria Garcia entra na arena, a câmera trava na largura da arena e uma barreira de contenção impede sair pela esquerda.
  - Inclui piso sólido, plataformas laterais de apoio e **1 bloco `[?]` de suporte** central-alto que libera `GEL` (Seringa/Álcool em Gel) ou `LAMP` (se no estado normal) e se regenera após cooldown (~10s) caso a jogadora perca o power-up durante o combate.
- **Chefão (`SuperVirusBoss`):**
  - 5 pontos de vida (`hp = 5`), exibidos em barra de vida fixa no topo da tela.
  - Visual mutante roxo/avermelhado gigante com expressão ameaçadora e espinhos retráteis.
  - Padrão de ataques cíclico: deslocamento/saltos pela arena, disparo de pequenos esporos/toxinas (`VirusSpore`) e investida lateral com espinhos ativados (sinalização visual clara antes de ativar espinhos).
  - **Formas de dano:**
    1. Pular exatamente na cabeça quando `spikesActive === false` (impulso seguro para cima e para trás).
    2. Acertar com projéteis de Seringa/Álcool em Gel (`SanitizerDrop`).
- **Vitória Final:**
  - Ao zerar a vida do Super-Vírus, o chefão explode em partículas verdes e douradas de limpeza/purificação, a enfermeira comemora na tela por breve momento e é exibida a tela final de vitória celebrando o **Hospital Maria Amélia Buarque de Hollanda**, mostrando pontuação da Fase 1, Fase 2, total acumulado, salas visitadas e botão **"JOGAR NOVAMENTE"** (reinício completo na Fase 1).
