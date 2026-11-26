# 👻 Ghost Multiplayer (Nand2Tetris — Linguagem Jack)

Implementação de uma versão competitiva para **dois jogadores (multiplayer local)** do clássico jogo *Ghost*, desenvolvida em linguagem **Jack** como projeto prático do **Capítulo 9 (High-Level Language)** do curso **Nand2Tetris** (*The Elements of Computing Systems*).

---

## 📋 Sobre o Projeto

Na versão original do *Ghost*, um único jogador movimenta um fantasma livremente pela tela. Nesta evolução multiplayer, o jogo ganha um **labirinto com obstáculos**, **movimentação contínua**, **contagem regressiva em duas fases** e um **segundo jogador** que controla uma pessoa tentando sobreviver até o tempo acabar.

### ✨ Principais Funcionalidades
- **Dois Jogadores Simultâneos:** Controle independente e sem conflito de teclas (leitura não-bloqueante).
- **Movimento Automático Contínuo:** Basta pressionar a direção desejada uma vez para que o personagem se desloque continuamente até encontrar uma parede ou receber o comando de parada.
- **Labirinto em Grade ($32 \times 16$):** Mapa com bordas e corredores internos que impedem a passagem dos personagens.
- **Posicionamento Aleatório Seguro:** A cada rodada, Fantasma e Pessoa surgem em células livres aleatórias distintas.
- **Relógio Regressivo em Duas Etapas:**
  1. **Fase de Fuga (`3 segundos`):** Apenas o Jogador 2 (Pessoa) pode se mover para ganhar distância.
  2. **Fase de Perseguição (`7 segundos`):** Ambos os jogadores se movimentam pelo labirinto.
- **Fim de Jogo e Reinício Automático:** Exibe no centro da tela quem perdeu, aguarda 3 segundos e inicia uma nova rodada automaticamente (ou encerra ao pressionar `ESC`).

---

## 🎮 Regras e Controles

### Objetivo
- **Jogador 1 (Fantasma):** Alcançar e tocar na Pessoa antes que o cronômetro de `7 segundos` chegue a zero.
- **Jogador 2 (Pessoa):** Fugir pelo labirinto durante os `3 segundos` iniciais de vantagem e sobreviver aos `7 segundos` de perseguição.

### Mapeamento de Teclas

| Jogador / Função | Cima | Baixo | Esquerda | Direita | Parar Movimento |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Jogador 1 (Fantasma)** | `↑` (Seta Cima) | `↓` (Seta Baixo) | `←` (Seta Esq.) | `→` (Seta Dir.) | `.` ou `;` |
| **Jogador 2 (Pessoa)** | `W` | `S` | `A` | `D` | `E` |
| **Sistema** | — | — | — | — | `ESC` (Encerrar jogo) |

---

## 🗂️ Estrutura do Projeto

O código segue arquitetura modular orientada a objetos, dividido em 6 classes:

```text
ghost-multiplayer-jack/
├── Main.jack         # Ponto de entrada: instancia e executa o jogo
├── GhostGame.jack    # Controlador principal: loop do jogo, teclado, relógios, colisões e HUD
├── Map.jack          # Representação do labirinto em matriz 32x16 e detecção de paredes
├── Ghost.jack        # Entidade do Jogador 1 (movimentação e renderização do sprite 16x16)
├── Person.jack       # Entidade do Jogador 2 (movimentação e renderização do sprite 16x16)
├── Random.jack       # Gerador de números pseudoaleatórios para o spawn inicial
└── README.md         # Documentação do projeto
```

### Detalhes Técnicos da Implementação
- **Mapeamento de Tela:** A tela do computador Hack ($512 \times 256$ pixels) é tratada como uma grade lógica de $32 \times 16$ blocos de $16 \times 16$ pixels. A linha `0` é reservada para a barra de status (relógio e instruções), enquanto as linhas `1` a `15` formam o labirinto.
- **Renderização Direta em Memória:** Os sprites são desenhados escrevendo diretamente na memória de vídeo (base `16384`) via `Memory.poke`, garantindo desempenho fluido sem flickers.
- **Temporização:** Como a linguagem Jack não possui relógio de hardware nativo, cada ciclo do loop principal executa `Sys.wait(50)` ($50\text{ ms}$), acumulando 20 ciclos para decrementar `1 segundo` no cronômetro.

---

## 🚀 Como Compilar e Executar

### Pré-requisitos
- Ter a suíte de ferramentas do **Nand2Tetris** instalada (`JackCompiler` e `VMEmulator`) com Java configurado.

### Passo a Passo

1. **Clone este repositório:**
   ```bash
   git clone https://github.com/SEU_USUARIO/ghost-multiplayer-jack.git
   cd ghost-multiplayer-jack
   ```

2. **Compile os arquivos `.jack` para código intermediário `.vm`:**
   - No **Windows**:
     ```bash
     JackCompiler.bat .
     ```
   - No **Linux / macOS**:
     ```bash
     ./JackCompiler.sh .
     ```

3. **Execute no VMEmulator:**
   - Abra o **VMEmulator** do Nand2Tetris.
   - Vá em `File` → `Load Program` e selecione a pasta do projeto.
   - No menu superior **Animate**, selecione **`No animation`** (essencial para que o temporizador de $50\text{ ms}$ funcione na velocidade correta).
   - Clique em **Run (`>>`)** para iniciar a partida.

---

## 🎨 Personalização dos Sprites

Os desenhos dos personagens são matrizes de $16 \times 16$ pixels convertidas em 16 inteiros de 16 bits. Para editar os sprites utilizando o **Bitmap Editor** do Nand2Tetris:
- **Fantasma:** Substitua os valores de `Memory.poke` no método `drawHappyGhost` dentro de `Ghost.jack`.
- **Pessoa:** Substitua os valores de `Memory.poke` no método `drawPersonSprite` dentro de `Person.jack`.

---

## 👥 Autores

- **Jezreel David** — *Lógica do jogo, labirinto, física de colisão e temporizadores*
- **Carlos Yuri** — *Design dos sprites 16×16 e documentação/apresentação*

---

## 📄 Licença

Este projeto foi desenvolvido para fins acadêmicos na disciplina de Arquitetura de Computadores / Sistemas Computacionais, baseado na plataforma educacional [Nand2Tetris](https://www.nand2tetris.org/).
