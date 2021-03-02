/*
Reprodução do jogo Dots
Alice Henriques Da Rosa
90007
Realização: Abril 2018
*/
#include <SDL2/SDL.h>
#include <SDL2/SDL_ttf.h>
#include <SDL2/SDL_image.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>


#define MAX(a,b)    (((a)>(b))?(a):(b))
#define SQR(a) (a)*(a)
#define M_PI 3.14159265
#define STRING_SIZE 100       // max size for some strings
#define TABLE_SIZE 675        // main game space size
#define LEFT_BAR_SIZE 150     // left white bar size
#define WINDOW_POSX 200     // initial position of the window: x
#define WINDOW_POSY 200       // initial position of the window: y
#define SQUARE_SEPARATOR 8    // square separator in px
#define BOARD_SIZE_PER 0.7f   // board size in % wrt to table size
#define MAX_BOARD_POS 15      // maximum size of the board
#define MAX_COLORS 5
#define MARGIN 5
#define MAX_SIZE 100
#define MAX_STRING 5
#define MAX_C 2
// declaration of the functions related to graphical issues
void InitEverything(int , int , TTF_Font **, SDL_Surface **, SDL_Window ** , SDL_Renderer **, TTF_Font**);
void InitSDL();
void InitFont();
SDL_Window* CreateWindow(int , int );
SDL_Renderer* CreateRenderer(int , int , SDL_Window *);
int RenderText(int, int, const char *, TTF_Font *, SDL_Color *, SDL_Renderer *);
int RenderLogo(int, int, SDL_Surface *, SDL_Renderer *);
int RenderTable(int, int, int [], TTF_Font *, SDL_Surface **, SDL_Renderer *, int[][MAX_BOARD_POS], int, int);
void ProcessMouseEvent(int , int , int [], int , int *, int * );
void RenderPoints(int [][MAX_BOARD_POS], int, int, int [], int, SDL_Renderer *);
void RenderStats( SDL_Renderer *, TTF_Font *, int , int, int[], int *, int , SDL_Surface **, int *);
void filledCircleRGBA(SDL_Renderer * , int , int , int , int , int , int );
void askdata(int*, int*, int*, int*,int[],char[]);
void definecolors(int[][MAX_BOARD_POS], int, int, int);
void processMoves(int , int [][MAX_BOARD_POS], int, int, int[MAX_C][MAX_SIZE], int *, int[][MAX_BOARD_POS], int[][MAX_BOARD_POS], int *, int *, int *);
void playverification(int[][MAX_BOARD_POS], int, int, int[][MAX_BOARD_POS], int *, int[MAX_C][MAX_SIZE], int, int *);
int  detectfigure(int, int, int[][MAX_BOARD_POS], int [][MAX_BOARD_POS]);
void calculatelimits(int [][MAX_BOARD_POS], int, int, int[], int[]);
void eliminatefigures(int [][MAX_BOARD_POS], int, int, int[], int[], int[][MAX_BOARD_POS], int);
void descendentpoints(int [][MAX_BOARD_POS], int, int, int[][MAX_BOARD_POS], int, int, int);
int verifyshuffle(int[][MAX_BOARD_POS], int, int);
void shuffle(int [][MAX_BOARD_POS], int, int , int);
void countColorPlay(int [][MAX_BOARD_POS], int, int, int [], int[][MAX_BOARD_POS]);
void deletevector(int [][MAX_BOARD_POS], int[], int[], int [][MAX_BOARD_POS], int [][MAX_BOARD_POS], int, int, int[MAX_C][MAX_SIZE]);
void winlose(int, int, int[MAX_COLORS], int, int[], int *, int [], int);
void printstats(char[], int, int[], int[]);
void saveboard(int[][MAX_BOARD_POS], int[][MAX_BOARD_POS], int, int, int*, int);
void undo(int[][MAX_BOARD_POS], int[][MAX_BOARD_POS], int, int, int*, int, int, int, int[MAX_COLORS], int);

// definition of some strings: they cannot be changed when the program is executed !
const char myName[] = "Alice Rosa";
const char myNumber[] = "IST90007";
const int colors[3][MAX_COLORS+1] = {{255, 0, 54, 255, 253, 0},{0, 204, 61, 174, 118, 0},{0, 0, 105, 3, 144, 0}};
/**
 * main function: entry point of the program
 * only to invoke other functions !
 */
