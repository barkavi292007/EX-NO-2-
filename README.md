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

char m[5][5];

void makeKey(char k[]) {
    int used[26] = {0}, r = 0, c = 0;
    used['J'-'A'] = 1;

    for (int i = 0; k[i]; i++) {
        char ch = toupper(k[i]);
        if (ch < 'A' || ch > 'Z') continue;
        if (ch == 'J') ch = 'I';
        if (!used[ch-'A']) {
            m[r][c++] = ch;
            used[ch-'A'] = 1;
            if (c == 5) c = 0, r++;
        }
    }

    for (char ch = 'A'; ch <= 'Z'; ch++)
        if (!used[ch-'A']) {
            m[r][c++] = ch;
            if (c == 5) c = 0, r++;
        }
}

void encrypt(char a, char b) {
    int r1, c1, r2, c2;
    for (int i = 0; i < 5; i++)
        for (int j = 0; j < 5; j++) {
            if (m[i][j] == a) r1 = i, c1 = j;
            if (m[i][j] == b) r2 = i, c2 = j;
        }

    if (r1 == r2)
        printf("%c%c", m[r1][(c1+1)%5],
                       m[r2][(c2+1)%5]);
    else if (c1 == c2)
        printf("%c%c", m[(r1+1)%5][c1],
                       m[(r2+1)%5][c2]);
    else
        printf("%c%c", m[r1][c2], m[r2][c1]);
}

int main() {
    char key[100], text[100], p[200];
    int n = 0;

    printf("Enter key: ");
    scanf("%99s", key);
    printf("Enter plaintext: ");
    scanf("%99s", text);

    makeKey(key);

    for (int i = 0; text[i]; i++) {
        char ch = toupper(text[i]);
        if (ch >= 'A' && ch <= 'Z')
            p[n++] = (ch == 'J') ? 'I' : ch;
    }

    printf("Encrypted text: ");
    for (int i = 0; i < n; ) {
        char a = p[i++], b;
        if (i == n || a == p[i])
            b = 'X';
        else
            b = p[i++];
        encrypt(a, b);
    }

    printf("\n");
    return 0;
}

```


## Output:
<img width="1917" height="777" alt="image" src="https://github.com/user-attachments/assets/8f433756-4813-4306-b544-90b35b4f2c4f" />

## Result:

Thus, the C program to implement the Playfair Cipher encryption technique was executed successfully, and the plaintext was encrypted into ciphertext.
