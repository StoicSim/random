//text for 

#include <graphics.h>
#include <stdlib.h>
#include <stdio.h>
#include <conio.h>
#include <dos.h>
#include <math.h>

// Function prototypes
void rotation(int a[2][5], float t);
void drawsquare(int a[2][5]);
void translating(int a[2][5], int tx, int ty);
void scaling(int a[2][5], int sx, int sy);
void reflection(int a[2][5], int axis);

void shearing(int a[2][5], float shx, float shy);
void drawaxes();
void printmessage(char *message);
void resetSquare(int a[2][5]);

int main(void) {
    // Request auto detection of graphics driver
    int gdriver = DETECT, gmode, errorcode;
    int i, j;
    int tx, ty;
    int a[2][5], b[2][5];

    // Initialize graphics and local variables
    initgraph(&gdriver, &gmode, "C:\\TURBOC3\\BGI");

    // Read result of initialization
    errorcode = graphresult();

    // Check if an error occurred
    if (errorcode != grOk) {
        printf("Graphics error: %s\n", grapherrormsg(errorcode));
        printf("Press any key to halt:");
        getch();
        exit(1);
    }

    // Initialize coordinates of the square
    resetSquare(a);

    // Draw the initial square with axes
    cleardevice();
    drawaxes();
    printmessage("Initial square");
    drawsquare(a);
    getch();

    // Reflect the square across the y-axis
    resetSquare(a);
    cleardevice();
    drawaxes();
    reflection(a, 1); // Reflect across y-axis
    printmessage("Reflecting square across y-axis");
    drawsquare(a);
    getch();

    // Reflect the square across the x-axis
    resetSquare(a);
    cleardevice();
    drawaxes();
    reflection(a, 0); // Reflect across x-axis
    printmessage("Reflecting square across x-axis");
    drawsquare(a);
    getch();

    // Translate the square
    resetSquare(a);
    cleardevice();
    drawaxes();
    translating(a, 100, 100);
    printmessage("Translating square");
    drawsquare(a);
    getch();

    // Iterative rotation and translation
    for (i = 0; i < 10; i++) {
        resetSquare(a);
        for (j = 0; j < 5; j++) {
            b[0][j] = a[0][j];
            b[1][j] = a[1][j];
        }

        cleardevice();
        drawaxes();
        translating(b, -150, -150);
        rotation(b, i / 2.0);
        translating(b, i * 10 + 150, i * 10 + 150);
        printmessage("Rotating and translating square");
        drawsquare(b);
        getch();
    }

    for (i = 1; i <= 3; i++) {
        // Reset the original square coordinates before scaling
        resetSquare(a);
        cleardevice();
        drawaxes();
        scaling(a, i, i); // Scale the square by factors 1, 2, and 3
        printmessage("Scaling square");
        drawsquare(a);
        getch();
    }

    // Shear the square
    resetSquare(a);
    cleardevice();
    drawaxes();
    shearing(a, 1.0, 0.0); // Shear along x-axis
    printmessage("Shearing square along x-axis");
    drawsquare(a);
    getch();

    resetSquare(a);
    cleardevice();
    drawaxes();
    shearing(a, 0.0, 1.0); // Shear along y-axis
    printmessage("Shearing square along y-axis");
    drawsquare(a);
    getch();

    // Clean up
    getch();
    closegraph();
    return 0;
}

// Function to rotate the square
void rotation(int a[2][5], float t) {
    int i, x, y;
    for (i = 0; i < 5; i++) {
        x = a[0][i];
        y = a[1][i];
        a[0][i] = x * cos(t) - y * sin(t);
        a[1][i] = x * sin(t) + y * cos(t);
    }
}

// Function to draw the square
void drawsquare(int a[2][5]) {
    int i;
    for (i = 0; i < 4; i++) {
        line(a[0][i], a[1][i], a[0][i + 1], a[1][i + 1]);
    }
}

// Function to translate the square
void translating(int a[2][5], int tx, int ty) {
    int i;
    for (i = 0; i < 5; i++) {
        a[0][i] = a[0][i] + tx;
        a[1][i] = a[1][i] + ty;
    }
}

// Function to scale the square
void scaling(int a[2][5], int sx, int sy) {
    int i;
    for (i = 0; i < 5; i++) {
        a[0][i] = a[0][i] * sx;
        a[1][i] = a[1][i] * sy;
    }
}

// Function to reflect the square
void reflection(int a[2][5], int axis) {
    int i;
    for (i = 0; i < 5; i++) {
        if (axis == 0) {
            a[1][i] = getmaxy() - a[1][i]; // Reflect across x-axis
        } else if (axis == 1) {
            a[0][i] = getmaxx() - a[0][i]; // Reflect across y-axis
        }
    }
}

// Function to shear the square
void shearing(int a[2][5], float shx, float shy) {
    int i, x, y;
    for (i = 0; i < 5; i++) {
        x = a[0][i];
        y = a[1][i];
        a[0][i] = x + shx * y;
        a[1][i] = y + shy * x;
    }
}

// Function to draw the x and y axes
void drawaxes() {
    int maxx = getmaxx();
    int maxy = getmaxy();
    line(maxx/2, 0, maxx / 2, maxy); // Y-axis
    line(0, maxy / 2, maxx, maxy / 2); // X-axis
}

// Function to print messages on screen
void printmessage(char *message) {
    outtextxy(10, 10, message);
}

// Function to reset the square to its initial position
void resetSquare(int a[2][5]) {
    a[0][0] = 100;
    a[1][0] = 100;
    a[0][1] = 100;
    a[1][1] = 200;
    a[0][2] = 200;
    a[1][2] = 200;
    a[0][3] = 200;
    a[1][3] = 100;
    a[0][4] = a[0][0];
    a[1][4] = a[1][0];
}
