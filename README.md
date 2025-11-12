# Ao5-Tracker
In a standard World Tour we can easily check the solves and the Average of the players,
where we can separate the best and worst time of the solves and keep the rest three available solves to count the prominent averages of the players 5 solves.

To tracks the ao5 of players, the code first asks for the username , then asks for the results of the solves one by one, after 5 attempts the system excllludes the highest and the lowest count,
using the 3 solves it counts and gives the user the Ao5. it again aks for further player information on new players ,while keeping the previous information saved.
if selected on the load results in any point, the code will show the all previously saved database untill clicked on exist.

for a better service the information that can be saved has been saved to 20 players, based on a average perticipant count. 




#include <stdio.h>

#define MAX_PLAYERS 20
#define MAX_SOLVES 5

struct Player {
    char name[50];
    float times[MAX_SOLVES];
    float average;
};

int main() {
    struct Player players[MAX_PLAYERS];
    int playerCount = 0;
    int choice;

    while (1) {
        printf("\n1. Add New Player\n");
        printf("2. Show All Results\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        if (choice == 1) {
            if (playerCount >= MAX_PLAYERS) {
                printf("Cannot add more players!\n");
                continue;
            }

            printf("Enter player name: ");
            scanf("%s", players[playerCount].name);

            float sum = 0, max = 0, min = 9999;
            printf("Enter 5 solve times for %s:\n", players[playerCount].name);
            for (int i = 0; i < MAX_SOLVES; i++) {
                printf("Solve %d: ", i + 1);
                scanf("%f", &players[playerCount].times[i]);
                sum += players[playerCount].times[i];
                if (players[playerCount].times[i] > max) max = players[playerCount].times[i];
                if (players[playerCount].times[i] < min) min = players[playerCount].times[i];
            }

            players[playerCount].average = (sum - max - min) / 3.0;
            printf("%s's Ao5 = %.3f\n", players[playerCount].name, players[playerCount].average);

            playerCount++;
        }

        else if (choice == 2) {
            if (playerCount == 0) {
                printf("No results yet.\n");
                continue;
            }

            printf("\nAll Saved Results:\n");
            for (int i = 0; i < playerCount; i++) {
                printf("%d. %s - Ao5: %.3f\n", i + 1, players[i].name, players[i].average);
            }
        }

        else if (choice == 3) {
            printf("Exiting...\n");
            break;
        }

        else {
            printf("Invalid choice, try again.\n");
        }
    }

    return 0;
}