int main( void )
{
    SDL_Window *window = NULL;
    SDL_Renderer *renderer = NULL;
    TTF_Font *serif = NULL, *sans = NULL;
    SDL_Surface *imgs[5];
    SDL_Event event;
    int delay = 300;
    int quit = 0;
    int width = (TABLE_SIZE + LEFT_BAR_SIZE);
    int height = TABLE_SIZE;
    int square_size_px = 0, board_size_px[2] = {0};
    int board_pos_x = 0, board_pos_y = 0;
    int board[MAX_BOARD_POS][MAX_BOARD_POS] = {{0}};
    int pt_x = 0, pt_y = 0;
    int i=0, j=0, ncolors=0, nplay=0, init=2;
    int actualpos[MAX_BOARD_POS][MAX_BOARD_POS]={{0}};
    int buttonD[MAX_BOARD_POS][MAX_BOARD_POS]={{0}};
    int buttonU[MAX_BOARD_POS][MAX_BOARD_POS]={{0}};
    int red=0;
    int _max_vertical[MAX_BOARD_POS]={0};
    int _max_horizontal[MAX_BOARD_POS]={0};
    int goals[MAX_COLORS]={0};
    int _validation_color=0;
    int play_order[MAX_C][MAX_SIZE]={{0}};
    int lastX=0, lastY=0, saveplusone=0, playNumber=0;
    int _check=0;
    int score[MAX_SIZE]={0};
    char name[STRING_SIZE];
    int start=0;
    int printPlay=0; //Equivalente a _moves
    int shf=2;
    int iniciar=0;
    int gameinit=0;
    int vectwinlose[MAX_SIZE]={0};
    int verf=0;
    int newboard=0;
    int vectplay[MAX_SIZE]={0};
    int delay2=0;
    int vectSave[MAX_BOARD_POS][MAX_BOARD_POS]={{0}};
    int play=0;

    srand(1234);
    askdata(&i, &j, &ncolors, &nplay, goals, name);
    for (int i = 0; i < MAX_COLORS; i++)
      score[i] = goals[i];

    printPlay=nplay;
    definecolors(board, i, j, ncolors);


    board_pos_x = j;
    board_pos_y = i;


    // initialize graphics
    InitEverything(width, height, &serif, imgs, &window, &renderer, &sans);

    while( quit == 0 )
    {

        // while there's events to handle
        while( SDL_PollEvent( &event ) )
        {
            if( event.type == SDL_QUIT )
            {
                // quit !
                quit=1;
                printstats(name, gameinit, vectwinlose, vectplay);
            }
            else if ( event.type == SDL_KEYDOWN )
            {
                switch ( event.key.keysym.sym )
                {
                    case SDLK_n:
                    iniciar=1;

                    if(newboard==1) //Se reiniciar um novo jogo
                    {
                    for (int i = 0; i < MAX_COLORS; i++)
                    //Os parâmetros voltam a ser os iniciais
                    score[i] = goals[i];
                    printPlay=nplay;
                    definecolors(board, i, j, ncolors);
                    gameinit++;
                    }

                  vectwinlose[gameinit]=1; // Marcar derrota quando se carrega na tecla N
                  newboard=1;

                        break;

                    case SDLK_q:
                        printstats(name, gameinit, vectwinlose, vectplay);
                        quit=1;
                        break;

                    case SDLK_u:
                      undo(board, vectSave, i, j, &printPlay, _validation_color, playNumber, ncolors, score, play);

                    default:
                        break;
                }
            }

            if(iniciar==1)
            //Começa a jogar quando se carrega na tecla N
            {
            if ( event.type == SDL_MOUSEBUTTONDOWN )
            {
              ProcessMouseEvent(event.button.x, event.button.y, board_size_px, square_size_px, &pt_x, &pt_y);
              start=1;
              //Coloca as coordenadas nos respectivos vectores
              processMoves(start, actualpos, pt_x, pt_y, play_order, &playNumber, buttonD, buttonU, &lastX, &lastY, &saveplusone);
              //Começa a processar a jogada
              init = 1;
            }
            else if ( event.type == SDL_MOUSEBUTTONUP )
            {
                ProcessMouseEvent(event.button.x, event.button.y, board_size_px, square_size_px, &pt_x, &pt_y);
                //Acaba de processar a jogada
                init=0;
                start=3;
                processMoves(start, actualpos, pt_x, pt_y, play_order, &playNumber, buttonD, buttonU, &lastX, &lastY, &saveplusone);

                playverification(actualpos, i, j, board, &_validation_color, play_order, playNumber, &_check);
                //Se a jogada for válida, o processamento da jogada começa
                if(_check == 1)
                {
                  red = detectfigure(j, i, buttonD, buttonU);
                  //Se qualquer figura geométrica for detectada elimina essa figura
                  if(red==1)
                  {
                    calculatelimits(actualpos, j, i, _max_vertical, _max_horizontal);
                    eliminatefigures(actualpos, i, j, _max_vertical, _max_horizontal, board, _validation_color);
                  }
                  countColorPlay(actualpos, i, j, score, board);
                  saveboard(board, vectSave, i, j, &play, printPlay);
                  descendentpoints(board, i, j, actualpos, ncolors, red, _validation_color);
                  printPlay--;

                }

                shf=verifyshuffle(board, i, j);
                //Se não houver mais jogadas faz shuffle
                if(shf==1)
                {
                  shuffle(board, ncolors, i, j);
                }
                deletevector(actualpos,_max_vertical, _max_horizontal, buttonD, buttonU, i, j, play_order);
            }
            //Enquanto o init for igual a 1 passa as coordenadas para o vector
            else if ( event.type == SDL_MOUSEMOTION && init == 1 )
            {
                ProcessMouseEvent(event.button.x, event.button.y, board_size_px, square_size_px, &pt_x, &pt_y);
                start=2;
                processMoves(start, actualpos, pt_x, pt_y, play_order, &playNumber, buttonD, buttonU, &lastX, &lastY, &saveplusone);
            }

          }

          }

        // render game table
        square_size_px = RenderTable( board_pos_x, board_pos_y, board_size_px, serif, imgs, renderer, actualpos, i, j);

        winlose(printPlay, ncolors, score, gameinit, vectwinlose, &verf, vectplay, nplay);

        // render board
        RenderPoints(board, board_pos_x, board_pos_y, board_size_px, square_size_px, renderer);

        RenderStats( renderer, sans, ncolors, printPlay, score, &shf, verf, imgs, &iniciar); ///imprimir pontos e jogadas
        // render in the screen all changes above
        SDL_RenderPresent(renderer);
        //add a delay for shuffle
        if(shf==1)
        {
        delay2=2000;
       }
       SDL_Delay(delay2);
       shf=0;
        delay2=0;

        // add a delay

        SDL_Delay( delay );


    }


    // free memory allocated for images and textures and closes everything including fonts
    TTF_CloseFont(serif);
    SDL_FreeSurface(imgs[0]);
    SDL_FreeSurface(imgs[1]);
    SDL_FreeSurface(imgs[2]);
    SDL_FreeSurface(imgs[3]);
    SDL_FreeSurface(imgs[4]);
    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();
    return EXIT_SUCCESS;
}

/**
 * askdata: Pedir os dados do jogo
 * \param _i: Número de linhas
 * \param _j: Número de colunas
 * \param _ncolors: Número de cores
 * \param _nplays: Número de jogadas
 * \param goals: Número de pontos a elimininar de cada cor
 * \param _name: Nome
 */

