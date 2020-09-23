<div align="center">

## Buttons, Textareas and Checkboxes


</div>

### Description

This code shows how to use buttons, editboxes and checkboxes in windows.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Jon Steffensen](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/jon-steffensen.md)
**Level**          |Beginner
**User Rating**    |4.8 (77 globes from 16 users)
**Compatibility**  |C, C\+\+ \(general\)
**Category**       |[Controls/ Forms/ Dialogs/ Menus](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/controls-forms-dialogs-menus__3-3.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/jon-steffensen-buttons-textareas-and-checkboxes__3-1835/archive/master.zip)

### API Declarations

<windows.h>


### Source Code

```
#include <windows.h>
// Define control identifiers
#define IDC_EDIT 1001
#define IDC_STATIC 1002
#define IDC_BUTTON 1003
#define IDC_CHECK 1004
static HINSTANCE hInstance = NULL;
char szClassName[] = "ControlClasses";
// A function to set a control's text to the default font
int SetDefaultFont(int identifier, HWND hwnd)
{
 SendDlgItemMessage(
 hwnd,
 identifier,
 WM_SETFONT,
 (WPARAM)GetStockObject(DEFAULT_GUI_FONT),
 MAKELPARAM(TRUE, 0));
 return 0;
}
// A function to create static text
HWND CreateStatic(char* tempText, int x, int y, int width, int height, int identifier, HWND hwnd)
{
 HWND hStaticTemp;
 hStaticTemp = CreateWindowEx(
 0,
 "STATIC",
 tempText,
 WS_CHILD | WS_VISIBLE,
 x, y,
 width, height,
 hwnd, (HMENU)identifier, hInstance, NULL);
 return hStaticTemp;
}
// A function to create a textarea
HWND CreateEdit(char* tempText, int x, int y, int width, int height, int identifier, HWND hwnd)
{
 HWND hEditTemp;
 hEditTemp = CreateWindowEx(
 WS_EX_CLIENTEDGE,
 "EDIT",
 tempText,
 WS_CHILD | WS_VISIBLE,
 x, y,
 width, height,
 hwnd, (HMENU)identifier, hInstance, NULL);
 return hEditTemp;
}
// A function to create a button
HWND CreateButton(char* tempText, int x, int y, int width, int height, int identifier, HWND hwnd)
{
 HWND hButtonTemp;
 hButtonTemp = CreateWindowEx(
 0,
 "BUTTON",
 tempText,
 WS_CHILD | WS_VISIBLE,
 x, y,
 width, height,
 hwnd, (HMENU)identifier, hInstance, NULL);
 return hButtonTemp;
}
// A function to create a checkbox
HWND CreateCheck(char* tempText, int x, int y, int width, int height, int identifier, HWND hwnd)
{
 HWND hCheckTemp;
 hCheckTemp = CreateWindowEx(
 0,
 "BUTTON",
 tempText,
 WS_CHILD | WS_VISIBLE | BS_AUTOCHECKBOX,
 x, y,
 width, height,
 hwnd, (HMENU)identifier, hInstance, NULL);
 return hCheckTemp;
}
// The window procedure
LRESULT CALLBACK WindowProcedure(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam)
{
 // Control handles
 static HWND hwndEdit;
 static HWND hwndStatic;
 static HWND hwndButton;
 static HWND hwndCheck;
 switch (message)
 {
   case WM_CREATE:
    // Creates a static text box
    hwndStatic = CreateStatic("Enter text!", 10, 10, 75, 20, IDC_STATIC, hwnd);
    SetDefaultFont(IDC_STATIC, hwnd);
    // Creates a textarea with the text "Hello" in it
    hwndEdit = CreateEdit("Hello", 10, 30, 200, 20, IDC_EDIT, hwnd);
    SetDefaultFont(IDC_EDIT, hwnd);
    // Creates a button labeled "Click me!"
    hwndButton = CreateButton("Click me!", 10, 55, 100, 25, IDC_BUTTON, hwnd);
    SetDefaultFont(IDC_BUTTON, hwnd);
    // Creates a checkbox
    hwndCheck = CreateCheck("Delete text?", 130, 60, 75, 20, IDC_CHECK, hwnd);
    SetDefaultFont(IDC_CHECK, hwnd);
   break;
   case WM_COMMAND:
    // If a button is clicked...
    if(HIWORD(wParam) == BN_CLICKED)
    {
     switch(LOWORD(wParam))
     {
      // ...And the button is the one with the identifier IDC_BUTTON...
      case IDC_BUTTON:
       int tempEditLength = GetWindowTextLength(hwndEdit);
       char tempEditText[tempEditLength + 1];
       LRESULT checkState;
       // ...Then get the text from the textarea...
       GetWindowText(hwndEdit, tempEditText, tempEditLength + 1);
       tempEditText[tempEditLength] = '\0';
       // ...And display a messagebox with the text in it!
       MessageBox(hwnd, tempEditText, "The text entered is:", MB_OK);
       // Check the checkbox...
       checkState = SendDlgItemMessage(
       hwnd,
       IDC_CHECK,
       BM_GETCHECK,
       0, 0);
       // ...And if it's checked...
       if(checkState == BST_CHECKED)
       {
        // ...Empty the textarea!
        SetWindowText(hwndEdit, "");
       }
      break;
     }
    }
   break;
   case WM_DESTROY:
   PostQuitMessage(0);
   break;
   default:
   return DefWindowProc(hwnd, message, wParam, lParam);
 }
 return 0;
}
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpszArgument, int nFunsterStil)
{
 HWND hwnd;
 MSG messages;
 WNDCLASSEX wincl;
 wincl.hInstance = hInstance;
 wincl.lpszClassName = szClassName;
 wincl.lpfnWndProc = WindowProcedure;
 wincl.style = CS_DBLCLKS;
 wincl.cbSize = sizeof(WNDCLASSEX);
 wincl.hIcon = LoadIcon(NULL, IDI_APPLICATION);
 wincl.hIconSm = LoadIcon(NULL, IDI_APPLICATION);
 wincl.hCursor = LoadCursor(NULL, IDC_ARROW);
 wincl.lpszMenuName = NULL;
 wincl.cbClsExtra = 0;
 wincl.cbWndExtra = 0;
 wincl.hbrBackground = (HBRUSH) GetStockObject(LTGRAY_BRUSH);
 if(!RegisterClassEx(&wincl)) return 0;
 hwnd = CreateWindowEx(
   0,
   szClassName,
   "Control-classes",
   WS_OVERLAPPED | WS_CAPTION | WS_SYSMENU | WS_MINIMIZEBOX,
   CW_USEDEFAULT,
   CW_USEDEFAULT,
   250,
   120,
   HWND_DESKTOP,
   NULL,
   hInstance,
   NULL
   );
 ShowWindow(hwnd, nFunsterStil);
 while(GetMessage(&messages, NULL, 0, 0))
 {
   TranslateMessage(&messages);
   DispatchMessage(&messages);
 }
 return messages.wParam;
}
```

