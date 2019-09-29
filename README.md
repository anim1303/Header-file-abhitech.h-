#ifndef CODES_CONIO
#define CODES_CONIO
#include<stdio.h>
#include<stdlib.h>
#include<windows.h>
#include<conio.h>


void delay(int x)
{
    Sleep(x);
}


void clrscr()
{
    system("cls");

}



int x=40;
COORD coord={0,0};
void gotoxy (int x, int y)
{
        coord.X = x;
        coord.Y = y; // X and Y coordinates
        SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord);
}

void setColors(int ForgC, int BackC)
{
     WORD wColor = ((BackC & 0x0F) << 4) + (ForgC & 0x0F);
     SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), wColor);
     return;
}

void textcolor(int ForgC)
{
     WORD wColor;
                          //We will need this handle to get the current background attribute
     HANDLE hStdOut = GetStdHandle(STD_OUTPUT_HANDLE);
     CONSOLE_SCREEN_BUFFER_INFO csbi;

                           //We use csbi for the wAttributes word.
     if(GetConsoleScreenBufferInfo(hStdOut, &csbi))
     {
        int x=0;
        //for(x=0;x<=16;x++)
        {
          wColor = (csbi.wAttributes & 0xF0) + (ForgC & 0x0F) ; // + (1<<x);
          SetConsoleTextAttribute(hStdOut, wColor);
          //printf("\nHELLO THERE  %d ",wColor);
         // _getch();
        }
     }
     return;
}

WORD getColors()
{
    HANDLE    m_hConsole;
    WORD      m_currentConsoleAttr;
    CONSOLE_SCREEN_BUFFER_INFO   csbi;

    ///retrieve and save the current attributes
     m_hConsole=GetStdHandle(STD_OUTPUT_HANDLE);
      if(GetConsoleScreenBufferInfo(m_hConsole, &csbi))
                  m_currentConsoleAttr = csbi.wAttributes;
                     return m_currentConsoleAttr ;
}


void textbackground(int bg)
{
    WORD w ;
    int fr,bk;
    w = getColors();

    fr = w & 0x0F ;
    bk = (bg  & 0x0F) << 4 ;


    setColors(fr,bg);
}



void BOX(int sr,int sc,int er,int ec,int fg,int bg)
{
    int r,c;
    setColors(fg,bg);
    textcolor(fg);
    gotoxy(sc,sr);
    printf("%c",201);
    for(c=sc+1;c<ec;c++)
        printf("%c",205);

    printf("%c",187);
    gotoxy(sc,er);
    printf("%c",200);
    for(c=sc+1;c<ec;c++)
        printf("%c",205);

    printf("%c",188);

    for(r=sr+1;r<er;r++)
    {
        gotoxy(sc,r);
        printf("%c",186);
        gotoxy(ec,r);
        printf("%c",186);
    }

    for(r=sr+1;r<er;r++)
    {
        gotoxy(sc+1,r);
        for(c=sc+1;c<ec;c++)
            printf(" ");
    }
}

void box1(int sr,int sc,int er,int ec,int fg,int bg)
{
    int r,c;
    setColors(fg,bg);
    textcolor(fg);
    gotoxy(sc,sr);
    printf("%c",218);
    for(c=sc+1;c<ec;c++)
        printf("%c",196);

    printf("%c",191);
    gotoxy(sc,er);
    printf("%c",192);
    for(c=sc+1;c<ec;c++)
        printf("%c",196);

    printf("%c",217);

    for(r=sr+1;r<er;r++)
    {
        gotoxy(sc,r);
        printf("%c",179);
        gotoxy(ec,r);
        printf("%c",179);
    }

    for(r=sr+1;r<er;r++)
    {
        gotoxy(sc+1,r);
        for(c=sc+1;c<ec;c++)
            printf(" ");
    }
}

void textrc(char *A,int rn,int cn,int fg,int bg,int hot) // char text[]
{
    int x;
    gotoxy(cn,rn);
    for(x=0;A[x]!='\0';x++)
    {
        if(A[x]=='^')
        {  /// either HOT or ^ to be printed
            x++;
            if(A[x]=='^')
                setColors(fg,bg);
            else
                setColors(hot,bg);
        }
        else
        {
            setColors(fg,bg);
        }

        printf("%c",A[x]);
    }

}


int showMenu( char a[][40], int RN,int TR,int TC,int CUR,int fg,int bg ,int hot)
{
      int sr,sc,er,ec,r;
      char ch;
      char str[50];

      sr = TR/2 -  RN/2 ; // 18
      er = sr + RN -1 ;
      sc = TC/2 - 20 ;
      ec = TC/2 + 20 ;

      BOX(sr+1,sc+2,er+1,ec+2,0,0);
      BOX(sr,sc,er,ec,bg,bg);

      do
      {
            for(r=0;r<RN;r++)
            {
                sprintf(str,"%-41s",a[r]);
                if(r==CUR)
                     textrc(str,sr+r,sc, fg,hot,bg);
                else
                {
                     textrc(str,sr+r,sc, fg,bg,hot);
                }


            }

           fflush(stdin);
           ch=_getch();
           if(ch==0||ch==-32)
           {
                ch=_getch();
                switch(ch)
                {
                      case 72:
                      case 75:
                          CUR--;
                          if(CUR==-1)
                              CUR=RN-1;
                          continue;
                      case 77:
                      case 80:
                           CUR ++;
                           if(CUR==RN)
                              CUR=0;
                           continue;
                }

           }
           else if(ch=='\n'||ch=='\r')
           {
                 break;
           }


      }while(1);

      return CUR;


}

void Screen()
{

    setColors(15,15)   ;
    clrscr();
    BOX(1,1,56,168,15,1);

    gotoxy(5,0);
    setColors(5,15);
    printf(" EK DHANSU PROGRAM");
    setColors(4,15);
    gotoxy(150,57);
    printf(" ANITech Creations");

}


void cursor_hide(void)
{
    HANDLE hOut;
    CONSOLE_CURSOR_INFO ConCurInf;

    hOut=GetStdHandle(STD_OUTPUT_HANDLE);

    ConCurInf.dwSize=x;
    ConCurInf.bVisible=FALSE;

    SetConsoleCursorInfo(hOut, &ConCurInf);
}

void cursor_show(void)
{
    HANDLE hOut;
    CONSOLE_CURSOR_INFO ConCurInf;
    hOut=GetStdHandle(STD_OUTPUT_HANDLE);

    ConCurInf.dwSize=x;
    ConCurInf.bVisible=TRUE;

    SetConsoleCursorInfo(hOut, &ConCurInf);
}


void screen()
{
    setColors(4,15);
    clrscr();

    textrc("^Library ^Management ^System",0,55,  1,15,4);
    BOX(1,0,42,135,15,1);
    gotoxy(117,41);
    setColors(14,2);
    printf("AbhiTech Creations");

}
#ifndef __RANDOM___
#define __RANDOM___
#include<stdlib.h>
#include<time.h>
    void randomize()
    {

        time_t t;
        time(&t);
        srand(t);
    }

   long int random(long int x)
   {
       return rand() % x;
   }
#endif


#endif