void askdata(int *_i, int *_j , int *_ncolors, int *_nplays, int goals[], char _name[MAX_STRING])
{
  int j=0;
  //string temporária para guardar os valores
  char temp[STRING_SIZE];

  printf("Insira o seu nome(<8 letras):");
  fgets(_name,STRING_SIZE,stdin);
  while(strlen(_name)>8)
  {
    printf("Insira o seu nome(<8 letras):");
    fgets(_name,STRING_SIZE,stdin);
  }

  printf("Insira o tamanho do tabuleiro:\n");
  while(*_i<5 || *_i>15)
  {
    printf("Nº de linhas(>=5 e <=15): ");
    fgets(temp,STRING_SIZE,stdin);
    sscanf(temp,"%d", &*_i);
    temp[0]='\0';
  }

  while(*_j<5 || *_j>15)
  {
    printf("Nº de colunas(>=5 e <=15): ");
    fgets(temp,STRING_SIZE,stdin);
    sscanf(temp,"%d", &*_j);
    temp[0]='\0';
  }

  while(*_ncolors<=0 || *_ncolors>6)
  {
    printf("Insira o número de cores(<=5 cores): ");
    fgets(temp,STRING_SIZE,stdin);
    sscanf(temp,"%d", &*_ncolors);
    temp[0]='\0';
  }

  printf("Insira o número de pontos a alcançar(<= 99 pontos):\n");
  for(j=0; j<*_ncolors; j++)
  {
    while(goals[j]>100 || goals[j]<=0)
    {
      printf("Para a cor %d: ", j+1);
      fgets(temp,STRING_SIZE,stdin);
      sscanf(temp,"%d", &goals[j]);
      temp[0]='\0';
    }
  }

    printf("Insira o número de jogadas(<=99 jogadas): ");
    while(*_nplays>100 || *_nplays<=0)
    {
      fgets(temp,STRING_SIZE,stdin);
      sscanf(temp,"%d", &*_nplays);
      temp[0]='\0';
    }

  }

  /**
   * definecolors: Defenir um jogo novo
   */
  void definecolors(int _board[MAX_BOARD_POS][MAX_BOARD_POS], int _i, int _j, int _ncolors)
  {
    int n=0, m=0;

    for(n=0; n<_j; n++)
    for(m=0; m<_i; m++)
    {
      _board[n][m] = (int)(rand() % _ncolors);
    }
  }

/**
 * ProcessMoves: Processa as coordenadas das jogadas feitas, e coloco-as em vectores
 * \param start: Indica se o evento é do tipo MOUSEMOTION, MOUSEBUTTONDOWN ou MOUSEBUTTONUP
 * \param actualpos: Indica numa matriz 15x15 com o valor 2 nas coordenadas respectivas às coordenadas do quadro
 * \param pt_x e pt_y: Indica as coordenadas do rato em tempo real
 * \param play_order: Indica a ordem das jogadas. Vector 2xN em que 0 é a coordenada X e 1 é a coordenada Y e N incrementa conforme se muda de posição
 * \param playNumber: N
 * \param buttonD e buttonU: Indica a posição em que o utilizador começa(buttonD) e termina(buttonU) a jogada
 * \param lastX e last Y: A última posição que o rato passou (X e Y)
 * \param saveplusone: Guardar o N para não se perder o valor quando se muda de posição
 */

void processMoves(int start, int actualpos[][MAX_BOARD_POS], int pt_x, int pt_y, int play_order[MAX_C][MAX_SIZE], int *playNumber,
  int buttonD[][MAX_BOARD_POS], int buttonU[][MAX_BOARD_POS], int *lastX, int *lastY, int *saveplusone)
{
  int plusone=0;

  if(pt_x!=-1 && pt_y!=-1)
  {

    if(start==1)
    {
      actualpos[pt_x][pt_y]=2;
      buttonD[pt_x][pt_y]=1;
      *lastX=pt_x;
      *lastY=pt_y;
      play_order[0][0]=pt_x;
      play_order[1][0]=pt_y;
      plusone=*saveplusone;
      plusone++;
      *saveplusone=plusone;
    }

    if(start==2)
    {
      //Se a última posição que o rato passou for diferente da posição actual escreve
      if(*lastX != pt_x || *lastY != pt_y)
      {
        actualpos[pt_x][pt_y]=2;
        play_order[0][*saveplusone]=pt_x;
        play_order[1][*saveplusone]=pt_y;
        *lastX = pt_x;
        *lastY = pt_y;
        plusone=*saveplusone;
        plusone++;
        *saveplusone=plusone;
      }
   }

   if(start==3)
   {
     actualpos[pt_x][pt_y]=2;
     buttonU[pt_x][pt_y]=1;
     *playNumber=*saveplusone;
     *saveplusone=0;
     //Para recomeçar um nova jogada
   }
  }
}

/**
 * playverification: Verifica se a jogada feita é válida
 * \param validation_color: Cor da primeira bola que o utilizador selecionou
 * \param check: Se o valor for 0 a jogada é inválida e se for 1 a jogada é válida
 */

