# pacmanGame

## Pacman.java
  Contém o método main onde vai buildar todas as classes e componentes do projeto.
  
## Model.java

## Pode ser dividido em 7 partes principais:

## PARTE 1:

<strong> Pacote e Importaçoes: </strong>
* package PacmanCode;

* import java.awt.*;
  <p>Contém todas as classes para criar interfaces de usuário e para pintar gráficos e imagens. Um objeto de interface do usuário é chamado de componente.</p>
* import java.awt.event.ActionEvent;
* import java.awt.event.ActionListener;
* import java.awt.event.KeyAdapter;
* import java.awt.event.KeyEvent;
* import javax.swing.ImageIcon;
  <p>Uma implementação da interface Icon que pinta ícones a partir de imagens. As imagens criadas a partir de um URL, nome de arquivo ou matriz de bytes são pré-carregadas usando o MediaTracker para monitorar o estado carregado da imagem.</p>
* import javax.swing.JPanel;
* import javax.swing.Timer;

## PARTE 2:
`public class Model extends JPanel implements ActionListener {

}`

<p>A classe Model estende a classe JPanel e implementa a interface ActionListener. Isso significa que essa classe será responsável por representar a tela do jogo e também por escutar os eventos de ação (temporizador).</p>

## PARTE 3 - Variaveis e Constantes:

`private Dimension d;
private final Font smallFont = new Font("Arial", Font.BOLD, 14);
private boolean inGame = false;
private boolean dying = false;

private final int BLOCK_SIZE = 24;
private final int N_BLOCKS = 15;
private final int SCREEN_SIZE = N_BLOCKS * BLOCK_SIZE;
private final int MAX_GHOSTS = 12;`

<p>Nessa parte, são declaradas as variáveis e constantes que serão utilizadas no jogo, como tamanho do bloco(BLOCK_SIZE), tamanho da tela(SCREEN_SIZE), quantidade máxima de fantasmas(MAX_GHOSTS) etc.</p>

## PARTE 4 - Variáveis de Posição e Movimento:

`private int N_GHOSTS = 6;
private int lives, score;
private int[] dx, dy;
private int[] ghost_x, ghost_y, ghost_dx, ghost_dy, ghostSpeed;
private Image heart, ghost;
private Image up, down, left, right;
private int pacman_x, pacman_y, pacmand_x, pacmand_y;
private int req_dx, req_dy;`

* N_GHOSTS: Define o número de fantasmas no jogo. Nesse caso, há inicialmente 6 fantasmas.
lives, score: Representam as vidas e a pontuação do jogador.
* dx, dy: São os vetores de deslocamento utilizados para o movimento dos personagens do jogo (Pac-Man e fantasmas). Por exemplo, dx[0] representa o deslocamento em x do Pac-Man, dy[0] representa o deslocamento em y do Pac-Man, e assim por diante.
* ghost_x, ghost_y: Arrays que armazenam as posições x e y dos fantasmas.
* ghost_dx, ghost_dy: Arrays que armazenam os deslocamentos dx e dy dos fantasmas. Esses valores são usados para definir a direção de movimento dos fantasmas.
* ghostSpeed: Array que armazena a velocidade de cada fantasma.
* heart, ghost, up, down, left, right: Variáveis que armazenam as imagens do coração (vida do jogador), do fantasma e das diferentes direções do Pac-Man. Essas imagens são carregadas através do método loadImages().
* pacman_x, pacman_y: Representam as coordenadas x e y do Pac-Man na tela.
* pacmand_x, pacmand_y: São as variáveis que representam o deslocamento dx e dy do Pac-Man (sua direção de movimento).
* req_dx, req_dy: Representam a direção que o jogador deseja que o Pac-Man siga. Quando o jogador pressiona as teclas de movimento, essas variáveis são atualizadas para refletir a direção desejada.

<p>Essas variáveis são cruciais para controlar o movimento dos personagens e determinar as ações que ocorrem durante o jogo. Por exemplo, o movimento do Pac-Man é controlado pela variável pacmand_x e pacmand_y, que é alterada quando o jogador pressiona as teclas de seta. A posição do Pac-Man é atualizada multiplicando-se o deslocamento pelo valor da velocidade do Pac-Man (definido por PACMAN_SPEED). A movimentação dos fantasmas é controlada pelas variáveis ghost_x e ghost_y, que são atualizadas de acordo com seus deslocamentos ghost_dx e ghost_dy e velocidades individuais (ghostSpeed).</p>

## PARTE 5 - Matriz de Dados do Labirinto:

