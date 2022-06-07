# Task 1.           ROBOt 


## By :-

```sh
Contibutors ::--  Hassan Hosni Abdel_Aleem     
                  Mohamed Khaled Galloul       
                  Ahmed Mohamed Mohamed        
                  Moamen Gamal Ashmawy         
                  Youssef Mahmoud Elhely
```

# Introduction 


In any desgin it is important to have over veiw about all thingths in your design ,
so in design the robot first thinght the robot shoud be like human  ai all thingth like 
shap,movevment , joints  ,degree of movement freedom and laads ratio.
we design .boby  in the first and use the scene graph to help us in design all parts are reletive to body .
and we applayed the camera movement and zoom in and zoom out.
and access their movement , in our design we use OPENGL and it`s libraries .



# Implementation


we used " glPushmatrix" and "glPopmatrix" to orgnize or design in movement , rotation ,scaling and drawing 
the elements.
 ### movement 

- Shoulders move to 90 degree up and dowen
we used "P ,p" AND "O,o" AND "7,8///9//6" in there movements in.to movement in 2 plane x and z
- elbow move up to 150 degree UP and has small digree in movement freedom in other side .we used "I, i" AND "U,u" in there movements
- fingers movement by used "l,j,g,m,b"amd "L,J,G,M,B" for opposite direction .
- uper parts of fingers move by "k,h,f,n,v" and "K,H,F,N,V"
  for the opposite direction
- hip movements 90 digree in x and z plane we used "y,Y" AND "R,r" for movement in x plane and "Z,z"and "2,3" for movement in z plane 
- knee movement 90 digree up and dowun by "t.T" AND ""E,e"
- to zoom in we used "q"
- to zoom out we used"Q"
- for rotation around the robot we use keyboard aarrow "a,s,d,w"

✨ all movement  are  be with range of degrees 

# code 


### - include and define the variable 
```cpp
#include <GL/glut.h>
#include <math.h>
#include <stdio.h>

GLfloat body = 0, shoulder1 = -90, shoulder2 = 90, elbow1 = 0, elbow2 = 0, leg1 = 0,leg1z = 0, knee1 = 0, leg2 = 0,leg2z = 0, knee2 = 0;
GLfloat moving, startx, starty, theta = 0.0;
double eye[] = { 0 , 0, -1 };
double center[] = {0, 0, 4};
double up[] = { 0, 1, 0};
double perpendicularAxis[] = { 0, 0, 0 };
GLfloat Znear = 100.0;
int windowWidth;
int windowHeight;
int fingerBase1[5] = { 0 }, fingerUp1[5] = { 0 };
int fingerBase2[5] = { 0 }, fingerUp2[5] = { 0 };

GLfloat angle = 0.0;   /* in degrees */
GLfloat angle2 = 0.0;   /* in degrees */

```



### - design our work space and use "glShadeModel" to make the shap smooth and back ground be black
```cpp
void init(void)
{
   glClearColor(0.0, 0.0, 0.0, 0.0);
   glShadeModel(GL_FLAT);
}
```

deleting any anwanted color by 
```cpp
glClear(GL_COLOR_BUFFER_BIT);
```

### now we will first of all used "glPushMatrix"and "glPopMatrix()" to orginaze or design in our display function when we call it .
#### example of leg design 
```cpp
   /*******************************************************
     *                        LEG 1                        *
    ********************************************************/
    glPushMatrix();             /* LEG 1 STACK */
    glTranslatef(1.0, -1.35, 0.0);
    glRotatef((GLfloat)leg1, 1.0, 0.0, 0.0);
    glRotatef((GLfloat)leg1z, 0.0, 0.0, 1.0);
    glTranslatef(-0.35, -1.2, 0.0);
    glPushMatrix();             /* THIGH 1 STACK */
    glScalef(0.7, 2.0, 1.0);
    glutWireCube(1.0);          /* drawing the actual leg */
    glPopMatrix();              /* THIGH 1 STACK */

    /*******************************************************
     *                        KNEE 1                       *
    ********************************************************/
    glPushMatrix();             /* LOWER LEG 1 STACK */
    glTranslatef(1.0, -1.35, 0.0);
    glRotatef((GLfloat)knee1, 1.0, 0.0, 0.0);
    glTranslatef(-1, -0.7, 0.0);
    glPushMatrix();             /* KNEE 1 STACK */
    glScalef(0.7, 2.0, 1.0);
    glutWireCube(1.0);          /* drawing the actual knee */
    glPopMatrix();              /* KNEE 1 STACK */
    glTranslatef(0.0, -1.3, 0.0);
    glScalef(0.8, 0.7, 2.0);
    glutSolidCube(1.0);         /* drawing the actual ankle */
    glPopMatrix();              /* LOWER LEG 1 STACK */
    glPopMatrix();              /* LEG 1 STACK */