void playverification(int actualpos[][MAX_BOARD_POS], int _i, int _j, int _board[][MAX_BOARD_POS], int *validation_color,
  int play_order[MAX_C][MAX_SIZE], int playNumber, int *check)
  {
    int n=0, l=0, count=0;
    *check=1;

    for(n=0; n<playNumber-1; n++)
    // Verica se o utilizador joga fora do tabuleiro
    if(play_order[0][n]-play_order[0][n+1]>1 || play_order[1][n]-play_order[1][n+1]>1 ||
      play_order[0][n]-play_order[0][n+1]<-1 || play_order[1][n]-play_order[1][n+1]<-1)
    {
      *check=0;
    }

    for(n=0; n<playNumber-1; n++)
    //Verifica se o jogador faz diagonais
    if(play_order[0][n]!=play_order[0][n+1] && play_order[1][n]!=play_order[1][n+1])
    {
      *check=0;
    }

    for(n=0; n<playNumber-2; n++)
    //Verifica se o jogador voltou para trás
      if(play_order[0][n]==play_order[0][n+2] && play_order[1][n]==play_order[1][n+2])
      {
        *check=0;
      }

    for(n=0; n<_i;n++)
      for(l=0; l<_j; l++)
      {
        if(actualpos[l][n]==2)
        {
          *validation_color=_board[l][n];
          count++;
        }
      }

    for(n=0; n<_i; n++)
      for(l=0; l<_j; l++)
      {
        //Verifica se o jogador está a selecionar pontos da mesma cor e se o utilizador selecionou mais que um ponto
        if((actualpos[l][n]==2 && _board[l][n]!=*validation_color) || count==1)
          *check=0;
      }
  }

  /**
   * detectfigure: Processa as coordenadas das jogadas feitas, e coloco-as em vectores
   * \param _buttonD e _buttonU: Necessários para verificar se a primeira e última posições são iguais
   */

  int detectfigure(int _j, int _i, int _buttonD[][MAX_BOARD_POS], int _buttonU[][MAX_BOARD_POS])
  {
    for(int m=0; m<_i; m++)
    for(int n=0; n<_j; n++)
    {
      // Se a primeira e a última posição forem iguais, o utilizador faz um quadrado
      if(_buttonD[n][m]==1 && _buttonU[n][m]==1)
      return 1;
    }

    return 0;
  }

  /**
   * calculatelimits: Calcula os limites da figura
   * \param max_vertical: O último ponto carregado na coluna
   * \param max_horizontal: O último ponto carragado na linha
   */
  void calculatelimits(int actualpos[][MAX_BOARD_POS], int _j, int _i, int max_vertical[MAX_BOARD_POS], int max_horizontal[MAX_BOARD_POS])
  {

    for(int m=0; m<_j; m++)
    for(int n=0; n<_i; n++)
    {
      //Se o rato passou por lá e o número da coluna é maior que a anterior, guarda esse valor
      if(actualpos[m][n]==2)
      max_vertical[m]=n;
    }

    for(int m=0; m<_i; m++)
    for(int n=0; n<_j; n++)
    {
      //Se o rato passou por lá e o número da linha é maior que a anterior, guarda esse valor
      if(actualpos[n][m]==2);
      max_horizontal[m]=n;
    }
  }

  /**
   * eliminatefigures: Elimina a figura
   * \param max_vertical e max_horizontal: Utilizados para limitar o ciclo for
   * \param _board: Utilizada para eliminar todos os pontos da mesma cor da figura
   * \param validation_color: cor da figura
   */

  void eliminatefigures(int actualpos[][MAX_BOARD_POS], int _i, int _j, int max_vertical[], int max_horizontal[],
    int _board[][MAX_BOARD_POS], int validation_color)
    {
      int m=0, n=0, init=0;
      int vector_vertical[MAX_BOARD_POS][MAX_BOARD_POS]={{0}};
      int vector_horizontal[MAX_BOARD_POS][MAX_BOARD_POS]={{0}};

      for(m=0; m<_j; m++)
      for(n=0; n<max_vertical[m]; n++)
      {
        //Quando encontra um ponto selecionado nessa coluna começa a preencher
        if(actualpos[m][n]==2)
        {
          init=1;
        }
        if(init==1)
        {
          //preenche o vector com o valor 3
          vector_vertical[m][n]=3;
        }
      }
      init=0;

      for(m=0; m<_i; m++)
      for(n=0; n<max_horizontal[m]; n++)
      {
        //Quando encontra um ponto selecionado nessa linha começa a preencher
        if(actualpos[n][m]==2)
        {
          init=1;
        }
        //Preenche o vector com o valor 4
        if(init==1)
        {
          vector_horizontal[n][m]=4;
        }
      }


      for(m=0; m<_j; m++)
      for(n=0; n<_i; n++)
      {
        //A interseção das linhas e das colunas resulta na figura selecionada pelo utilizador
        if( vector_horizontal[m][n]==4 && vector_vertical[m][n]==3)
        {
          actualpos[m][n]=2;
        }
        //Se houver pontos da cor da figura, estes são eliminados do quadro
        if(_board[m][n]==validation_color)
        {
          actualpos[m][n]=2;
        }
      }

    }

    /**
     * saveboard: Salvar o vector Board todas as jogadas
     * \param vectSave: Vector onde se guarda a board actual
     * \param play: pointer onde guarda o número da jogada
     * \param printPlay: Variável que têm o número da jogada actual
     */

void saveboard(int board[][MAX_BOARD_POS], int vectSave[][MAX_BOARD_POS], int i, int j, int *play, int printPlay)
{

  for(int n=0; n<i; n++)
   for(int m=0; m<j; m++)
    {
      vectSave[m][n]=board[m][n];
    }

  *play=printPlay;
}

/**
* descendentpoints: Elimina os pontos, faz os pontos acima dos eliminados descerem no quadro e gera novos pontos
* \param red: Variável que verifica se o utilizador fez um quadrado
*/
void descendentpoints(int _board[][MAX_BOARD_POS], int _i, int _j, int actualpos[][MAX_BOARD_POS], int _ncolors, int red, int validation_color)
  {
    int columnvector[MAX_BOARD_POS]={0};
    int lastpoint[MAX_BOARD_POS]={0};
    int m=0, n=0;
    int count=0;
    srand(time(NULL));

    //Conta a quantidade de pontos selecionados em cada coluna(columnvector) e o último selecionado(lastpoint)
    for(m=0; m<_j; m++)
      for(n=0; n<_i; n++)
       {
        if(actualpos[m][n]==2)
          {
            columnvector[m]++;
            lastpoint[m]=n;
          }
        }

      for(m=0; m<_j; m++)
      {
        for(n=lastpoint[m]; n>=0; n--)
        {
          //Conta os pontos selecionados
          if(actualpos[m][n]==2)
          {
            count++;
          }
          //Se o ponto não for selecionado, a cor do ponto que foi "eliminado" n vezes abaixo dele vai ficar com a mesma cor deste
          else if(actualpos[m][n]==0)
          {
            _board[m][n+count]=_board[m][n];
          }
        }
        count=0;
      }

      for(m=0; m<_j; m++)
        for(n=0; n<columnvector[m]; n++)
        {
          //Gera novos pontos
          _board[m][n]=(int)(rand() % _ncolors);
          //Se o jogador fez um quadrado, continua a gerar cores até gerar uma diferente da cor do quadrado selecionado
          while(red==1 && _board[m][n]==validation_color)
          {
            _board[m][n]=(int)(rand() % _ncolors);
          }
        }

    }

    /**
     * verifyshuffle: Verifica se não existem mais jogadas
     * \param board: Para a verificação das cores adjacentes
     */
int verifyshuffle(int board[][MAX_BOARD_POS], int i, int j)
{

  for(int m=0; m<i; m++)
   for(int n=1; n<j; n++)
   {
     //A partir do momento que encontra duas cores iguais adjacentes na mesma coluna não se verifica o shuffle
     if(board[n][m]==board[n-1][m])
       return 0;
   }

   for(int m=0; m<j; m++)
    for(int n=1; n<i; n++)
    {
      //A partir do momento que encontra duas cores iguais adjacentes na mesma linha não se verifica o shuffle
      if(board[m][n]==board[m][n-1])
        return 0;
    }
  //Se não se verificar nenhuma das condições, é shuffle
   return 1;
}

/**
 * shuffle: Realização do shuffle
 * \param board: Dar novos valores ao vector
 */

