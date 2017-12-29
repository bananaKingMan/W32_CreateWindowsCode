//# W32_CreateWindowsCode
//createWindow steps(1 to 7)

#include <windows.h>

HINSTANCE g_hInstance = 0; // 用来接收当前程序实例句柄
//2.窗口处理函数
LRESULT CALLBACK WndProc(HWND hWnd, UINT msgID, WPARAM wParam, LPARAM lparam)
{
	switch(msgID)
	{
	case WM_DESTROY:
			PostQuitMessage(0);
			break;
	}
	return DefWindowProc(hWnd, msgID, wParam, lparam);//给各种消息进行默认处理
}
//注册窗口类
void Register(LPSTR lpClassName, WNDPROC)
{
	WNDCLASSEX wce = {0};
	wce.cbSize = sizeof(wce);//结构体大小
	wce.cbClsExtra = 0;//窗口类附加数缓冲区
	wce.cbWndExtra = 0;//窗口附加数据缓冲区
	wce.hbrBackground = (HBRUSH) (COLOR_WINDOW + 1);//背景颜色
	wce.hIcon = NULL;//图标
	wce.hCursor = NULL;//鼠标
	wce.hIconSm = NULL;//大图标
	wce.hInstance = g_hInstance;//句柄
	wce.lpfnWndProc = WndProc;//调用程序
	wce.lpszClassName = lpClassName;//窗口名字
	wce.lpszMenuName = NULL;//菜单
	wce.style = CS_HREDRAW|CS_HREDRAW;//窗口风格
	RegisterClassEx(&wce);
}
//4.创建窗口
HWND CreateMain(LPSTR lpClassName, LPSTR lpWndName)
{
	HWND hWnd = CreateWindowEx(0, lpClassName, lpWndName, WS_OVERLAPPEDWINDOW, 100, 100, 700, 500, NULL, NULL, g_hInstance, NULL);
	return hWnd;
}
//5.显示窗口
void Display(HWND hWnd)
{
	ShowWindow(hWnd, SW_SHOW);
	UpdateWindow(hWnd);
		////刷新窗口，不调用也没有关系，但是官网推荐调用

}
//6.显示循环
void Message()
{
	MSG nMsg = {0};
	while(GetMessage(&nMsg, NULL,0, 0))
	{
		TranslateMessage(&nMsg);
		DispatchMessage(&nMsg);
	}
}
//1.入口函数
int CALLBACK WinMain(HINSTANCE hIns, HINSTANCE hPreIns, LPSTR lpCmdLine, int nCmdShow)
{
	g_hInstance  = hIns;
	Register("Main", WndProc);
	HWND hWnd = CreateMain("Main", "Window");
	Display(hWnd);
	Message();
	return 0;
}
