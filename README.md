## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

## AIM:
To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.

## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.

## Program:
```

#include <stdio.h>
#include <string.h>
#include <ctype.h>

char key[5][5];
int used[26] = {0};

void generateKey(char str[])
{
    int k = 0;
    used['J' - 'A'] = 1;

    for (int i = 0; str[i]; i++)
    {
        char c = toupper(str[i]);
        if (c >= 'A' && c <= 'Z' && !used[c - 'A'])
        {
            key[k / 5][k % 5] = c;
            used[c - 'A'] = 1;
            k++;
        }
    }

    for (char c = 'A'; c <= 'Z'; c++)
    {
        if (!used[c - 'A'])
        {
            key[k / 5][k % 5] = c;
            k++;
        }
    }
}

void findPosition(char c, int *r, int *col)
{
    if (c == 'J') c = 'I';

    for (int i = 0; i < 5; i++)
        for (int j = 0; j < 5; j++)
            if (key[i][j] == c)
            {
                *r = i;
                *col = j;
                return;
            }
}

void process(char text[], int encrypt)
{
    int len = strlen(text);

    for (int i = 0; i < len; i += 2)
    {
        int r1, c1, r2, c2;
        findPosition(text[i], &r1, &c1);
        findPosition(text[i+1], &r2, &c2);

        if (r1 == r2)
        {
            text[i] = key[r1][(c1 + (encrypt ? 1 : 4)) % 5];
            text[i+1] = key[r2][(c2 + (encrypt ? 1 : 4)) % 5];
        }
        else if (c1 == c2)
        {
            text[i] = key[(r1 + (encrypt ? 1 : 4)) % 5][c1];
            text[i+1] = key[(r2 + (encrypt ? 1 : 4)) % 5][c2];
        }
        else
        {
            text[i] = key[r1][c2];
            text[i+1] = key[r2][c1];
        }
    }
}

int main()
{
    char keyword[100], plain[200], cipher[200], decrypted[200];
    int n = 0;

    printf("Enter keyword: ");
    scanf("%99s", keyword);

    printf("Enter plaintext: ");
    scanf("%199s", plain);

    for (int i = 0; plain[i]; i++)
    {
        char c = toupper(plain[i]);
        if (c >= 'A' && c <= 'Z')
        {
            if (c == 'J') c = 'I';

            if (n > 0 && plain[i] == plain[i-1] && n % 2 == 1)
                cipher[n++] = 'X';

            cipher[n++] = c;
        }
    }

    if (n % 2 != 0)
        cipher[n++] = 'X';

    cipher[n] = '\0';

    generateKey(keyword);

    process(cipher, 1);
    printf("\nEncrypted Text: %s", cipher);

    strcpy(decrypted, cipher);
    process(decrypted, 0);
    printf("\nDecrypted Text: %s\n", decrypted);

    return 0;
}

```


## Output:
<img width="923" height="392" alt="image" src="https://github.com/user-attachments/assets/5626645e-ebad-4e24-a640-46df021291a4" />


## Result:

Thus, the C program to implement the Playfair Cipher encryption technique was executed successfully, and the plaintext was encrypted into ciphertext.
