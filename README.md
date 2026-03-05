/* Prototipo mínimo para desarrollo de videojuego
Objetivos:
- Mostrar un mapa fijo en consola por medio de ASCII
- Dibujar al jugador @ en una posición (x, y)
- Leer comandos WASD para mover al jugador
- Evitar que el jugador atraviese paredes
- Recolectar monedas
*/

#include <stdio.h>
#include <string.h>

#define ALTO 10
#define ANCHO 20

/* Mapa fijo */
static const char *MAPA_FIJO[ALTO] = {
    "####################",
    "#.............*....#",
    "#.######.########..#",
    "#.#....#.#......#..#",
    "#.#.##.#.#.####.#..#",
    "#...##...#....#...#",
    "#####.#########.###",
    "#.....#.......#...#",
    "#.#####.#####.###.#",
    "####################"
};

static void consola_limpiar_simple(void)
{
    for (int i = 0; i < 40; i++)
    {
        printf("\n");
    }
}

/* Verificar si hay pared */
static int es_pared(const char mapa[ALTO][ANCHO + 1], int x, int y)
{
    if (x < 0 || x >= ANCHO || y < 0 || y >= ALTO)
        return 1;

    if (mapa[y][x] == '#')
        return 1;

    return 0;
}

/* Dibujar mapa */
static void renderizar(const char mapa[ALTO][ANCHO + 1], int jugador_x, int jugador_y, int pasos, int monedas)
{
    consola_limpiar_simple();

    printf("Juego de Laberinto\n");
    printf("Pasos: %d | Monedas: %d\n\n", pasos, monedas);

    /* BUCLES ANIDADOS */
    for (int y = 0; y < ALTO; y++)
    {
        for (int x = 0; x < ANCHO; x++)
        {
            if (x == jugador_x && y == jugador_y)
                putchar('@');
            else
                putchar(mapa[y][x]);
        }
        putchar('\n');
    }

    puts("\nControles: W A S D + Enter | Q para salir");
}

int main()
{
    char mapa[ALTO][ANCHO + 1];

    /* Copiar mapa */
    for (int y = 0; y < ALTO; y++)
    {
        strncpy(mapa[y], MAPA_FIJO[y], ANCHO + 1);
    }

    /* VARIABLES DEL JUGADOR */
    int xJugador = 1;
    int yJugador = 1;

    int pasos = 0;
    int monedas = 0;

    int nx, ny;

    char tecla;

    while (1)
    {
        renderizar(mapa, xJugador, yJugador, pasos, monedas);

        printf("> ");
        scanf(" %c", &tecla);

        if (tecla == 'q' || tecla == 'Q')
            break;

        int dx = 0, dy = 0;

        /* CONDICIONALES */
        if (tecla == 'w' || tecla == 'W')
            dy = -1;
        else if (tecla == 's' || tecla == 'S')
            dy = 1;
        else if (tecla == 'a' || tecla == 'A')
            dx = -1;
        else if (tecla == 'd' || tecla == 'D')
            dx = 1;

        nx = xJugador + dx;
        ny = yJugador + dy;

        if (!es_pared(mapa, nx, ny))
        {
            /* Si hay moneda */
            if (mapa[ny][nx] == '*')
            {
                monedas++;
                mapa[ny][nx] = '.';
                printf("¡Moneda recogida!\n");
            }

            xJugador = nx;
            yJugador = ny;
            pasos++;
        }
        else
        {
            printf("¡No puedes atravesar paredes!\n");
        }
    }

    printf("\nJuego terminado\n");
    printf("Pasos dados: %d\n", pasos);
    printf("Monedas recogidas: %d\n", monedas);

    return 0;
}
