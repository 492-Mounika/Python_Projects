#include <stdio.h>
#include <stdlib.h>

FILE *summary;

struct batsman {
    char name[25];
    int runs, balls, fours, sixes;
    float str;
} pl1[100];

struct bowler {
    char name[25];
    int runsgv, wkttkn, overs;
    float econ;
} pl2[100];

void createReport(int m, int n) {
    int i;
    int toruns = 0, max_run = 0, max_six = 0, max_four = 0, max_w = 0;

    fprintf(summary, "                     Match summary\n");
    fprintf(summary, "==========================================================================\n");
    fprintf(summary, " Batsman        runs           balls        fours       sixes         sr   \n");
    fprintf(summary, "==========================================================================\n");

    for (i = 0; i < m; i++) {
        fprintf(summary, " %-15s %-14d %-13d %-11d %-11d %-9.2f\n",
                pl1[i].name, pl1[i].runs, pl1[i].balls, pl1[i].fours, pl1[i].sixes, pl1[i].str);
        toruns += pl1[i].runs;

        if (max_run < pl1[i].runs) {
            max_run = pl1[i].runs;
        }

        if (max_six < pl1[i].sixes) {
            max_six = pl1[i].sixes;
        }

        if (max_four < pl1[i].fours) {
            max_four = pl1[i].fours;
        }
    }

    fprintf(summary, "\nTOTAL RUNS:%d\n\n", toruns);
    fprintf(summary, "\n\n");
    fprintf(summary, "=================================================================\n");
    fprintf(summary, " Bowler        overs           runs        wicket       economy\n");
    fprintf(summary, "=================================================================\n");

    for (i = 0; i < n; i++) {
        pl2[i].econ = (float)pl2[i].runsgv / pl2[i].overs;
        fprintf(summary, " %-15s %-14d %-13d %-11d %-11.2f\n\n\n", pl2[i].name, pl2[i].overs, pl2[i].runsgv, pl2[i].wkttkn, pl2[i].econ);

        if (max_w < pl2[i].wkttkn) {
            max_w = pl2[i].wkttkn;
        }
    }

    fprintf(summary, "\nHighest runs scored by the batsman:%d\n", max_run);
    fprintf(summary, "Maximum fours scored by the batsman:%d\n", max_four);
    fprintf(summary, "Maximum sixes scored by the batsman:%d\n", max_six);
    fprintf(summary, "Maximum wickets taken by the bowler:%d\n", max_w);
    fprintf(summary, "==========================================================================\n");
    fprintf(summary, "\n\n");
}

int main() {
    int choice;
    int i, m, n;

    summary = fopen("summary.txt", "a"); 

    if (summary == NULL) {
        printf("Error opening the file.\n");
        return 1;
    }

    do {
        printf("Enter the choice:\n 1)Create Report :\n2)View previous report\n 3)Exit: \n ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter the number of batsmen:\n");
                scanf("%d", &m);

                for (i = 0; i < m; i++) {
                    printf("Enter name of batsman%d:\n", i + 1);
                    scanf("%s", pl1[i].name);
                    printf("Enter the number of fours scored by player%d:\n", i + 1);
                    scanf("%d", &pl1[i].fours);
                    printf("Enter the number of sixes scored by player%d:\n", i + 1);
                    scanf("%d", &pl1[i].sixes);
                    printf("Enter the balls played by the player%d:\n", i + 1);
                    scanf("%d", &pl1[i].balls);

                    pl1[i].runs = (4 * pl1[i].fours) + (6 * pl1[i].sixes);
                    pl1[i].str = (pl1[i].runs * 100.0) / pl1[i].balls;
                }

                printf("Enter the number of bowlers:\n");
                scanf("%d", &n);

                for (i = 0; i < n; i++) {
                    printf("Enter name of bowler%d:\n", i + 1);
                    scanf("%s", pl2[i].name);
                    printf("Enter the runs given by the bowler%d:\n", i + 1);
                    scanf("%d", &pl2[i].runsgv);
                    printf("Enter the overs bowled by the bowler%d:\n", i + 1);
                    scanf("%d", &pl2[i].overs);
                    printf("Enter the wickets taken by the bowler%d\n", i + 1);
                    scanf("%d", &pl2[i].wkttkn);
                }

                printf("Thank you, all details are recorded\n");
                createReport(m, n);

                break;

            case 2:
                
                fclose(summary);

                summary = fopen("summary.txt", "r");

                if (summary == NULL) {
                    printf("Error opening the file.\n");
                    return 1;
                }

                int ch;
                while ((ch = fgetc(summary)) != EOF) {
                    putchar(ch);
                }

                fclose(summary);
                break;

            case 3:
                exit(0);

            default:
                printf("Enter the correct choice\n");
                break;
        }

    } while (choice != 3);

    fclose(summary);

    return 0;
}