void shuffle(int board[][MAX_BOARD_POS], int ncolors, int i, int j)
{
  int scolor[MAX_COLORS]={0}; //vector onde se guarda o número de pontos de cada cor
  int k=0;

   //Contar o número de pontos na altura do shuffle
   for(int n=0; n<i; n++)
    for(int m=0; m<j; m++)
     {
        switch(board[m][n]){
          case 0:
          scolor[0]++; break;

          case 1:
          scolor[1]++; break;

          case 2:
          scolor[2]++; break;

          case 3:
          scolor[3]++; break;

          default:
          scolor[4]++;
        }

      }

  for(int n=0; n<i; n++)
   for(int m=0; m<j; m++)
   {
     k = rand() % ncolors;
     //Se não houver mais pontos a utilizar dessa cor, continua a fazer random de outra cor
     while(scolor[k]==0)
     {
     k = rand() % ncolors;
     }

     board[m][n]=k;
     scolor[k]--;
     }

   }

   /**
    * deletevector: Limpa vetores e variáveis para a próxima jogada
    */
void deletevector(int actualpos[][MAX_BOARD_POS], int max_vertical[MAX_BOARD_POS], int max_horizontal[MAX_BOARD_POS],
 int _buttonD[][MAX_BOARD_POS], int _buttonU[][MAX_BOARD_POS], int _i, int  _j, int play_order[MAX_C][MAX_SIZE])
{
 int n=0, m=0;

  for( n=0; n<_j; n++)
    {
        for(m=0; m<_i; m++)
            {
              actualpos[n][m]=0;
              _buttonD[n][m]=0;
              _buttonU[n][m]=0;
              max_vertical[m]=0;
                }
                   max_horizontal[n]=0;
                  }

    for(n=0; n<MAX_SIZE; n++)
    {
      for(m=0; m<2; m++)
      {
       play_order[m][n]=0;
      }
    }

}

/**
* countColorPlay: Conta os pontos que faltam eliminar em cada jogada
* \param score: Guarda os pontos a eliminar de cada cor no decorrer do jogo
*/

void countColorPlay(int actualpos[][MAX_BOARD_POS], int _i, int _j, int score[], int board[][MAX_BOARD_POS])
{
  for(int n=0; n<_i; n++)
   for(int m=0; m<_j; m++)
   {
    if(actualpos[m][n]==2)
    {
      switch(board[m][n]){

        case 0:
        score[0]--;
        break;

        case 1:
        score[1]--; break;

        case 2:
        score[2]--; break;

        case 3:
        score[3]--; break;

        case 4:
        score[4]--; break;

      }

    }
  }
}

/**
 * RenderStats: Mostrar alguma informação sobre o jogo
 * \param _ncolors: número de cores no tabuleiro
 * \param height: _moves Número de jogadas
 * \param _font: Letra do texto
 * \param _img: Imagens que vão ser geradas quando se ganha, perde ou se faz shuffle
 * \param score: Vetor onde estão guardados os pontos a eliminar no quadro a cada momento
 * \param shf: variável cujo valor indica se é shuffle ou não
 * \param verf: variável que indica se é uma vitória ou derrota
 * \param iniciar: Não deixa jogar enquanto não se carrega na tecla N.
 */
void RenderStats( SDL_Renderer *_renderer, TTF_Font *_font, int _ncolors, int _moves, int score[MAX_COLORS], int *shf, int verf,
  SDL_Surface *_img[], int *iniciar)
  {
    SDL_Color black = { 0, 0, 0 };
    SDL_Color light = { 205, 193, 181 };
    SDL_Rect square_colors;
    SDL_Rect uwin_image;
    SDL_Rect ulose_image;
    SDL_Rect shuffle_image;
    SDL_Texture *youwin;
    SDL_Texture *youlose;
    SDL_Texture *shuffle;
    int circleX=0, circleY=0, circleR=0, clr=0, i=0;
    char string_goals[MAX_STRING];
    char string_moves[MAX_STRING];


    SDL_SetRenderDrawColor(_renderer, light.r, light.g, light.b, light.a );
    square_colors.x = 300;
    square_colors.y = 30;
    square_colors.w = 90;
    square_colors.h = 50;
    SDL_RenderFillRect(_renderer, &square_colors);

    SDL_SetRenderDrawColor(_renderer, light.r, light.g, light.b, light.a );
    for ( i = 0; i < _ncolors; i++ )
    {
      //Quanto maior o número de cores que tenho menor vai ser o espaço entre os rectângulos e menor vai ser a distância X
      square_colors.x = (int)(300/_ncolors) + i*((int)(600/_ncolors));
      square_colors.y = 110;
      square_colors.w = 90;
      square_colors.h = 50;
      SDL_RenderFillRect(_renderer, &square_colors);
    }

    square_colors.x=(int)(300/_ncolors);

    for ( i = 0; i < _ncolors; i++ )
    {
      circleX = square_colors.x+ 20 + i*((int)(600/_ncolors));
      circleY = 135;
      circleR = 12;
      clr = i;
      filledCircleRGBA(_renderer, circleX, circleY, circleR, colors[0][clr], colors[1][clr], colors[2][clr]);
    }
    circleX=square_colors.x+ 20;

    if(_moves<0)
    {
      _moves=0;
    }
    sprintf(string_moves, "%d", _moves);
    RenderText(335 , 35 , string_moves, _font, &black, _renderer);

    for( i=0; i<_ncolors; i++)
    {
      if(score[i]<0)
      {
        score[i]=0;
      }
      sprintf(string_goals, "%d", score[i]);
      RenderText(circleX+circleR+15+(i*((int)(600/_ncolors))), circleY-18, string_goals, _font, &black, _renderer);

    }

    if(*shf==1)
    {
      shuffle_image.x = 150;
      shuffle_image.y = 150;
      shuffle_image.w = 378;
      shuffle_image.h = 378;
      shuffle = SDL_CreateTextureFromSurface(_renderer, _img[4]);
      SDL_RenderCopy(_renderer, shuffle, NULL, &shuffle_image);
    }

    //Se for vitória, imprimir a imagem
    if(verf==2)
    {
      uwin_image.x = 90;
      uwin_image.y = 250;
      uwin_image.w = 512;
      uwin_image.h = 276;
      youwin = SDL_CreateTextureFromSurface(_renderer, _img[2]);
      SDL_RenderCopy(_renderer, youwin, NULL, &uwin_image);
      *iniciar=0;
    }
    //se for derrota, imprimir a imagem
    else if(verf==1)
    {
      ulose_image.x = 155;
      ulose_image.y = 50;
      ulose_image.w = 337;
      ulose_image.h = 484;
      youlose = SDL_CreateTextureFromSurface(_renderer, _img[3]);
      SDL_RenderCopy(_renderer, youlose, NULL, &ulose_image);
      *iniciar=0;
    }

  }

  /**
   * winlose: Verifica se o jogador ganhou ou perdeu
   * \param _moves: Número de jogadas restantes
   * \param _ncolors: Número de cores
   * \param gameinit: Número do jogo em que o utilizador se encontra
   * \param vectwinlose: Guardar em cada jogo o valor 1 que corresponde a derrota ou 2 que corresponde a vitória
   * \param verf: pointer que indica se o jogador perdeu ou ganhou o jogo
   * \param vectplay: Número de jogadas utilizadas para obter a vitória
   * \param nplay: Número de jogadas disponíveis
   */
  void winlose(int _moves, int _ncolors, int score[MAX_COLORS], int gameinit, int vectwinlose[], int *verf, int vectplay[], int nplay)
  {
    //conta o número de cores que não existem mais pontos a eliminar
    int count=0;

    for(int i=0; i<_ncolors; i++)
    {
      if(score[i]==0)
      {
        count++;
      }
    }

    if(count==_ncolors){
      *verf=2;
      vectwinlose[gameinit]=2;
      vectplay[gameinit]=nplay - _moves;
    }
    else if(_moves==0 && count!=_ncolors){
      *verf=1;
      vectwinlose[gameinit]=1;
    }
    else
    {
      *verf=3;
    }
  }

  /**
   * printstats: Imprimir as estatísticas no ficheiro
   * \param name: nome do jogador
   * \param gameinit: número do jogo
   * \param vectwinlose: Guardar em cada jogo o valor 1 que corresponde a derrota ou 2 que corresponde a vitória
   * \param vectplay: Número de jogadas utilizadas para obter a vitória
   */
  void printstats( char name[], int gameinit, int vectwinlose[], int vectplay[])
  {
    int i=0, d=0, w=0;
    FILE *fp =NULL;

    fp=fopen("stats.txt", "w");

    for(i=0; i<strlen(name); i++)
    {
      fprintf(fp, "%c", name[i]);
    }
    fprintf(fp, "Número total de jogos: %d\n", gameinit+1);

    for(i=0; i <= gameinit;i++)
    {
      if(vectwinlose[i]==1)
      {
        fprintf(fp, "Jogo %d: D\n", i+1);
        d++;
      }

      else if(vectwinlose[i]==2)
      {
        fprintf(fp, "Jogo %d: %d V\n", i+1, vectplay[i]);
        w++;
      }

    }

    fprintf(fp, "Derrotas: %d\n", d);
    fprintf(fp, "Vitórias: %d\n", w);


    fclose(fp);
  }