```
### example of code to design the first finger 

- not the corrdinates change from finger to other 
so that we will callculate the corrdinates for each finger. and we design them in separete part for more orginization and applayed ther movement 
```cpp
void displayfingers(int armNumber) {
    float y = -0.3;
    for (int i = 0; i < 5; i++) {
        glPushMatrix();
        glTranslatef(armNumber? 1.0:-1.6 , (i ? 0.3 : -0.2), (i ? (y + i * 0.18) : (y + i * 0.15))); // one step forward and one step in y according to finger number
        glRotatef((GLfloat) fingerBase1[i], 0.0, 0.0, i ? -1.0 : 1.0); // rotate figerbase around -z (variable)
        glTranslatef(0.15, 0.0, 0.0);                   // step 0.15 forward
        glPushMatrix();
        glScalef(0.3, 0.1, 0.1);
        glutWireCube(1);                            // DRAW THE FINGERBASE
        glPopMatrix();

        //Draw finger flang 1 
        glTranslatef(0.15, 0.0, 0.0);                   // step 0.15 forward
        glRotatef((GLfloat) -fingerUp1[i], 0.0, 0.0, i ? -1.0 : -1.0);    // rotate upperfinger around -z (variable)
        glTranslatef(0.15, 0.0, 0.0);                   // step 0.15 forward
        glPushMatrix();
        glScalef(0.3, 0.1, 0.1);
        glutWireCube(1);                            // DRAW THE UPPERFINGER
        glPopMatrix();
        glPopMatrix();
    }
}

