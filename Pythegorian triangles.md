#include <stdio.h>
#include <conio.h>
#include <math.h>

int antal;

typedef struct elem{
    long x;
    long y;
} elements;

int gcd(int num1, int num2)
{
    int i, hcf;
    for(i=1; i<=num1 || i<=num2; ++i)
    {
        if(num1%i==0 && num2%i==0)
            hcf=i;
    }
    return hcf;
}

void printtabell( elements *table) {
    int i;
    printf("    i sida_a sida_b sida_c\n");
    for (i=0; i <= antal-1;  i++){
        printf("%5d %6ld %6ld %6d\n", i, table[i].x, table[i].y, (int)sqrt(table[i].x*table[i].x+table[i].y*table[i].y));
    }
    printf("\n");
    return;
}

void printtabellfil( elements *table) {
    FILE *F = fopen("tabell.txt", "w");
    if (F){
        int i;
        for (i=0; i <= antal-1;  i++){
            if(i%200==0)fprintf(F,"antal sida_a sida_b sida_c\n");
            fprintf(F,"%5d %6ld %6ld %6d\n", i+1, table[i].x, table[i].y, (int)sqrt(table[i].x*table[i].x+table[i].y*table[i].y));
        }
        printf("\n");
    }
    fclose(F);
    return;

}

void insertelem( elements* table, elements* current) {
    int i,j,k=antal,hcfx, hcfy;
    if (antal == 0){
        table[0].x = current->x;
        table[0].y = current->y;
        antal++;
    }else{
        i=0;
        while(i < antal){
            if (table[i].x == current->x && table[i].y == current->y )
            {
                k=0;
                break;
            }

            hcfx = gcd(table[i].x, current->x);
            hcfy = gcd(table[i].y, current->y);
            if (hcfx > 1 && hcfy > 1 ) {
                k=0;
                break;
            }

            if (table[i].x > current.x){
                j=antal-1;
                while (j>i-1){
                    k=i;
                    table[j+1].x = table[j].x;
                    table[j+1].y = table[j].y;
                    j--;
                }
                table[k].x = current->x;
                table[k].y = current->y;
                antal++;
                break;
            }else{
                i++;
            }
        }
        if (k==antal && antal>0){
            table[k].x = current->x;
            table[k].y = current->y;
            antal++;
        }
    }
    return;
}

int main(void)
{
    /* algoritm a2 + b2 = c2 där c=k(m2 + n2), b=2kmn, a=k(m2-n2) och m>n */
    int z;
    long a, b, c, k, m, n, m2, n2, temp;
    elements tabell[500];
    elements current;
    printf("Mata in  z: ");
    scanf("%i", &z);
    antal = 0;
    for (k=1; k<z;  k++){
        for (m=1; m<=z;  m++){
            for (n=1; n<m;  n++){
                m2 = m*m;
                n2 = n*n;
                if ((m2+n2) < (long)(z))
                    {
                    c = k * (m2+n2);
                    if (c < (long)z) {
                        a = k * (m2 - n2);
                        b = 2 * k * m * n;
                        if (a>b) {temp=a; a=b; b=temp;}
                        printf("%7d   abc %7ld  %7ld, %7ld, kmn %ld %ld %ld \n",antal,a,b,c,k,m,n);
                        current.x = a;
                        current.y = b;
                        insertelem(tabell,&current);
                    }
                }
            }
        }
    }
    printtabell(tabell);
    printtabellfil(tabell);
    return 0;
}

