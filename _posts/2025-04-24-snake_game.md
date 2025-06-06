---
layout:       post
title:        "贪吃蛇游戏源代码 source code for Snake game "
subtitle:     "source code for Snake game"
date:         2025-04-24 14:08:00
author:       "xuepro"
header-img:   "img/home_bg.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - it
---

## 贪吃蛇游戏源代码 source code for Snake game

《现代C++编程实战：从入门到应用》第8章：“8.13 实战：面向对象游戏：基于链表的贪吃蛇游戏”

"Modern C++ Programming Practice: From Entry to Application" Chapter 8: "8.13 Practice: Object-oriented Games: Snake Game Based on Linked Lists"

《现代C++编程实战：从入门到应用》 中文版电子书：[leanpub.com/c01](leanpub.com/c01)

"Modern C++ Programming Practice: From Beginners to Applications" English e-book: [https://leanpub.com/m_cpp](https://leanpub.com/m_cpp)

### snake_game.cpp
```cpp
#include "ChGL.hpp"

class BackGround {
    Color top_color{ ' ' }, bottom_color{ '_' }, side_color{ '|' };
public:
    void draw(ChGL& window) {
        auto w = window.getWidth();
        auto h = window.getHeight();
        int right = w - 1;
        int bottom = h - 1;
        for (int x = 0; x < w; x++) {
            window.setPixel(x, 0, top_color);
            window.setPixel(x, bottom, bottom_color);
        }      
        for (int y = 0; y < h; y++) {
            window.setPixel(0, y, side_color);          
            window.setPixel(right, y, side_color);
        }
    }
};

class Snake;
class Egg;

//#define version_bg
#ifdef version_bg
class Snake;
class Egg;

class GameEngine {
    ChGL* window{ nullptr };
    BackGround bg;         // 游戏画布
    Snake* snake{ nullptr }; // 蛇
    Egg* egg{ nullptr };     // 鸡蛋
    bool running{ true };
    bool start{ false };
    InputHandler input;
public:
    GameEngine(int w = 50, int h = 20);
    ~GameEngine() {
        if (window) delete window;
    }
    void run() {
        while (running) {
            processEvent();
            update();
            collision();
            render();
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
        }
    }
private:
    void processEvent();
    void update();
    void collision();
    void drawScene();
    void render() {
        if (!running or !window) return;
        window->clear();
        drawScene();
        window->show();
    }
};
GameEngine::GameEngine(const int w, const int h) {
    window = new ChGL(w, h);
};
void GameEngine::processEvent() {}
void GameEngine::update() {}
void GameEngine::collision() {}
void GameEngine::drawScene() {
    bg.draw(*window);
}

#endif

//#define version_egg_snake
#ifdef version_egg_snake

#include <chrono>
#include <random>
class Random {
    std::mt19937 rng;
public:
    Random() : rng(std::random_device{}()) {}
    int get(int min, int max) {
        std::uniform_int_distribution<int> dist(min, max);
        return dist(rng);
    }
};

class Egg {
    int x, y;
    int size{ 1 };
    Color color{ 'G' };
    Random rand;
public:
    Egg(int w, int h, Color color = 'G', int s = 1)
        :size{ s }, color(color) {
        x = rand.get(2, w - 2);
        y = rand.get(2, h - 2);
    }
    void draw(ChGL& window) {
        window.setPixel(x, y, color);
    }
    Color getColor() const { return color; }
    int getX() const { return x; }
    int getY() const { return y; }
};

//表示一个位置
class Position {
    int x{ 0 }, y{ 0 };
public:
    Position(int x = 0, int y = 0) :x{ x }, y{ y } {}
    void set_x(int x) { this->x = x; }
    void set_y(int y) { this->y = y; }
    auto get_x() { return x; }
    auto get_y() { return y; }
};

//一个蛇身像素在内存中的结点表示
class SnakeNode {
    Position pos{};                      //蛇身像素位置
    SnakeNode* next{ nullptr };            //下一个蛇身像素结点的指针
public:
    SnakeNode(const Position pos, SnakeNode* n = nullptr)
        :pos{ pos }, next{ n } {
    }
    Position get_pos() { return pos; }
    SnakeNode* get_next() { return next; }   //返回该结点的next值，即指向下一个结点的指针
    void set_next(SnakeNode* n) { next = n; } //修改next值
};

class Snake {
    //蛇身用2个结点指针变量分别指向链表的头结点和尾结点。
    SnakeNode* head{ nullptr }, * tail{ nullptr };
    int direction{ 0 }; // 0:上, 1:下, 2:左, 3:右
    Color body_color{ 'o' }, head_color{ '@' };
    bool dead{ false };
    bool eating{ false };
    int width{ 0 }, height{ 0 };
    Random rand; // 随机数生成器    
public:
    //初始化窗口范围[width，height]里指定长度的一条蛇
    Snake(int width, int height, int length = 3,
        Color body_color = 'o', Color head_color = '@');

    void draw(ChGL& window);              //在画布window上绘制自己形状

    //沿direction方向前进，前进过程中需要检查是否发生了碰撞
    void move();
    void eat(bool eating);                  //吃了一个鸡蛋
    void set_direction(int d) {           //设置蛇的运动方向
        direction = d;
    }
    SnakeNode* get_head() { return head; } //返回链表的头结点
    SnakeNode* get_tail() { return tail; } //返回链表的尾结点
    Color get_body_color() { return body_color; }
    Color get_head_color() { return head_color; }
    bool is_dead() { return dead; }
};

Snake::Snake(int width, int height, int length, Color body_color, Color head_color)
    :width{ width }, height{ height }, body_color{ body_color }, head_color{ head_color } {
    int x_min = length + 1, x_max = width - x_min;
    int y_min = length + 1, y_max = height - y_min;
    //生成随机的蛇的位置
    int x = rand.get(x_min, x_max);
    int y = rand.get(y_min, y_max);

    tail = new SnakeNode(Position(x, y));
    head = new SnakeNode(Position(), tail);
    direction = rand.get(0, 3);
    //蛇的前进方向正好与蛇头到蛇尾的方向相反
    for (int i = 1; i < length; i++) {
        if (direction == 0) y++;      //蛇头向上，蛇身向下
        else if (direction == 1) y--; //蛇头向下，蛇身向上
        else if (direction == 2) x++; //蛇头向左，蛇身向右
        else x--;                     //蛇头向右，蛇身向左
        SnakeNode* p = new SnakeNode(Position(x, y), head->get_next());
        head->set_next(p);
    }
}

void Snake::draw(ChGL& window) {
    SnakeNode* p = head->get_next();
    while (p != tail) {                          //遍历每个蛇身结点
        window.setPixel(p->get_pos().get_x(),
            p->get_pos().get_y(), body_color);  //在画布中设置这个蛇身像素结点的颜色
        p = p->get_next();                    //指针p向后移动，指向下一个结点
    }
    window.setPixel(p->get_pos().get_x(),        //在画布上绘制蛇头结点
        p->get_pos().get_y(), head_color);
}


class GameEngine {
    ChGL* window{ nullptr };
    BackGround bg;         // 游戏画布
    Snake* snake{ nullptr }; // 蛇
    Egg* egg{ nullptr };     // 鸡蛋
    bool running{ true };
    bool start{ false };
    InputHandler input;
public:
    GameEngine(int w = 50, int h = 20);
    ~GameEngine() {
        if (window) delete window;
    }
    void run() {
        while (running) {
            processEvent();
            update();
            collision();
            render();
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
        }
    }
private:
    void processEvent();
    void update();
    void collision();
    void drawScene();
    void render() {
        if (!running or !window) return;
        window->clear();
        drawScene();
        window->show();
    }
};
GameEngine::GameEngine(int w, int h) {
    window = new ChGL(w,h);   
    snake = new Snake(w, h, 4);
    egg = new Egg(w, h);
}
void GameEngine::processEvent() {}
void GameEngine::update() {}
void GameEngine::collision() {}
void GameEngine::drawScene() {
    bg.draw(*window);
    if (snake) snake->draw(*window);
    if (egg) egg->draw(*window);
}


#endif


#define version_final
#ifdef version_final

#include <chrono>
#include <random>
class Random {
    std::mt19937 rng;
public:
    Random() : rng(std::random_device{}()) {}
    int get(int min, int max) {
        std::uniform_int_distribution<int> dist(min, max);
        return dist(rng);
    }
};


class Egg {
    int x, y;
    int size{ 1 };
    Color color{ 'G' };
    Random rand;
public:
    Egg(int w, int h, Color color = 'G', int s = 1)
        :size{ s }, color(color) {
        x = rand.get(2, w - 2);
        y = rand.get(2, h - 2);
    }
    void draw(ChGL& window) {
        window.setPixel(x, y, color);
    }
    Color getColor() const { return color; }
    int getX() const { return x; }
    int getY() const { return y; }
};

//表示一个位置
class Position {
    int x{ 0 }, y{ 0 };
public:
    Position(int x = 0, int y = 0) :x{ x }, y{ y } {}
    void set_x(int x) { this->x = x; }
    void set_y(int y) { this->y = y; }
    auto get_x() { return x; }
    auto get_y() { return y; }
};

//一个蛇身像素在内存中的结点表示
class SnakeNode {
    Position pos{};                      //蛇身像素位置
    SnakeNode* next{ nullptr };            //下一个蛇身像素结点的指针
public:
    SnakeNode(const Position pos, SnakeNode* n = nullptr)
        :pos{ pos }, next{ n } {
    }
    Position get_pos() { return pos; }
    SnakeNode* get_next() { return next; }   //返回该结点的next值，即指向下一个结点的指针
    void set_next(SnakeNode* n) { next = n; } //修改next值
};

class Snake {
    //蛇身用2个结点指针变量分别指向链表的头结点和尾结点。
    SnakeNode* head{ nullptr }, * tail{ nullptr };
    int direction{ 0 }; // 0:上, 1:下, 2:左, 3:右
    Color body_color{ 'o' }, head_color{ '@' };
    bool dead{ false };
    int width{ 0 }, height{ 0 };
    bool eating{ false };

    Random rand; // 随机数生成器    
public:
    //初始化窗口范围[width，height]里指定长度的一条蛇
    Snake(int width, int height, int length = 3,
        Color body_color = 'o', Color head_color = '@');

    void draw(ChGL& window);              //在画布window上绘制自己形状

    //沿direction方向前进，前进过程中需要检查是否发生了碰撞
    void move();
    void eat(bool eating);                  //吃了一个鸡蛋
    void set_direction(int d) {           //设置蛇的运动方向
        direction = d;
    }
    SnakeNode* get_head() { return head; } //返回链表的头结点
    SnakeNode* get_tail() { return tail; } //返回链表的尾结点
    Color get_body_color() { return body_color; }
    Color get_head_color() { return head_color; }
    bool is_dead() { return dead; }
};

Snake::Snake(int width, int height, int length, Color body_color, Color head_color)
    :width{ width }, height{ height }, body_color{ body_color }, head_color{ head_color } {
    int x_min = length + 1, x_max = width - x_min;
    int y_min = length + 1, y_max = height - y_min;
    //生成随机的蛇的位置
    int x = rand.get(x_min, x_max);
    int y = rand.get(y_min, y_max);

    tail = new SnakeNode(Position(x, y));
    head = new SnakeNode(Position(), tail);

    direction = rand.get(0, 3);
    //蛇的前进方向正好与蛇头到蛇尾的方向相反
    for (int i = 1; i < length; i++) {
        if (direction == 0) y++;      //蛇头向上，蛇身向下
        else if (direction == 1) y--; //蛇头向下，蛇身向上
        else if (direction == 2) x++; //蛇头向左，蛇身向右
        else x--;                     //蛇头向右，蛇身向左
        SnakeNode* p = new SnakeNode(Position(x, y), head->get_next());
        head->set_next(p);
    }

}

void Snake::draw(ChGL& window) {
    SnakeNode* p = head->get_next();
    while (p != tail) {                          //遍历每个蛇身结点
        window.setPixel(p->get_pos().get_x(),
            p->get_pos().get_y(), body_color);  //在画布中设置这个蛇身像素结点的颜色
        p = p->get_next();                    //指针p向后移动，指向下一个结点
    }
    window.setPixel(p->get_pos().get_x(),        //在画布上绘制蛇头结点
        p->get_pos().get_y(), head_color);
}

void Snake::eat(bool eating) {
    this->eating = eating;
}

void Snake::move() {
    if (dead) return;
    Position head_pos = tail->get_pos(); //当前蛇头位置，注意：链表尾部表示蛇头
    int x = head_pos.get_x(), y = head_pos.get_y();
    if (direction == 0) y--;       //向上移动，y--
    else if (direction == 1) y++;  //向下移动，y++
    else if (direction == 2) x--;   //左键-->向左移动,x--
    else x++;                       //右键-->向右移动，
    if (x <= 0 || x >= width - 1 || y <= 0 || y >= height - 1) {
        dead = true;
        return;
    }
    //创建新的蛇头，加入到链表的尾部。
    SnakeNode* p = new SnakeNode(Position(x, y));//创建新的结点
    tail->set_next(p); //p加到尾结点(tail)的后面，即蛇头结点的后面
    tail = p; // p成为新的链表尾结点。即p成为了新蛇头结点
    if (!eating) {//如果没有吃鸡蛋，则删除蛇尾结点
        p = head->get_next();
        head->set_next(p->get_next());
        delete p;
    }
    else {
        //否则，正吃了一个鸡蛋，不用删除蛇尾结点。但应清空吃蛋标志
        eating = false;
    }
}

class GameEngine {
    ChGL* window{ nullptr };
    BackGround bg;         // 游戏画布
    Snake* snake{ nullptr }; // 蛇
    Egg* egg{ nullptr };     // 鸡蛋
    bool running{ true };
    bool start{false};
    InputHandler input;
public:
    GameEngine(int w = 50, int h = 20);
    ~GameEngine() {
        if (window) delete window;
    }
    void run() {
        while (running) {
            processEvent();
            update();
            collision();
            render();
            std::this_thread::sleep_for(std::chrono::milliseconds(300));
        }
    }
private:
    void processEvent();
    void update();
    void collision();
    void drawScene();
    void render() {
        if (!running or !window) return;
        window->clear();
        drawScene();
        window->show();
    }
};

GameEngine::GameEngine(int w, int h) {
    window = new ChGL(w, h);
    snake = new Snake(w, h, 4);
    egg = new Egg(w, h);
}

void GameEngine::drawScene() {
    bg.draw(*window);
    if (snake) snake->draw(*window);
    if (egg) egg->draw(*window);
}

// 方向键定义（跨平台）
#ifdef _WIN32
#define KEY_UP 72
#define KEY_DOWN 80
#define KEY_LEFT 75
#define KEY_RIGHT 77
#else
#define KEY_UP 'A' // Linux下用小写w/a/s/d模拟方向键
#define KEY_DOWN 'B'
#define KEY_LEFT 'D'
#define KEY_RIGHT 'C'
#endif

void GameEngine::processEvent() {
    if (input.kbhit()) {
        char key = input.getch();
        if (key == 27) running = false;
        else if (key == ' ') start = !start;
        else {
            start = true;
            if (key == KEY_UP) snake->set_direction(0);
            else if (key == KEY_DOWN) snake->set_direction(1);
            else if (key == KEY_LEFT) snake->set_direction(2);
            else if (key == KEY_RIGHT) snake->set_direction(3);
        }
    }
}

void GameEngine::update() {
    if (start && !snake->is_dead()) snake->move();
}

void GameEngine::collision() {  
    if (!start ) return;
    auto tail = snake->get_tail();
    auto pos = tail->get_pos();
    int x = pos.get_x(), y = pos.get_y();

    if (x == 0 || y == 0 || x == window->getWidth() - 1
        || y == window->getHeight() - 1) {
        running = false; //超出窗口，蛇死亡,游戏结束
        return;
    }

    Color color = window->getPixel(x, y);
    Color bg_color = window->getClearColor();
    if (color == bg_color) return;

    if (color != snake->get_head_color()) {
        if (egg && color == egg->getColor()) {
            snake->eat(true);
            delete egg;
            egg = new Egg(window->getWidth(), window->getHeight());
        }
        else {
            running = false; // 撞墙或自身
        }
    }
}

#endif

int main() {
    GameEngine game;
    game.run();
}
```

### 面向对象的控制台字符图形库ChGL

ChGL.hpp
```
#ifndef ChGL_HPP
#define ChGL_HPP

#include <iostream>
#include <thread>
#include <chrono>
#include <string>

#ifdef _WIN32
#include <windows.h>
#include <conio.h>
#else
#include <termios.h>
#include <unistd.h>
#include <fcntl.h>
#endif

// 定义颜色类型（字符表示）
using Color = char;

// 光标控制类
class CursorController {
public:
    void showCursor() {
#ifdef _WIN32
        HANDLE console = GetStdHandle(STD_OUTPUT_HANDLE);
        CONSOLE_CURSOR_INFO cursorInfo;
        GetConsoleCursorInfo(console, &cursorInfo);
        cursorInfo.bVisible = TRUE;
        SetConsoleCursorInfo(console, &cursorInfo);
#else
        write(STDOUT_FILENO, "\033[?25h", 6); // 显示光标
#endif
    }

    void hideCursor() {
#ifdef _WIN32
        HANDLE console = GetStdHandle(STD_OUTPUT_HANDLE);
        CONSOLE_CURSOR_INFO cursorInfo;
        GetConsoleCursorInfo(console, &cursorInfo);
        cursorInfo.bVisible = FALSE;
        SetConsoleCursorInfo(console, &cursorInfo);
#else
        write(STDOUT_FILENO, "\033[?25l", 6); // 隐藏光标
#endif
    }
};

// 输入处理类（独立模块）
class InputHandler {
public:
    bool kbhit() {
#ifdef _WIN32
        return _kbhit();
#else
        struct termios oldt, newt;
        int ch, oldf;
        tcgetattr(STDIN_FILENO, &oldt);
        newt = oldt;
        newt.c_lflag &= ~(ICANON | ECHO);
        tcsetattr(STDIN_FILENO, TCSANOW, &newt);
        oldf = fcntl(STDIN_FILENO, F_GETFL, 0);
        fcntl(STDIN_FILENO, F_SETFL, oldf | O_NONBLOCK);
        ch = getchar();
        tcsetattr(STDIN_FILENO, TCSANOW, &oldt);
        fcntl(STDIN_FILENO, F_SETFL, oldf);
        if (ch != EOF) {
            ungetc(ch, stdin);
            return true;
        }
        return false;
#endif
    }

    char getch() {
#ifdef _WIN32
        return _getch();
#else
        struct termios oldt, newt;
        char ch;
        tcgetattr(STDIN_FILENO, &oldt);
        newt = oldt;
        newt.c_lflag &= ~(ICANON | ECHO);
        tcsetattr(STDIN_FILENO, TCSANOW, &newt);
        ch = getchar();
        tcsetattr(STDIN_FILENO, TCSANOW, &oldt);
        return ch;
#endif
    }
};

// 核心图形库类
class ChGL {
private:
    Color* framebuffer;          // 帧缓冲区
    int width;                   // 窗口宽度
    int height;                  // 窗口高度
    Color clear_color;           // 清屏颜色
    CursorController cursor;     // 光标控制器 

public:
    ChGL(int w, int h, Color clear_color = ' ') : width{ w }, height{h},clear_color(clear_color) {
        framebuffer = new Color[w * h];
        if (!framebuffer) return;
        width = w;
        height = h;
        clear();
        cursor.hideCursor(); // 初始化时隐藏光标
    }

    ~ChGL() {
        // 销毁窗口
        if (framebuffer) {
            delete[] framebuffer;
            framebuffer = nullptr;
        }
        width = 0;
        height = 0;
        cursor.showCursor(); // 销毁时恢复光标
    }

    // 清空帧缓冲区
    void clear() {
        auto frame_size = width * height;
        for (int k = 0; k != frame_size; k++)
            framebuffer[k] = clear_color;
    }

    // 显示帧缓冲区
    void show() {
#ifdef _WIN32       
        HANDLE hConsole = GetStdHandle(STD_OUTPUT_HANDLE);
        SetConsoleCursorPosition(hConsole, COORD{ 0,0 });// 将光标移动到 (0,0)
        DWORD written;
        for (int y = 0; y < height; y++) {
            WriteConsoleOutputCharacterA(
                hConsole,
                &framebuffer[y * width],
                width,
                { 0, (SHORT)y },
                &written
            );
        }
#else
        //std::cout << "\033[H";  // 将光标移动到 (0,0)
        write(STDOUT_FILENO, "\033[H\033[J", 6); // 清屏并移动光标
        for (int y = 0; y < height; y++) {
            write(STDOUT_FILENO, &framebuffer[y * width], width);
            write(STDOUT_FILENO, "\n", 1);
        }
#endif
    }

    // 设置像素
    void setPixel(int x, int y, Color c = ' ') {
        if (x >= 0 && x < width && y >= 0 && y < height)
            framebuffer[y * width + x] = c;
    }

    // 获取像素
    Color getPixel(int x, int y) {
        if (x >= 0 && x < width && y >= 0 && y < height)
            return framebuffer[y * width + x];
        return clear_color;
    }

    // 设置/获取清屏颜色
    void setClearColor(Color c) {
        clear_color = c;
    }

    Color getClearColor() const {
        return clear_color;
    }

    // 获取窗口尺寸
    int getWidth() const { return width; }
    int getHeight() const { return height; }
};

#endif
```