```
### now for control the movement of robot  we define function call "kebourd"  to allow us after running the program to control in the fobot  , we use "switch " function for diffrent cases
#### pice of code and will be same for all elements but the change the degree of movement  (the example for shoulder )
```cpp
void keyboard(unsigned char key, int x, int y)
{
   switch (key)
   {
    case '7':
        if (shoulder10 > 150){ shoulder10 = 150; }
        else
        {
            shoulder10 = (shoulder10 + 5);
            glutPostRedisplay();
        }
        break;
    case '8':
        if (shoulder10 <= -90)
        {
            shoulder10 = -90;
        }
        else
        {
            shoulder10 = (shoulder10 - 5);
            glutPostRedisplay();
        }
        break;
        ////////
    case 'p':
        if (shoulder1 > 45){ shoulder1 = 45; }
        else
        {
            shoulder1 = (shoulder1 + 5);
            glutPostRedisplay();
        }
        break;
    case 'P':
        if (shoulder1 <= -90)
        {
            shoulder1 = -90;
        }
        else
        {
            shoulder1 = (shoulder1 - 5);
            glutPostRedisplay();
        }
        break;
        
```
### now for chnge the angle of veiw we will define a new function to all us from that by using mouse . tha code will be like 
```cpp
void mouse(int button, int state, int x, int y)
{
    if (button == GLUT_LEFT_BUTTON) {
        if (state == GLUT_DOWN) {
            moving = 1;
            startx = x;
            starty = y;
        }
        if (state == GLUT_UP) {
            moving = 0;
        }
    }
}
```
### for the camera movement and rotatiom we use the following code 
```cpp
void rotatePoint(double a[], double theta, double p[])
{

    double temp[3];
    temp[0] = p[0];
    temp[1] = p[1];
    temp[2] = p[2];

    temp[0] = -a[2] * p[1] + a[1] * p[2];
    temp[1] = a[2] * p[0] - a[0] * p[2];
    temp[2] = -a[1] * p[0] + a[0] * p[1];

    temp[0] *= sin(theta);
    temp[1] *= sin(theta);
    temp[2] *= sin(theta);

    temp[0] += (1 - cos(theta)) * (a[0] * a[0] * p[0] + a[0] * a[1] * p[1] + a[0] * a[2] * p[2]);
    temp[1] += (1 - cos(theta)) * (a[0] * a[1] * p[0] + a[1] * a[1] * p[1] + a[1] * a[2] * p[2]);
    temp[2] += (1 - cos(theta)) * (a[0] * a[2] * p[0] + a[1] * a[2] * p[1] + a[2] * a[2] * p[2]);

    temp[0] += cos(theta) * p[0];
    temp[1] += cos(theta) * p[1];
    temp[2] += cos(theta) * p[2];

    p[0] = temp[0];
    p[1] = temp[1];
    p[2] = temp[2];

}

void crossProduct(double a[], double b[], double c[])
{
    c[0] = a[1] * b[2] - a[2] * b[1];
    c[1] = a[2] * b[0] - a[0] * b[2];
    c[2] = a[0] * b[1] - a[1] * b[0];
}

void normalize(double a[])
{
    double norm;
    norm = a[0] * a[0] + a[1] * a[1] + a[2] * a[2];
    norm = sqrt(norm);
    a[0] /= norm;
    a[1] /= norm;
    a[2] /= norm;
}
```
### and for control
```cpp
void keyboard(unsigned char key, int x, int y)
{
    switch (key)
    {
    case 'w':
        crossProduct(up, center, perpendicularAxis);
        normalize(perpendicularAxis);
        rotatePoint(perpendicularAxis, 0.05, center);
        rotatePoint(perpendicularAxis, 0.05, up);
        glutPostRedisplay();
        break;
    case 's':
        crossProduct(up, center, perpendicularAxis);
        normalize(perpendicularAxis);
        rotatePoint(perpendicularAxis, -0.05, center);
        rotatePoint(perpendicularAxis, -0.05, up);
        glutPostRedisplay();
        break;
    case 'a':
        rotatePoint(up, -0.05, center);
        glutPostRedisplay();
        break;
    case 'd':
        rotatePoint(up, 0.05, center);
        glutPostRedisplay();
        break;
    case 'q':
        if (Znear > 120) { Znear = 120; }
        else if (Znear <= 60) { Znear = 60; }
        else 
            Znear -= 1.0;

        reshape(windowWidth, windowHeight);
        glutPostRedisplay();
        break;
    case 'Q':
        if (Znear >= 120) { Znear = 120; }
        else if (Znear < 60) { Znear = 60; }
        else
            Znear += 1.0;
        reshape(windowWidth, windowHeight);
        glutPostRedisplay();
        break;
```

### finally we will define  two functioms one for shape reshape and other for motion .
 #### the code will be like 
 ```cpp
 void reshape(int w, int h)
{
    windowWidth = w;
    windowHeight = h;
    glViewport(0, 0, (GLsizei)w, (GLsizei)h);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    printf("\n%f", Znear);
    gluPerspective(Znear, (GLfloat)w / (GLfloat)h, 0.0, 20.0);
    glMatrixMode(GL_MODELVIEW);
    glLoadIdentity();
    glTranslatef(0.0, 0.0, -5.0);
}
 ```
 and 
 ```cpp
void motion(int x, int y)
{
    if (moving) {
        /*
        angle = angle + (x - startx);
        angle2 = angle2 + (y - starty);*/
        int dx = x - startx;
        int dy = y - starty;
        rotatePoint(up, -(dx % 360) * 0.01 , center);
        crossProduct(up, center, perpendicularAxis);
        normalize(perpendicularAxis);
        rotatePoint(perpendicularAxis, -dy * 0.01, center);
        rotatePoint(perpendicularAxis, -dy * 0.01, up);
        startx = x;
        starty = y;
        glutPostRedisplay();
    }
}
 ```
 
 ### then now we will find our main funtion like that 
 ```cpp
 int main(int argc, char** argv)
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB);
    glutInitWindowSize(5000, 5000);
    glutInitWindowPosition(10, 10);
    glutCreateWindow(argv[0]);
    init();
    glutMouseFunc(mouse);
    glutMotionFunc(motion);
    glutDisplayFunc(display);
    glutReshapeFunc(reshape);
    glutKeyboardFunc(keyboard);
    glutMainLoop();
    return 0;
}
 ```
 
 
 # thank you 
 
 #####  Issues that  I faced in the assignment 
 some function and how we  can use it . like how we applayed the camera movement . it take more time to solvd it and we used google for help.
 
 
 #### no code sharing and no copy from internet only copy from start up code " task one "