/**
 * ProcessMouseEvent: gets the square pos based on the click positions !
 * \param _mouse_pos_x position of the click on pixel coordinates
 * \param _mouse_pos_y position of the click on pixel coordinates
 * \param _board_size_px size of the board !
 * \param _square_size_px size of each square
 * \param _pt_x square nr
 * \param _pt_y square nr
 */
void ProcessMouseEvent(int _mouse_pos_x, int _mouse_pos_y, int _board_size_px[], int _square_size_px,
        int *_pt_x, int *_pt_y )
{
    int sqr_x=0, sqr_y  =0, dist=0, circleX=0, circleY=0, circleR=0;
    // corner of the board
    int x_corner = (TABLE_SIZE - _board_size_px[0]) >> 1;
    int y_corner = (TABLE_SIZE - _board_size_px[1] - 15);

    // verify if valid cordinates
    if (_mouse_pos_x < x_corner || _mouse_pos_y < y_corner || _mouse_pos_x > (x_corner + _board_size_px[0])
        || _mouse_pos_y > (y_corner + _board_size_px[1]) )
    {
        *_pt_x = -1;
        *_pt_y = -1;
        return;
    }

    // computes the square where the mouse position is
    sqr_x = (_mouse_pos_x - x_corner) / (_square_size_px + SQUARE_SEPARATOR);
    sqr_y = (_mouse_pos_y - y_corner) / (_square_size_px + SQUARE_SEPARATOR);

    //compute the circle
    circleX = x_corner + (sqr_x+1)*SQUARE_SEPARATOR + sqr_x*(_square_size_px)+(_square_size_px>>1);
    circleY = y_corner + (sqr_y+1)*SQUARE_SEPARATOR + sqr_y*(_square_size_px)+(_square_size_px>>1);
    circleR = (int)(_square_size_px*0.4f);

    dist = (int)floor(sqrt( SQR((_mouse_pos_x - circleX)) + SQR((_mouse_pos_y - circleY )) ) );
    //
    if(dist < circleR)
    {
     *_pt_x=sqr_x;
     *_pt_y = sqr_y;
    }
   else
   {
     *_pt_x = -1;
     *_pt_y = -1;
   }
}


/**
 * RenderPoints: renders the board
 * \param _board 2D array with integers representing board colors
 * \param _board_pos_x number of positions in the board (x axis)
 * \param _board_pos_y number of positions in the board (y axis)
 * \param _square_size_px size of each square
 * \param _board_size_px size of the board
 * \param _renderer renderer to handle all rendering in a window
 */
void RenderPoints(int _board[][MAX_BOARD_POS], int _board_pos_x, int _board_pos_y,
        int _board_size_px[], int _square_size_px, SDL_Renderer *_renderer )
{
    int clr, x_corner, y_corner, circleX, circleY, circleR;

    // corner of the board
    x_corner = (TABLE_SIZE - _board_size_px[0]) >> 1;
    y_corner = (TABLE_SIZE - _board_size_px[1] - 15);

    // renders the squares where the dots will appear
    for ( int i = 0; i < _board_pos_x; i++ )
    {
        for ( int j = 0; j < _board_pos_y; j++ )
        {

                // define the size and copy the image to display
                circleX = x_corner + (i+1)*SQUARE_SEPARATOR + i*(_square_size_px)+(_square_size_px>>1);
                circleY = y_corner + (j+1)*SQUARE_SEPARATOR + j*(_square_size_px)+(_square_size_px>>1);
                circleR = (int)(_square_size_px*0.4f);
                // draw a circle
                clr = _board[i][j];
                filledCircleRGBA(_renderer, circleX, circleY, circleR, colors[0][clr], colors[1][clr], colors[2][clr]);
        }
    }


}

