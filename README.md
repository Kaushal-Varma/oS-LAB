# oS-LAB
Akshay
#include <stdio.h>
#include <string.h>

int main()
{
    char data[100], key[20], temp[120];
    int i, j, n;

    printf("Enter data: ");
    scanf("%s", data);

    printf("Enter generator: ");
    scanf("%s", key);

    n = strlen(key) - 1;
    strcpy(temp, data);

    for (i = 0; i < n; i++)
        strcat(temp, "0");

    for (i = 0; i < strlen(data); i++)
        if (temp[i] == '1')
            for (j = 0; j < strlen(key); j++)
                temp[i + j] = temp[i + j] == key[j] ? '0' : '1';

    printf("CRC: ");
    for (i = strlen(data); i < strlen(temp); i++)
        printf("%c", temp[i]);

    return 0;
}


/* ==================================================
   QUESTION 1(i): BASIC LINUX SHELL COMMANDS
   ==================================================

pwd
ls
mkdir OS_Lab
cd OS_Lab
touch test.txt
echo "Hello OS"
echo "Operating Systems" > notes.txt
cat notes.txt
ls -l


/* ==================================================
   QUESTION 6: NON-PREEMPTIVE SJF SCHEDULING
   ================================================== */

#include <stdio.h>

int main()
{
    int n = 5;
    int p[] = {1, 2, 3, 4, 5};
    int bt[] = {3, 6, 4, 5, 2};
    int at[] = {0, 2, 4, 6, 8};

    int ct[5], tat[5], wt[5], rt[5];
    int done[5] = {0, 0, 0, 0, 0};
    int i, time = 0, completed = 0, shortest;

    float avgwt = 0, avgtat = 0, avgrt = 0;

    while (completed < n)
    {
        shortest = -1;

        for (i = 0; i < n; i++)
        {
            if (at[i] <= time && done[i] == 0)
            {
                if (shortest == -1 || bt[i] < bt[shortest])
                    shortest = i;
            }
        }

        if (shortest == -1)
            time++;
        else
        {
            rt[shortest] = time - at[shortest];
            time += bt[shortest];
            ct[shortest] = time;
            tat[shortest] = ct[shortest] - at[shortest];
            wt[shortest] = tat[shortest] - bt[shortest];
            done[shortest] = 1;
            completed++;

            avgwt += wt[shortest];
            avgtat += tat[shortest];
            avgrt += rt[shortest];
        }
    }

    printf("P\tAT\tBT\tCT\tTAT\tWT\tRT\n");

    for (i = 0; i < n; i++)
        printf("P%d\t%d\t%d\t%d\t%d\t%d\t%d\n",
        p[i], at[i], bt[i], ct[i], tat[i], wt[i], rt[i]);

    printf("\nAverage WT = %.2f", avgwt / n);
    printf("\nAverage TAT = %.2f", avgtat / n);
    printf("\nAverage RT = %.2f\n", avgrt / n);

    return 0;
}


/* ==================================================
   QUESTION 8: BEST FIT MEMORY ALLOCATION
   ================================================== */

#include <stdio.h>

int main()
{
    int block[] = {100, 500, 200, 300, 600};
    int process[] = {212, 417, 112, 426};
    int n = 5, m = 4;
    int allocation[4];
    int i, j, best;

    for (i = 0; i < m; i++)
        allocation[i] = -1;

    for (i = 0; i < m; i++)
    {
        best = -1;

        for (j = 0; j < n; j++)
        {
            if (block[j] >= process[i])
            {
                if (best == -1 || block[j] < block[best])
                    best = j;
            }
        }

        if (best != -1)
        {
            allocation[i] = best;
            block[best] -= process[i];
        }
    }

    printf("Process\tSize\tBlock\n");

    for (i = 0; i < m; i++)
    {
        printf("P%d\t%d\t", i + 1, process[i]);

        if (allocation[i] != -1)
            printf("%d\n", allocation[i] + 1);
        else
            printf("Not Allocated\n");
    }

    return 0;
}


/* ==================================================
   QUESTION 9: LRU PAGE REPLACEMENT
   ================================================== */

#include <stdio.h>

int main()
{
    int p[] = {1,2,1,3,7,4,5,6,3,1,2,4,6,3,1};
    int n = 15, f[3] = {-1,-1,-1};
    int i, j, k, pos, fault = 0, found, least;

    for (i = 0; i < n; i++)
    {
        found = 0;

        for (j = 0; j < 3; j++)
            if (f[j] == p[i])
                found = 1;

        if (!found)
        {
            fault++;
            pos = -1;

            for (j = 0; j < 3; j++)
                if (f[j] == -1)
                {
                    pos = j;
                    break;
                }

            if (pos == -1)
            {
                least = n + 1;

                for (j = 0; j < 3; j++)
                {
                    for (k = i - 1; k >= 0; k--)
                        if (f[j] == p[k])
                            break;

                    if (k < least)
                    {
                        least = k;
                        pos = j;
                    }
                }
            }

            f[pos] = p[i];
        }

        printf("%d\t%d\t%d\t%d\t%s\n",
        p[i], f[0], f[1], f[2],
        found ? "Hit" : "Fault");
    }

    printf("\nTotal Page Faults = %d\n", fault);

    return 0;
}
