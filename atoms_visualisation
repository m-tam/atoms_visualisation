#include <windows.h>
#include <winuser.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <conio.h>
#include <gl/freeglut.h>
#include <WinGDI.h>

#define bool int
#define FALSE 0
#define TRUE 0
#define SAVE 1
#define SHOW 2

static HGLRC hRC;		
static HDC hDC;	

int mainWindow;    
int mainMenu;
int menuFlag = 0;

float rot_x = 0, rot_y = 0, rot_z = 0;
static GLfloat atom_radius;
float x = 0;
float y = 0;
float z = 1;

float right_but_x = 0 ;
float right_but_y = 0 ;
float right_but_z = 0 ;

GLfloat mag = 1;
POINT MousePos;
bool scroll = FALSE;
bool move = FALSE;
bool right_but_pressed = FALSE;
static double objrx, objry, objrz;

double modelview[16]= {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
double projection[16] = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
int viewport[4]= {0, 0, 0, 0};

unsigned char *outputImage = 0;
FILE *pic;
unsigned long imageSize;
unsigned char *data=NULL;
GLenum lastBuffer;
char *mbtext = NULL;

unsigned int selectBuffer[128] = {0};
int objectsFound = 0;
unsigned int lowestDepth = 0;
static int objectID;
int j = 0;

char at_data_buf[10000] = {0};
static GLdouble viewer[] = {10.0, 10.0, 10.0};

/************************************************************************/
/*                                                                      */
/************************************************************************/

void SetupRC(void)
{
	
	glClearColor(0.90f, 0.90f, 0.98f, 0.5f);
	glEnable (GL_DEPTH_TEST);
	glShadeModel (GL_FLAT);
	/*glInitNames();
	glPushName(0);*/
	
}

/************************************************************************/
/*                                                                      */
/************************************************************************/

/*Resizing OpenGL scene anytime the window is resized*/
void Reshape (GLsizei width,GLsizei height)
{
	if (height == 0) height = 1;		//Prevent dividing into 0
	glRenderMode(GL_RENDER);

	glViewport(0,0,width,height);		//Set view field (ѕѕ)
	
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluPerspective(60.0f, (GLfloat) width/(GLfloat) height, 1.0, 30.0);
	
	glMatrixMode(GL_MODELVIEW);
	
	glLoadIdentity();
	gluLookAt(10, 10, 10, 0, 0, 0, 0, 1, 0);	
	
}



/************************************************************************/
/*                                                                      */
/************************************************************************/
void polygon(GLfloat a, GLfloat b, GLfloat c)
{
	glColor3f(0.75f, 0.75f, 0.75f); 
	glScalef(0.5, 0.5,0.5);
	glPushMatrix();
	glPolygonMode(GL_FRONT_AND_BACK, GL_LINE);


	glBegin(GL_POLYGON);			//front face
	glNormal3f(0.0f, 0.0f, c);
	glVertex3f(0.0f, 0.0f, c);
	glVertex3f(a, 0.0f, c);
	glVertex3f(a, b, c);
	glVertex3f(0.0f, b, c);	
	glEnd();


	glBegin(GL_POLYGON);			//back face
	glNormal3f(0.0f, 0.0f, c);
	glVertex3f(0.0f, 0.0f, 0.0f);
	glVertex3f(a, 0.0f,0.0f);
	glVertex3f(a, b,0.0f);
	glVertex3f(0.0f, b, 0.0f);	
	glEnd();


	glBegin(GL_POLYGON);			//left face
	glNormal3f(a, 0.0f, 0.0f);
	glVertex3f(0.0f, 0.0f, c);
	glVertex3f(0.0f, 0.0f, 0.0f);
	glVertex3f(0.0f, b, 0.0f);
	glVertex3f(0.0f, b, c);	
	glEnd();


	glBegin(GL_POLYGON);			//right face
	glNormal3f(a, 0.0f, 0.0f);
	glVertex3f(a, 0.0f, c);
	glVertex3f(a, 0.0f, 0.0f);
	glVertex3f(a, b, 0.0f);
	glVertex3f(a, b, c);	
	glEnd();


	glBegin(GL_POLYGON);			//top face
	glNormal3f(0.0f, b, 0.0f);
	glVertex3f(0.0f, b, c);	
	glVertex3f(a, b, c);
	glVertex3f(a, b,0.0f);
	glVertex3f(0.0f, b, 0.0f);
	glEnd();


	glBegin(GL_POLYGON);			//bottom face
	glNormal3f(0.0f, b, 0.0f);
	glVertex3f(0.0f, 0.0f, c);
	glVertex3f(a, 0.0f, c);
	glVertex3f(a, 0.0f,0.0f);
	glVertex3f(0.0f, 0.0f, 0.0f);
	glEnd();

	glPopMatrix();

}

/************************************************************************/
/*                                                                      */
/************************************************************************/

void draw_atoms(char atom, GLfloat atx, GLfloat aty, GLfloat atz, GLfloat a, GLfloat b, GLfloat c)
{
	glPolygonMode(GL_FRONT_AND_BACK, GL_FILL);

	/**************************s-elements*******************************/

		if (atom == (char)"H" || atom ==(char)"He" || atom ==(char)"Li" || atom ==(char)"Be"|| 
			atom ==(char)"Na" || atom ==(char)"Mg" || atom ==(char)"K" || atom ==(char)"Ca"|| 
			atom ==(char)"Rb"|| atom ==(char)"Sr"|| atom ==(char)"Cs"|| atom ==(char)"Ba"|| 
			atom ==(char)"Fr"|| atom ==(char)"Ra")	
		{
			//colour = {};
			glColor3f(0.98f, 0.4f, 0.7f);

			switch (atom)
			{
			case ('H'):
				atom_radius = 53.0f;
				break;
			case ('He'):
				atom_radius = 31.0f;
				break;
			case ('Li'):
				atom_radius = 167.0f;
				break;
			case ('Be'):
				atom_radius = 112.0f;
				break;
			case ('Na'):
				atom_radius = 190.0f;
				break;
			case ('Mg'):
				atom_radius = 145.0f;
				break;
			case ('K'):
				atom_radius = 243.0f;
				break;
			case ('Ca'):
				atom_radius = 194.0f;
				break;
			case ('Rb'):
				atom_radius = 265.0f;
				break;
			case ('Sr'):
				atom_radius = 219.0f;
				break;
			case ('Cs'):
				atom_radius = 298.0f;
				break;
			case ('Ba'):
				atom_radius = 253.0f;
				break;
			case ('Fr'):
				atom_radius = 277.0f;
				break;
			case ('Ra'):
				atom_radius = 235.0f;
				break;
			}
			glPushMatrix();
			glTranslatef(atx*a, aty*b, atz*c);
			glutSolidSphere(atom_radius/100, 30, 30);
			//auxSolidSphere(atom_radius/500);

			glPopMatrix();
		}


		/********************************p-elements*************************************/
		//case('B' || 'C' || 'N' || 'O'|| 'F' || 'Ne' || 'Al' || 'Si'|| 'P'|| 'S'|| 'Cl'|| 'Ar'|| 'Ga'|| 'Ge'|| 'As'|| 'Se'|| 'Br'|| 'Kr'|| 'In'|| 'Sn'|| 'Sb'|| 'Te'|| 'I'|| 'Xe'|| 'Tl'|| 'Pb'|| 'Bi'|| 'Po'|| 'At'|| 'Rn' ):
		else if (atom == (char)"B" || atom ==(char)"C" || atom ==(char)"N" || atom ==(char)"O"|| atom ==(char)"F" || 
			atom ==(char)"Ne" || atom ==(char)"Al" || atom ==(char)"Si"|| atom ==(char)"P"|| atom ==(char)"S"|| 
			atom ==(char)"Cl"|| atom ==(char)"Ar"|| atom ==(char)"Ga"|| atom ==(char)"Ge"|| atom ==(char)"As"|| 
			atom ==(char)"Se"|| atom ==(char)"Br"|| atom ==(char)"Kr"|| atom ==(char)"In"|| atom ==(char)"Sn"|| 
			atom ==(char)"Sb"|| atom ==(char)"Te"|| atom ==(char)"I"|| atom ==(char)"Xe"|| atom ==(char)"Tl"|| 
			atom ==(char)"Pb"|| atom ==(char)"Bi"|| atom ==(char)"Po"|| atom ==(char)"At"|| atom ==(char)"Rn" )		
		{
			//colour = {1.0f, 1.0f, 0.0f};
			glColor3f(1.0f, 1.0f, 0.0f); //yellow

			switch (atom)
			{
			case ('B'):
				atom_radius = 87.0f;
				break;
			case ('C'):
				atom_radius = 67.0f;
				break;
			case ('N'):
				atom_radius = 56.0f;
				break;
			case ('O'):
				atom_radius = 48.0f;
				break;
			case ('F'):
				atom_radius = 42.0f;
				break;
			case ('Ne'):
				atom_radius = 38.0f;
				break;
			case ('Al'):
				atom_radius = 118.0f;
				break;
			case ('Si'):
				atom_radius = 111.0f;
				break;
			case ('P'):
				atom_radius = 98.0f;
				break;
			case ('S'):
				atom_radius = 88.0f;
				break;
			case ('Cl'):
				atom_radius = 79.0f;
				break;
			case ('Ar'):
				atom_radius = 71.0f;
				break;
			case ('Ga'):
				atom_radius = 136.0f;
				break;
			case ('Ge'):
				atom_radius = 125.0f;
				break;
			case ('As'):
				atom_radius = 114.0f;
				break;
			case ('Se'):
				atom_radius = 103.0f;
				break;
			case ('Br'):
				atom_radius = 94.0f;
				break;
			case ('Kr'):
				atom_radius = 88.0f;
				break;
			case ('In'):
				atom_radius = 156.0f;
				break;
			case ('Sn'):
				atom_radius = 145.0f;
				break;
			case ('Sb'):
				atom_radius = 133.0f;
				break;
			case ('Te'):
				atom_radius = 123.0f;
				break;
			case ('I'):
				atom_radius = 115.0f;
				break;
			case ('Xe'):
				atom_radius = 108.0f;
				break;
			case ('Tl'):
				atom_radius = 156.0f;
				break;
			case ('Pb'):
				atom_radius = 154.0f;
				break;
			case ('Bi'):
				atom_radius = 143.0f;
				break;
			case ('Po'):
				atom_radius = 135.0f;
				break;
			case ('At'):
				atom_radius = 144.0f;
				break;
			case ('Rn'):
				atom_radius = 120.0f;
				break;

			}
			glPushMatrix();
			glTranslatef(atx*a, aty*b, atz*c);
			glutSolidSphere(atom_radius/100, 30, 30);
			//auxSolidSphere(atom_radius/100);
			glPopMatrix();

		}

		/*********************d-elements*************************************/
		//case('Sc' || 'Ti' || 'V' || 'Cr'|| 'Mn' || 'Fe' || 'Co' || 'Ni'|| 'Cu'|| 'Zn'|| 'Y'|| 'Zr'|| 'Nb'|| 'Mo'|| 'Tc'|| 'Ru'|| 'Rh'|| 'Pd'|| 'Ag'|| 'Cd'|| 'Hf'|| 'Ta'|| 'W'|| 'Re'|| 'Os'|| 'Ir'|| 'Pt'|| 'Au'|| 'Hg'|| /*'Rf'|| 'Db'|| 'Sg'||*/ 'Bh'|| /*'Hn'|| 'Mt'*/ ):
		else if (atom == (char)"Sc" || atom ==(char)"Ti" || atom ==(char)"V" || atom ==(char)"Cr"|| 
			atom ==(char)"Mn" || atom ==(char)"Fe" || atom ==(char)"Co" || atom ==(char)"Ni"|| 
			atom ==(char)"Cu"|| atom ==(char)"Zn"|| atom ==(char)"Y"|| atom ==(char)"Zr"|| 
			atom ==(char)"Nb"|| atom ==(char)"Mo"|| atom ==(char)"Tc"|| atom ==(char)"Ru"|| 
			atom ==(char)"Rh"|| atom ==(char)"Pd"|| atom ==(char)"Ag"|| atom ==(char)"Cd"|| 
			atom ==(char)"Hf"|| atom ==(char)"Ta"|| atom ==(char)"W"|| atom ==(char)"Re"|| 
			atom ==(char)"Os"|| atom ==(char)"Ir"|| atom ==(char)"Pt"|| atom ==(char)"Au"|| 
			atom ==(char)"Hg"|| atom ==(char)"Bh")	
		{
			//colour = {0.0f, 0.0f, 0.9f};
			glColor3f(0.0f, 0.0f, 0.9f); //blue

			switch (atom)
			{
			case ('Sc'):
				atom_radius = 184.0f;
				break;
			case ('Ti'):
				atom_radius = 176.0f;
				break;
			case ('V'):
				atom_radius = 171.0f;
				break;
			case ('Cr'):
				atom_radius = 166.0f;
				break;
			case ('Mn'):
				atom_radius = 130.0f;
				break;
			case ('Fe'):
				atom_radius = 156.0f;
				break;
			case ('Co'):
				atom_radius = 152.0f;
				break;
			case ('Ni'):
				atom_radius = 149.0f;
				break;
			case ('Cu'):
				atom_radius = 145.0f;
				break;
			case ('Zn'):
				atom_radius = 142.0f;
				break;
			case ('Y'):
				atom_radius = 212.0f;
				break;
			case ('Zr'):
				atom_radius = 206.0f;
				break;
			case ('Nb'):
				atom_radius = 198.0f;
				break;
			case ('Mo'):
				atom_radius = 190.0f;
				break;
			case ('Tc'):
				atom_radius = 183.0f;
				break;
			case ('Ru'):
				atom_radius = 178.0f;
				break;
			case ('Rh'):
				atom_radius = 173.0f;
				break;
			case ('Pd'):
				atom_radius = 169.0f;
				break;
			case ('Ag'):
				atom_radius = 165.0f;
				break;
			case ('Cd'):
				atom_radius = 161.0f;
				break;
			case ('Hf'):
				atom_radius = 208.0f;
				break;
			case ('Ta'):
				atom_radius = 200.0f;
				break;
			case ('W'):
				atom_radius = 193.0f;
				break;
			case ('Re'):
				atom_radius = 188.0f;
				break;
			case ('Os'):
				atom_radius = 185.0f;
				break;
			case ('Ir'):
				atom_radius = 180.0f;
				break;
			case ('Pt'):
				atom_radius = 177.0f;
				break;
			case ('Au'):
				atom_radius = 174.0f;
				break;
			case ('Hg'):
				atom_radius = 171.0f;
				break;
			case ('Bh'):
				atom_radius = 128.0f;
				break;
			}

			glPushMatrix();
			glTranslatef(atx*a, aty*b, atz*c);
			glutSolidSphere(atom_radius/100, 30, 30);
			//auxSolidSphere(atom_radius/100);
			glPopMatrix();

		}

		/**************************f-elements lanthanides************************/
		//case('La' || 'Ce' || 'Pr' || 'Nd'|| 'Pm' || 'Sm' || 'Eu' || 'Gd'|| 'Tb'|| 'Dy'|| 'Ho'|| 'Er'|| 'Tm'|| 'Yb'|| 'Lu' ):
		else if (atom == (char)"La" || atom == (char)"Ce" || atom == (char)"Pr" || atom == (char)"Nd"|| 
			atom == (char)"Pm" || atom == 'Sm' || atom == 'Eu' || atom == 'Gd'|| 
			atom == 'Tb'|| atom == 'Dy'|| atom == 'Ho'|| atom == 'Er'|| 
			atom == 'Tm'|| atom == 'Yb'|| atom == 'Lu' )	
		{
			//colour = {0.0f, 0.9f, 0.0f};//green
			glColor3f(0.0f, 0.9f, 0.0f);

			switch (atom)
			{
			case ('La'):
				atom_radius = 187.0f;
				break;
			case ('Ce'):
				atom_radius = 181.0f;
				break;
			case ('Pr'):
				atom_radius = 182.0f;
				break;
			case ('Nd'):
				atom_radius = 182.0f;
				break;
			case ('Pm'):
				atom_radius = 205.0f;
				break;
			case ('Sm'):
				atom_radius = 238.0f;
				break;
			case ('Eu'):
				atom_radius = 231.0f;
				break;
			case ('Gd'):
				atom_radius = 233.0f;
				break;
			case ('Tb'):
				atom_radius = 225.0f;
				break;
			case ('Dy'):
				atom_radius = 228.0f;
				break;
			case ('Ho'):
				atom_radius = 226.0f;
				break;
			case ('Er'):
				atom_radius = 226.0f;
				break;
			case ('Tm'):
				atom_radius = 222.0f;
				break;
			case ('Yb'):
				atom_radius = 222.0f;
				break;
			case ('Lu'):
				atom_radius = 217.0f;
				break;
			}
			glPushMatrix();
			glTranslatef(atx*a, aty*b, atz*c);
			glutSolidSphere(atom_radius/100, 30, 30);
			//auxSolidSphere(atom_radius/100);
			glPopMatrix();

		}

		/****************************f-elements actinoids***********************************/
		//case('Ac' || 'Th' || 'Pa' || 'U'|| 'Np' || 'Pu' || 'Am' || 'Cm'|| 'Bk'|| 'Cf'|| 'Es'|| 'Fm'|| 'Md'|| 'No' || 'Lr' ):
		else if (atom == 'Ac' || atom == 'Th' || atom == 'Pa' || atom == 'U'|| 
			atom == 'Np' || atom == 'Pu' || atom == 'Am' || atom == 'Cm'|| 
			atom == 'Bk'|| atom == 'Cf'|| atom == 'Es'|| atom == 'Fm'|| 
			atom == 'Md'|| atom == 'No' || atom == 'Lr' )	
		{	
			//colour = {0.0f, 0.7f, 0.0f}; //green
			glColor3f(0.0f, 0.7f, 0.0f);

			switch (atom)
			{
			case ('Ac'):
				atom_radius = 188.0f;
				break;
			case ('Th'):
				atom_radius = 180.0f;
				break;
			case ('Pa'):
				atom_radius = 161.0f;
				break;
			case ('U'):
				atom_radius = 138.0f;
				break;
			case ('Np'):
				atom_radius = 130.0f;
				break;
			case ('Pu'):
				atom_radius = 162.0f;
				break;
			case ('Am'):
				atom_radius = 173.0f;
				break;
			case ('Cm'):
				atom_radius = 299.0f;
				break;
			case ('Bk'):
				atom_radius = 297.0f;
				break;
			case ('Cf'):
				atom_radius = 295.0f;
				break;
			case ('Es'):
				atom_radius = 292.0f;
				break;
			case ('Fm'):
				atom_radius = 290.0f;
				break;
			case ('Md'):
				atom_radius = 287.0f;
				break;
			case ('No'):
				atom_radius = 285.0f;
				break;
			case ('Lr'):
				atom_radius = 282.0f;
				break;
			}
			glPushMatrix();
			glTranslatef(atx*a, aty*b, atz*c);
			glutSolidSphere(atom_radius/100, 30, 30);
			//auxSolidSphere(atom_radius/100);
			glPopMatrix();

		}
	
}

/************************************************************************/
/*                                                                      */
/************************************************************************/

int readstr(FILE *fp, char *str)
{
	do
	{
		char *ok = fgets(str,256,fp);
		if(!ok) return 0;
	} 
	while(str[0]=='\n');
	return 1;
}

/************************************************************************/
/*                                                                      */
/************************************************************************/

struct cell  
{
	char cell_a[256], cell_b[256], cell_c[256], num_pos[256], trash[256];
	int i;
};
struct cell c;
struct cell *cp = &c;

struct atoms
{
	char atom[26], atx[26], aty[26], atz[26];
};
struct atoms m_atoms[100];

/************************************************************************/
/*                                                                      */
/************************************************************************/

void file_open(void)
{
	/************************************************************************/
	/* OPEN FILE                                                            */
	/************************************************************************/
	FILE *fp;
	char str[256];
	int count = 0;


	
	
	fp = fopen("1.ace","r");

	while(!feof(fp))
	{
		
		if(readstr(fp,str)) count++;
		//printf("%s",str);

		if (count == 7) 
			{
				 fscanf(fp,"%*2c%s\t%*2c%s\t%*2c%s", cp->cell_a, cp->cell_b, cp->cell_c);	
			}
		if (count == 10) 
			{
				fscanf(fp, "%*28c%s", cp->num_pos);
				
			}
		if (count >= 11)
		{
			for(cp->i = 0; cp->i < atoi(cp->num_pos); cp->i++)
			{
				
				fscanf(fp, "%s\t%*c\t%s\t%s\t%s\t%s\t%s",m_atoms[cp->i].atom, m_atoms[cp->i].atx, m_atoms[cp->i].aty, m_atoms[cp->i].atz, cp->trash, cp->trash);
			}
			
		}
		
	
	}
	
	printf("\n%f\n%f\n%f\n%i", atof(cp->cell_a), atof(cp->cell_b), atof(cp->cell_c), atoi(cp->num_pos)); 
	
	for (cp->i = 0; cp->i<atoi(cp->num_pos); cp->i++ )
	{
		printf("\n%s\n%s\n%s\n%s\n", m_atoms[cp->i].atom, m_atoms[cp->i].atx, m_atoms[cp->i].aty, m_atoms[cp->i].atz);
	}

	/*for (i = 0; i<atoi(num_pos); i++ )
	{
		free (atom[i]);
		free(atx[i]);
		free(aty[i]);
		free(atz[i]);
	}
	*/

	fclose(fp);

	//getch();
	
}




HBITMAP CaptureScreen(const char *fileName)
{

	BITMAPFILEHEADER bmfh;
	BITMAPINFOHEADER bmih;
	bmfh.bfType='MB';
	bmfh.bfReserved1=0;
	bmfh.bfReserved2=0;
	bmfh.bfOffBits=54;
	glGetIntegerv(GL_VIEWPORT,viewport);
	imageSize = viewport[2]*viewport[3]*10;
	bmfh.bfSize=imageSize+sizeof(bmfh)+sizeof(bmih);
	data=(unsigned char*)malloc(imageSize);
	glPixelStorei(GL_PACK_ALIGNMENT,4);
	glPixelStorei(GL_PACK_ROW_LENGTH,0);
	glPixelStorei(GL_PACK_SKIP_ROWS,0);
	glPixelStorei(GL_PACK_SKIP_PIXELS,0);
	glPixelStorei(GL_PACK_SWAP_BYTES,1);
	glGetIntegerv(GL_READ_BUFFER,(GLint*)&lastBuffer);
	glReadBuffer(GL_FRONT);
	glReadPixels(0,0,viewport[2],viewport[3],GL_BGR_EXT,GL_UNSIGNED_BYTE,data);
	data[imageSize-1]=0;
	data[imageSize-2]=0;
	glReadBuffer(lastBuffer);
	pic=fopen(fileName,"wb");
	bmih.biSize=40;
	bmih.biWidth=viewport[2];
	bmih.biHeight=viewport[3];
	bmih.biPlanes=1;
	bmih.biBitCount=24;
	bmih.biCompression=0;
	bmih.biSizeImage=imageSize;
	bmih.biXPelsPerMeter=45089;
	bmih.biYPelsPerMeter=45089;
	bmih.biClrUsed=0;
	bmih.biClrImportant=0;
	fwrite(&bmfh,sizeof(bmfh),1,pic);
	fwrite(&bmih,sizeof(bmih),1,pic);
	fwrite(data,imageSize,1,pic);
	free(data);
	fclose(pic);
	return 0;
}

void MsgBox(/*int ID,*/ char* a, char* b, char* c , char* num_pos /*, char* atom, char* atx, char* aty, char* atz*/)
{	
	//sprintf(at_data_buf, "Object ID = %i\nCell parameters: a = %f; b = %f; c = %f\nNumber of positions in cell: %i\nAtom: %s\nX = %f; Y = %f; Z = %f", ID, atof(a), atof(b), atof(c), atoi(num_pos), atom, atof(atx), atof(aty), atof(atz));
	sprintf(at_data_buf, "Cell parameters: a = %f; b = %f; c = %f\nNumber of positions in cell: %i",atof(a), atof(b), atof(c), atoi(num_pos));
	MessageBoxA(NULL, at_data_buf, "Atom Data", NULL);
}

/************************************************************************/
/*                                                                      */
/************************************************************************/

/*Calling the drawing scene procedure*/
void Display()
{
	
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
	
	glPushMatrix();

		glScalef(mag,mag,mag);

		glRotated(rot_x, 1.0, 0.0, 0.0);
		glRotated(rot_y, 0.0, 1.0, 0.0);
	
		

	
		polygon((GLfloat)atof(cp->cell_a), (GLfloat)atof(cp->cell_b), (GLfloat)atof(cp->cell_c));
		//glInitNames();
		
		for (cp->i= 0; cp->i<= atoi(cp->num_pos); cp->i++ )
		{
			//glPushName((GLuint) cp->i);
			draw_atoms((char)m_atoms[cp->i].atom, atof(m_atoms[cp->i].atx), atof(m_atoms[cp->i].aty), atof(m_atoms[cp->i].atz), (GLfloat)atof(cp->cell_a), (GLfloat)atof(cp->cell_b), (GLfloat)atof(cp->cell_c));
			//glPopName();

		}

	glPopMatrix();

glFlush();
glutSwapBuffers(); 

}	

void motion(int ax,int ay)
{
	
	if (x != 0)
	{
		move = TRUE;
		rot_x += x - ax;
		rot_y += y - ay;
	}
	x = ax;
	y = ay;


}

void mouse (int button, int state, int ax, int ay)
{
	if (button == GLUT_LEFT_BUTTON && state == GLUT_UP)
	{
		move = FALSE;
	}

	if (button == GLUT_RIGHT_BUTTON && state == GLUT_DOWN)
	{
		move = FALSE;
		right_but_pressed = TRUE;
		right_but_x = ax;
		right_but_y = ay;
		glReadPixels( right_but_x, viewport[3]-right_but_y, 1, 1, GL_DEPTH_COMPONENT, GL_FLOAT, &right_but_z );	
		gluUnProject( right_but_x, viewport[3]-right_but_y, right_but_z, modelview, projection, viewport, &objrx, &objry, &objrz );


	}

}

void mouseWheel (int wheel, int dir, int mouseX, int mouseY)
{
	if (dir == 1)
	{
		scroll = TRUE;
		mag += 0.05;

	}
	if (dir == -1) 
	{
		scroll = TRUE;
		mag -=0.05;

	}


}


void idle()
{
	if (move == TRUE || scroll == TRUE)
	{


		glutPostWindowRedisplay(mainWindow);

	}


}

//void SelectObj (float sx, float sy)
//{
//	glSelectBuffer(128 , selectBuffer);
//
//
//	glGetIntegerv(GL_VIEWPORT, viewport);
//
//	glMatrixMode(GL_PROJECTION); 
//	glPushMatrix();
//		glRenderMode(GL_SELECT);
//		glLoadIdentity();   
//		gluPickMatrix(sx, viewport[3] - sy, 2, 2, viewport);
//		gluPerspective(60.0f, viewport[2]/viewport[3], 1.0, 30.0);
//
//		glMatrixMode(GL_MODELVIEW); 
//		Display(); 
//	
//		objectsFound = glRenderMode(GL_RENDER);
//		glMatrixMode(GL_PROJECTION);
//	glPopMatrix();	
//	glMatrixMode(GL_MODELVIEW);  
//
//	if (objectsFound > 0)
//	{
//		lowestDepth = selectBuffer[1];
//		objectID = selectBuffer[3];
//		for(j = 1; j < objectsFound; j++)
//		{
//
//			if(selectBuffer[(j * 4) + 1] < lowestDepth)
//			{
//
//				lowestDepth = selectBuffer[(j * 4) + 1];
//
//
//				objectID = selectBuffer[(j * 4) + 3];
//			}
//		}
//		glRenderMode(GL_RENDER);
//
//		return objectID;
//	}
//
//	//else 
//	//return 0;
//
//}

void ShowAtomData()
{
	//SelectObj(objrx, objry);
	//MsgBox(objectID,cp->cell_a, cp->cell_b, cp->cell_c , cp->num_pos, m_atoms[objectID].atom, m_atoms[objectID].atx, m_atoms[objectID].aty, m_atoms[objectID].atz);
	MsgBox(cp->cell_a, cp->cell_b, cp->cell_c , cp->num_pos);
}


void processMenuStatus(int status, int x, int y) {

	if (status == GLUT_MENU_IN_USE)
		menuFlag = 1;
	else
		menuFlag = 0;
}

void processMainMenu(int option)
{

	if (option == SAVE)
	{
		move = FALSE;
		CaptureScreen("pic.bmp");
	}

	if (option == SHOW)		
	{		
		move = FALSE;
		ShowAtomData( );		
	}

}

void createPopupMenus() {



	mainMenu = glutCreateMenu(processMainMenu);
	glutAddMenuEntry("Save picture to file",SAVE);
	glutAddMenuEntry("Show atom data",SHOW);


	glutAttachMenu(GLUT_RIGHT_BUTTON);

	// this will allow us to know if the menu is active
	glutMenuStatusFunc(processMenuStatus);
}

/***************************************************/

int main(int argc, char** argv)
{
	

	file_open();
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH);
	mainWindow = glutCreateWindow("ViAt");
	SetupRC();
	glutDisplayFunc(Display);
	glutReshapeFunc(Reshape);

	glutMouseFunc(mouse);
	glutMotionFunc(motion);
	glutMouseWheelFunc(mouseWheel);
	glutIdleFunc(idle);


	glEnable(GL_DEPTH_TEST);

	glEnable(GL_LIGHTING);
	glEnable(GL_LIGHT0);
	glEnable(GL_COLOR_MATERIAL);



	createPopupMenus();

	glutMainLoop();
	return 0;
}