/**
* filledCircleRGBA: renders a filled circle
* \param _circleX x pos
* \param _circleY y pos
* \param _circleR radius
* \param _r red
* \param _g gree
* \param _b blue
*/
void filledCircleRGBA(SDL_Renderer * _renderer, int _circleX, int _circleY, int _circleR, int _r, int _g, int _b)
{
  int off_x = 0;
  int off_y = 0;
  float degree = 0.0;
  float step = M_PI / (_circleR*8);

  SDL_SetRenderDrawColor(_renderer, _r, _g, _b, 255);

  while (_circleR > 0)
  {
    for (degree = 0.0; degree < M_PI/2; degree+=step)
    {
      off_x = (int)(_circleR * cos(degree));
      off_y = (int)(_circleR * sin(degree));
      SDL_RenderDrawPoint(_renderer, _circleX+off_x, _circleY+off_y);
      SDL_RenderDrawPoint(_renderer, _circleX-off_y, _circleY+off_x);
      SDL_RenderDrawPoint(_renderer, _circleX-off_x, _circleY-off_y);
      SDL_RenderDrawPoint(_renderer, _circleX+off_y, _circleY-off_x);
    }
    _circleR--;
  }
}

/*
 * RenderTable: Draws the table where the game will be played, namely:
 * -  some texture for the background
 * -  the right part with the IST logo and the student name and number
 * -  the grid for game board with squares and seperator lines
 * \param _board_pos_x number of positions in the board (x axis)
 * \param _board_pos_y number of positions in the board (y axis)
 * \param _board_size_px size of the board
 * \param _font font used to render the text
 * \param _img surfaces with the table background and IST logo (already loaded)
 * \param _renderer renderer to handle all rendering in a window
 */
int RenderTable( int _board_pos_x, int _board_pos_y, int _board_size_px[],
        TTF_Font *_font, SDL_Surface *_img[], SDL_Renderer* _renderer, int actualpos[][MAX_BOARD_POS], int i, int j)
{
    SDL_Color black = { 0, 0, 0 }; // black
    SDL_Color light = { 205, 193, 181 };
    SDL_Color dark = { 120, 110, 102 };
    SDL_Color lighty={102 , 0 , 0};
    SDL_Texture *table_texture;
    SDL_Rect tableSrc, tableDest, board, board_square;
    int height, board_size, square_size_px, max_pos;

    // set color of renderer to some color
    SDL_SetRenderDrawColor( _renderer, 255, 255, 255, 255 );

    // clear the window
    SDL_RenderClear( _renderer );

    tableDest.x = tableSrc.x = 0;
    tableDest.y = tableSrc.y = 0;
    tableSrc.w = _img[0]->w;
    tableSrc.h = _img[0]->h;
    tableDest.w = TABLE_SIZE;
    tableDest.h = TABLE_SIZE;

    // draws the table texture
    table_texture = SDL_CreateTextureFromSurface(_renderer, _img[0]);
    SDL_RenderCopy(_renderer, table_texture, &tableSrc, &tableDest);

    // render the IST Logo
    height = RenderLogo(TABLE_SIZE, 0, _img[1], _renderer);

    // render the student name
    height += RenderText(TABLE_SIZE+3*MARGIN, height, myName, _font, &black, _renderer);

    // this renders the student number
    RenderText(TABLE_SIZE+3*MARGIN, height, myNumber, _font, &black, _renderer);

    // compute and adjust the size of the table and squares
    max_pos = MAX(_board_pos_x, _board_pos_y);
    board_size = (int)(BOARD_SIZE_PER*TABLE_SIZE);
    square_size_px = (board_size - (max_pos+1)*SQUARE_SEPARATOR) / max_pos;
    _board_size_px[0] = _board_pos_x*(square_size_px+SQUARE_SEPARATOR)+SQUARE_SEPARATOR;
    _board_size_px[1] = _board_pos_y*(square_size_px+SQUARE_SEPARATOR)+SQUARE_SEPARATOR;

    // renders the entire board background
    SDL_SetRenderDrawColor(_renderer, dark.r, dark.g, dark.b, dark.a );
    board.x = (TABLE_SIZE - _board_size_px[0]) >> 1;
    board.y = (TABLE_SIZE - _board_size_px[1] - 15);
    board.w = _board_size_px[0];
    board.h = _board_size_px[1];
    SDL_RenderFillRect(_renderer, &board);

    // renders the squares where the numbers will appear
    SDL_SetRenderDrawColor(_renderer, light.r, light.g, light.b, light.a );


    // iterate over all squares
    for ( int i = 0; i < _board_pos_x; i++ )
    {
        for ( int j = 0; j < _board_pos_y; j++ )
        {
            if(actualpos[i][j]==2)
            SDL_SetRenderDrawColor(_renderer, lighty.r, lighty.g, lighty.b, lighty.a);

            board_square.x = board.x + (i+1)*SQUARE_SEPARATOR + i*square_size_px;
            board_square.y = board.y + (j+1)*SQUARE_SEPARATOR + j*square_size_px;
            board_square.w = square_size_px;
            board_square.h = square_size_px;
            SDL_RenderFillRect(_renderer, &board_square);

            SDL_SetRenderDrawColor(_renderer, light.r, light.g, light.b, light.a );

        }
    }

    // destroy everything
    SDL_DestroyTexture(table_texture);
    // return for later use
    return square_size_px;
}

/**
 * undo function: Volta uma jogada para trás
 * \param board: Para voltar a colocar as cores que estavam
 * \param vectSave: Vector onde está guardado a board
 * \param moves: vector que vai ser imprimido no ecrã
 * \param play: variável onde está guardado o número da jogada antes do undo
 * \param score:Guarda os pontos a eliminar de cada cor no decorrer do jogo
 */
void undo(int board[][MAX_BOARD_POS], int vectSave[][MAX_BOARD_POS], int i, int j, int *moves, int validation_color,
  int playNumber, int _ncolors, int score[MAX_COLORS], int play)
{
  for(int n=0; n<i; n++){
   for(int m=0; m<j; m++)
   {
     board[m][n]=vectSave[m][n];
   }
 }

   for(int i=0; i<_ncolors; i++)
     if(i==validation_color)
     {
       score[i]=score[i]+playNumber;
     }

   *moves=play;
}