`private final short[] levelData = {
            19, 18, 18, 18, 18, 18, 18, 18, 18, 18, 18, 18, 18, 18, 22,
            17, 16, 16, 16, 16, 24, 16, 16, 16, 16, 16, 16, 16, 16, 20,
            25, 24, 24, 24, 28, 0, 17, 16, 16, 16, 16, 16, 16, 16, 20,
            0,  0,  0,  0,  0,  0, 17, 16, 16, 16, 16, 16, 16, 16, 20,
            19, 18, 18, 18, 18, 18, 16, 16, 16, 16, 24, 24, 24, 24, 20,
            17, 16, 16, 16, 16, 16, 16, 16, 16, 20, 0,  0,  0,   0, 21,
            17, 16, 16, 16, 16, 16, 16, 16, 16, 20, 0,  0,  0,   0, 21,
            17, 16, 16, 16, 24, 16, 16, 16, 16, 20, 0,  0,  0,   0, 21,
            17, 16, 16, 20, 0, 17, 16, 16, 16, 16, 18, 18, 18, 18, 20,
            17, 24, 24, 28, 0, 25, 24, 24, 16, 16, 16, 16, 16, 16, 20,
            21, 0,  0,  0,  0,  0,  0,   0, 17, 16, 16, 16, 16, 16, 20,
            17, 18, 18, 22, 0, 19, 18, 18, 16, 16, 16, 16, 16, 16, 20,
            17, 16, 16, 20, 0, 17, 16, 16, 16, 16, 16, 16, 16, 16, 20,
            17, 16, 16, 20, 0, 17, 16, 16, 16, 16, 16, 16, 16, 16, 20,
            25, 24, 24, 24, 26, 24, 24, 24, 24, 24, 24, 24, 24, 24, 28
    };`

<p>Essa matriz levelData armazena os dados do labirinto do jogo. Cada elemento da matriz representa um bloco do labirinto e é representado por um valor numérico. O labirinto é formado por uma grade de blocos, onde cada bloco pode conter paredes, espaços vazios ou pontos para o Pac-Man coletar.</p>

<p>Os valores dos elementos da matriz levelData indicam o que está presente em cada posição do labirinto. Vejamos o que cada valor representa:</p>

* 0: Espaço vazio. O Pac-Man pode se mover livremente nessas posições.
* 1: Parede sólida. O Pac-Man não pode atravessar essas posições.
* 2: Linha vertical. Representa uma parede sólida vertical.
* 4: Linha horizontal. Representa uma parede sólida horizontal.
* 8: Ponto grande. Esses pontos adicionam pontos à pontuação do jogador e são coletados quando o Pac-Man passa por eles.
* 16: Ponto pequeno. Esses pontos também adicionam pontos à pontuação do jogador e são coletados quando o Pac-Man passa por eles.

<p>ssa matriz é fundamental para a lógica do jogo, pois controla a posição das paredes, pontos e outros elementos do labirinto. A matriz é utilizada para verificar colisões, determinar a movimentação dos personagens e verificar se o jogador completou o labirinto ao coletar todos os pontos. Durante o jogo, o Pac-Man verifica a posição atual no levelData para determinar quais movimentos são permitidos e quais ações ele deve tomar, como coletar pontos ou perder vidas ao colidir com fantasmas.

A matriz screenData é uma matriz semelhante à levelData e é usada para armazenar temporariamente o estado atual do labirinto durante o jogo, permitindo que elementos sejam modificados sem afetar a estrutura original do labirinto.</p>

## PARTE 6 - Variáveis do Jogo:

`private final int[] validSpeeds = {1, 2, 3, 4, 6, 8};
private int currentSpeed = 4;
private short[] screenData;
private Timer timer;`

* validSpeeds: É um array que contém os valores de velocidades válidas para os fantasmas. Esses valores são usados para definir a velocidade de cada fantasma durante o jogo. No código, os valores são 1, 2, 3, 4, 6 e 8, representando diferentes níveis de velocidade. O nível de velocidade é escolhido aleatoriamente para cada fantasma quando o jogo começa.

* currentSpeed: Representa a velocidade atual do jogo. No início do jogo, é definido como 4. Conforme o jogador progride no jogo, essa velocidade pode ser alterada para valores mais altos para tornar o jogo mais desafiador.

* screenData: É uma matriz que armazena temporariamente o estado atual do labirinto durante o jogo. Ela é uma cópia da matriz levelData e é utilizada para realizar modificações temporárias no labirinto sem afetar a estrutura original. Essa matriz é atualizada durante o jogo para refletir as mudanças no labirinto, como a coleta de pontos pelo Pac-Man e a movimentação dos fantasmas.

