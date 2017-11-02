# WinAPI
## Prosty Quiz oparty na około 8 pytaniach
## założenia
    - bez zliczarki punktów
    - zła odpowiedź = koniec quizu = przegrana
    - w jednym okienku znajduje się pytanie i 4 odpowiedzi (3 złe, 1 prawidłowa)
   

#include <windows.h>

LPSTR NazwaKlasy = "Klasa123"; /*zmienna globalna*/
MSG Komunikat; /*zmienna globalna typu MSG*/

#define ID_PRZYCISK1 501

HWND hButton1;
HWND hButton2;
HWND hButton3;
HWND hButton4;

/*statyczne elementy - kontrolki z którymi użytkownik
nie może nic zrobić - tekst*/
HWND hStatic;
HWND hIcon;


LRESULT CALLBACK WndProc (HWND hwnd, UINT msg,
                          WPARAM wParam,
                          LPARAM lParam );
/*jest to mała funkcja, która obsługuje komunikatów,
ktore sie pojawiaja w naszym oknie,
potem są otrzymywane przez msg*/

int WINAPI WinMain(HINSTANCE hInstance,
                   /*uchwyt do instancji, numerek jej wywołania -
                   10 otwartych programów i kazdy ma inny numerek*/
                   HINSTANCE hPrevInstance,
                   /*uchwyt do poprzedniej instancji, nie używa się*/
                   LPSTR lpCmdLine,
                   /*zawiera linię poleceń z jakich został uruchomiony nasz program
                   LPSTR - wskaźnik na char*/
                   int nCmdShow)
                   /*określa stan okna programu*/
{

    WNDCLASS wc;

    /* WYPELNIANIE KLASY OKNA */
    wc.style = 0;
    wc.lpfnWndProc = WndProc;
    wc.cbClsExtra = 0;
    wc.cbWndExtra = 0;
    wc.hInstance = hInstance;
    wc.hIcon = LoadIcon( NULL, IDI_APPLICATION );
    wc.hCursor = LoadCursor( NULL, IDC_ARROW );
    wc.hbrBackground =( HBRUSH )( COLOR_WINDOW + 1 );
    wc.lpszMenuName = NULL;
    wc.lpszClassName = NazwaKlasy;

    /* REJESTROWANIE KLASY OKNA */
    RegisterClass(&wc);

    /* TWORZENIE OKNA funkcja zwraca uchwyt do okna */
    HWND hwnd = CreateWindowEx (WS_EX_CLIENTEDGE,
                               NazwaKlasy, "PyrQuiz",
                               WS_OVERLAPPEDWINDOW,
                               CW_USEDEFAULT,
                               /*pozycja okna domyslna*/
                               CW_USEDEFAULT,
                               650, 350,
                               NULL, NULL,
                               /*uchwyt do okna rodzicielskiego,
                               uchwyt do okna menu*/
                               hInstance, NULL);

      MessageBox( NULL, "Hej! Chcesz zagrac w PyrQuiz? Zobaczmy jak dobrze znasz Pyrkon! ;)", "PyrQuiz", NULL );

      /* TWORZENIE STATIC */
      HWND hStatic = CreateWindowEx
                     (0, "STATIC", NULL, WS_CHILD | WS_VISIBLE | SS_CENTER,
                      125, 10, 250, 25, hwnd, NULL, hInstance, NULL);

    /* USTAWIANIE TEKSTU */
    SetWindowText(hStatic, "Gdzie aktualnie odbywa się Pyrkon?");

        /* TWORZENIE PRZYCISKOW */
        hButton1 = CreateWindowEx( WS_EX_CLIENTEDGE, "BUTTON", "Ocean Spokojny", WS_CHILD | WS_VISIBLE,
         50, 50, 150, 30, hwnd, NULL, hInstance, NULL );

        hButton2 = CreateWindowEx( WS_EX_CLIENTEDGE, "BUTTON", "Sopot", WS_CHILD | WS_VISIBLE,
         300, 50, 150, 30, hwnd, NULL, hInstance, NULL );

        hButton3 = CreateWindowEx( WS_EX_CLIENTEDGE, "BUTTON", "Malta", WS_CHILD | WS_VISIBLE,
         50, 100, 150, 30, hwnd, NULL, hInstance, NULL );

        hButton4 = CreateWindowEx( WS_EX_CLIENTEDGE, "BUTTON", "Okoń", WS_CHILD | WS_VISIBLE,
         300, 100, 150, 30, hwnd, (HMENU)ID_PRZYCISK1, hInstance, NULL );


        /* WYSWIETLENIE OKNA */
        ShowWindow(hwnd, nCmdShow);
        UpdateWindow(hwnd);


        /* PETLA KOMUNIKATOW -
    prosta petla, ktora przechwytuje komunikaty
    wysylane przez system do naszego okienka*/

        while (GetMessage(&Komunikat, NULL, 0, 0))
        /*pobiera komunikaty z kolejki komunikatow*/
        {
        TranslateMessage(&Komunikat);
        DispatchMessage(&Komunikat);
        }
    return Komunikat.wParam;
}


/* PROCEDURA OKNA */
LRESULT CALLBACK WndProc (HWND hwnd, UINT msg,
                          WPARAM wParam,
                          LPARAM lParam)
 {
    switch(msg)
    {
        case WM_COMMAND:
            if((HWND) lParam == hButton4)
                MessageBox (hwnd, "Brawo! Jedziemy dalej!", "Ideolo" , MB_OK);
            if((HWND) lParam == hButton3)
                MessageBox (hwnd, "Niestety, ale nie - próbuj dalej", "Nope", MB_OK);
            if((HWND) lParam == hButton2)
                MessageBox (hwnd, "Niestety, ale nie - próbuj dalejś", "Nope", MB_OK);
            if((HWND) lParam == hButton1)
                MessageBox (hwnd, "Niestety, ale nie - próbuj", "Nope", MB_OK);
        break;
            switch(wParam)
            {
            case ID_PRZYCISK1:
                MessageBox (0, "Brawo! Jedziemy dalej!", "Ideolo" , MB_OK);
            }

        case WM_CLOSE:
            DestroyWindow(hwnd);
            break;

        case WM_DESTROY:
            PostQuitMessage(0);
            break;

        default:
            return DefWindowProc (hwnd, msg, wParam, lParam);
    }
    return 0;
 }