/**
 * RenderLogo function: Renders the IST logo on the app window
 * \param x X coordinate of the Logo
 * \param y Y coordinate of the Logo
 * \param _logoIST surface with the IST logo image to render
 * \param _renderer renderer to handle all rendering in a window
 */
int RenderLogo(int x, int y, SDL_Surface *_logoIST, SDL_Renderer* _renderer)
{
    SDL_Texture *text_IST;
    SDL_Rect boardPos;

    // space occupied by the logo
    boardPos.x = x;
    boardPos.y = y;
    boardPos.w = _logoIST->w;
    boardPos.h = _logoIST->h;

    // render it
    text_IST = SDL_CreateTextureFromSurface(_renderer, _logoIST);
    SDL_RenderCopy(_renderer, text_IST, NULL, &boardPos);

    // destroy associated texture !
    SDL_DestroyTexture(text_IST);
    return _logoIST->h;
}

/**
 * RenderText function: Renders some text on a position inside the app window
 * \param x X coordinate of the text
 * \param y Y coordinate of the text
 * \param text string with the text to be written
 * \param _font TTF font used to render the text
 * \param _color color of the text
 * \param _renderer renderer to handle all rendering in a window
 */
int RenderText(int x, int y, const char *text, TTF_Font *_font, SDL_Color *_color, SDL_Renderer* _renderer)
{
    SDL_Surface *text_surface;
    SDL_Texture *text_texture;
    SDL_Rect solidRect;

    solidRect.x = x;
    solidRect.y = y;
    // create a surface from the string text with a predefined font
    text_surface = TTF_RenderText_Blended(_font,text,*_color);
    if(!text_surface)
    {
        printf("TTF_RenderText_Blended: %s\n", TTF_GetError());
        exit(EXIT_FAILURE);
    }
    // create texture
    text_texture = SDL_CreateTextureFromSurface(_renderer, text_surface);
    // obtain size
    SDL_QueryTexture( text_texture, NULL, NULL, &solidRect.w, &solidRect.h );
    // render it !
    SDL_RenderCopy(_renderer, text_texture, NULL, &solidRect);
    // clear memory
    SDL_DestroyTexture(text_texture);
    SDL_FreeSurface(text_surface);
    return solidRect.h;
}

/**
 * InitEverything: Initializes the SDL2 library and all graphical components: font, window, renderer
 * \param width width in px of the window
 * \param height height in px of the window
 * \param _font font that will be used to render the text
 * \param _img surface to be created with the table background and IST logo
 * \param _window represents the window of the application
 * \param _renderer renderer to handle all rendering in a window
 */
void InitEverything(int width, int height, TTF_Font **_font, SDL_Surface *_img[], SDL_Window** _window, SDL_Renderer** _renderer, TTF_Font** sans)
{
    InitSDL();
    InitFont();
    *_window = CreateWindow(width, height);
    *_renderer = CreateRenderer(width, height, *_window);

    // load the table texture
    _img[0] = IMG_Load("table_texture.png");
    if (_img[0] == NULL)
    {
        printf("Unable to load image: %s\n", SDL_GetError());
        exit(EXIT_FAILURE);
    }

    // load IST logo
    _img[1] = SDL_LoadBMP("ist_logo.bmp");
    if (_img[1] == NULL)
    {
        printf("Unable to load bitmap: %s\n", SDL_GetError());
        exit(EXIT_FAILURE);
    }

    //load Win image
    _img[2] = IMG_Load("uwin.jpg");
    if (_img[2] == NULL)
    {
        printf("Unable to load bitmap: %s\n", SDL_GetError());
        exit(EXIT_FAILURE);
    }

    //load lost image
    _img[3] = IMG_Load("ulose.jpg");
    if (_img[3] == NULL)
    {
        printf("Unable to load bitmap: %s\n", SDL_GetError());
        exit(EXIT_FAILURE);
    }

    //load shuffle image
    _img[4] = IMG_Load("shuffle.png");
    if (_img[1] == NULL)
    {
        printf("Unable to load bitmap: %s\n", SDL_GetError());
        exit(EXIT_FAILURE);
    }
    // this opens (loads) a font file and sets a size
    *_font = TTF_OpenFont("FreeSerif.ttf", 16);
    if(!*_font)
    {
        printf("TTF_OpenFont: %s\n", TTF_GetError());
        exit(EXIT_FAILURE);
    }

    *sans = TTF_OpenFont("OpenSans.ttf", 28);
    if(!*sans)
    {
        printf("TTF_OpenFont: %s\n", TTF_GetError());
        exit(EXIT_FAILURE);
    }

}

/**
 * InitSDL: Initializes the SDL2 graphic library
 */
void InitSDL()
{
    // init SDL library
    if ( SDL_Init( SDL_INIT_EVERYTHING ) == -1 )
    {
        printf(" Failed to initialize SDL : %s\n", SDL_GetError());
        exit(EXIT_FAILURE);
    }
}

/**
 * InitFont: Initializes the SDL2_ttf font library
 */
void InitFont()
{
    // Init font library
    if(TTF_Init()==-1)
    {
        printf("TTF_Init: %s\n", TTF_GetError());
        exit(EXIT_FAILURE);
    }
}

/**
 * CreateWindow: Creates a window for the application
 * \param width width in px of the window
 * \param height height in px of the window
 * \return pointer to the window created
 */
SDL_Window* CreateWindow(int width, int height)
{
    SDL_Window *window;
    // init window
    window = SDL_CreateWindow( "IST Dots", WINDOW_POSX, WINDOW_POSY, width, height, 0 );
    // check for error !
    if ( window == NULL )
    {
        printf("Failed to create window : %s\n", SDL_GetError());
        exit(EXIT_FAILURE);
    }
    return window;
}

/**
 * CreateRenderer: Creates a renderer for the application
 * \param width width in px of the window
 * \param height height in px of the window
 * \param _window represents the window for which the renderer is associated
 * \return pointer to the renderer created
 */
SDL_Renderer* CreateRenderer(int width, int height, SDL_Window *_window)
{
    SDL_Renderer *renderer;
    // init renderer
    renderer = SDL_CreateRenderer( _window, -1, 0 );

    if ( renderer == NULL )
    {
        printf("Failed to create renderer : %s", SDL_GetError());
        exit(EXIT_FAILURE);
    }

    // set size of renderer to the same as window
    SDL_RenderSetLogicalSize( renderer, width, height );

    return renderer;
}