* timer: É um objeto da classe javax.swing.Timer. Ele é responsável por disparar eventos de ação em intervalos regulares (definidos em milissegundos). Neste caso, ele é usado para atualizar a tela do jogo em intervalos regulares para criar a animação e o movimento dos personagens. O timer está configurado para disparar eventos a cada 500 milissegundos (ou 0,5 segundos) e é iniciado no construtor do jogo. Quando o temporizador é ativado, o método actionPerformed(ActionEvent e) é chamado, e a tela é redesenhada através do método repaint().

<p>Essas variáveis são importantes para controlar a dinâmica do jogo, como a velocidade de movimento dos fantasmas, o nível de dificuldade do jogo e a atualização periódica da tela para criar a animação. O uso do temporizador (timer) garante que o jogo seja atualizado em um ritmo constante, evitando atualizações muito rápidas ou muito lentas, o que poderia afetar a jogabilidade e a experiência do jogador.</p>

## PARTE 7 - Métodos do Jogo:

* public Model(): O construtor da classe Model. É chamado quando um objeto da classe é criado. Esse construtor é responsável por inicializar as variáveis, carregar as imagens e configurar o jogo para iniciar.

* private void loadImages(): Método privado que carrega as imagens do jogo. As imagens do Pac-Man, fantasmas, coração e direções são carregadas aqui usando o caminho dos arquivos de imagem fornecidos.

* private void initVariables(): Método privado que inicializa as variáveis do jogo, criando os arrays para os fantasmas, suas velocidades, direções, entre outros.

* private void playGame(Graphics2D g2d): Método privado que é chamado durante o jogo, controlando o fluxo do jogo, verificando colisões, movimentando o Pac-Man e fantasmas, desenhando-os na tela e atualizando a pontuação.

* private void showIntroScreen(Graphics2D g2d): Método privado que mostra a tela de introdução do jogo. Mostra o texto "Press SPACE to start" na tela antes de o jogo começar.

* private void drawScore(Graphics2D g): Método privado para desenhar a pontuação e o número de vidas na tela.

* private void checkMaze(): Método privado para verificar se o jogador completou o labirinto. Ele verifica se todos os pontos foram coletados e, se sim, aumenta a pontuação, aumenta o número de fantasmas e ajusta a velocidade do jogo.

* private void death(): Método privado chamado quando o Pac-Man colide com um fantasma. Reduz o número de vidas do jogador e, se não houver mais vidas, define a variável inGame como false para encerrar o jogo.

* private void moveGhosts(Graphics2D g2d): Método privado que controla o movimento dos fantasmas. Atualiza suas posições de acordo com suas velocidades e direções, verifica colisões com o Pac-Man e, se houver uma colisão, define a variável dying como true para representar que o jogador perdeu uma vida.

* private void drawGhost(Graphics2D g2d, int x, int y): Método privado para desenhar um fantasma na tela, recebendo sua posição x e y.

* private void movePacman(): Método privado para controlar o movimento do Pac-Man. Atualiza sua posição de acordo com sua velocidade e direção, verificando colisões com paredes e pontos.

* private void drawPacman(Graphics2D g2d): Método privado para desenhar o Pac-Man na tela, de acordo com sua direção de movimento.

* private void drawMaze(Graphics2D g2d): Método privado para desenhar o labirinto na tela, usando as informações da matriz screenData para desenhar paredes, pontos e outros elementos.

* private void initGame(): Método privado para inicializar o jogo. Define o número de vidas, a pontuação e chama o método initLevel() para começar o jogo.

* private void initLevel(): Método privado para inicializar um novo nível do jogo. Ele copia a matriz levelData para a matriz screenData, colocando os fantasmas e o Pac-Man em suas posições iniciais.

* private void continueLevel(): Método privado chamado quando um nível é concluído. Aumenta a pontuação, adiciona mais fantasmas e aumenta a velocidade do jogo. Em seguida, chama o método initLevel() para iniciar o próximo nível.

* public void paintComponent(Graphics g): Método sobrescrito para desenhar a tela do jogo. Ele é responsável por desenhar o labirinto, os personagens, a pontuação e a tela de introdução, de acordo com o estado atual do jogo.

* class TAdapter extends KeyAdapter: Classe interna que estende a classe KeyAdapter. É usada para lidar com os eventos de teclado do jogo. Quando o jogador pressiona as teclas de seta ou a barra de espaço, os métodos keyPressed(KeyEvent e) são chamados para atualizar a direção desejada do Pac-Man ou para iniciar o jogo quando o jogador pressiona a barra de espaço.
