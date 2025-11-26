This project will create a C program that manages a Rubik’s Cube speed-solving tournament. The program will allow the user to enter player information, record their solve times, and automatically calculate each player’s AO5 (Average of 5). After collecting all data, the program will display each player’s results, rank all participants based on their AO5, and determine the 1st, 2nd, and 3rd place winners. This program solves the problem of manually tracking competition data and ensures accurate and quick ranking.

Main features:

1. Add player information (name and ID).
2. Enter five solve times for each player.
3. Auto-calculate AO5 according to standard rules (best & worst removed, average of remaining 3).
4. Display all player results in a clean table.
5. Rank players based on AO5 and show top 3 winners.
6. Save all results to a file for later viewing.

Concepts

Structures- to store player names, solve times, and AO5.
dynamic Memory Allocation-(`malloc`) to create arrays for any number of players.
Pointers-for passing structures to functions and sorting.
Functions-for input, calculation, sorting, and display.
file Handling- to save and load tournament results.

Expected Output / User Interface

The program will have a text-based menu where the user enters the number of players, inputs their times, and views results. After all data is entered, the program will show a formatted list of players with their AO5 values and display the top 3 winners. The final results will also be written to a text file.


#include <stdio.h>
#include <stdlib.h>

typedef struct {
    char name[50];
    float times[5];
    float ao5;
} Player;

float calculateAO5(float t[]) {
    float min = t[0], max = t[0], sum = 0;
    for (int i = 0; i < 5; i++) {
        sum += t[i];
        if (t[i] < min) min = t[i];
        if (t[i] > max) max = t[i];
    }
    return (sum - min - max) / 3.0;
}

void sortPlayers(Player *p, int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (p[i].ao5 > p[j].ao5) {
                Player temp = p[i];
                p[i] = p[j];
                p[j] = temp;
            }
        }
    }
}

int main() {
    int n;
    printf("Enter number of players: ");
    scanf("%d", &n);

    Player *p = malloc(n * sizeof(Player));

    for (int i = 0; i < n; i++) {
        printf("\nPlayer %d name: ", i + 1);
        scanf("%s", p[i].name);

        printf("Enter 5 solve times:\n");
        for (int j = 0; j < 5; j++) {
            printf("Time %d: ", j + 1);
            scanf("%f", &p[i].times[j]);
        }

        p[i].ao5 = calculateAO5(p[i].times);
    }

    sortPlayers(p, n);

    printf("\n===== Final Ranking =====\n");
    for (int i = 0; i < n; i++) {
        printf("%d. %s - AO5: %.3f\n", i + 1, p[i].name, p[i].ao5);
    }

    FILE *f = fopen("tournament_results.txt", "w");
    for (int i = 0; i < n; i++) {
        fprintf(f, "%d. %s - AO5: %.3f\n", i + 1, p[i].name, p[i].ao5);
    }
    fclose(f);

    printf("\nResults saved to tournament_results.txt\n");

    free(p);
    return 0;
}

