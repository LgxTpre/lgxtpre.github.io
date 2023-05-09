# Home

欢迎来到 $\mathcal{LgxTpre}$ 的代码仓库呀！~

为了保证您的使用体验，下面将会对一些内容进行测试，请查看其是否正确 qwq。

## 下面是数学公式

- 行内公式：$f(x)=\sum_{n=0}^m a_nx^n$
- 行间公式：  
$$
h(x) = \sum_{d\mid x} f(d)g(\frac xd) = \sum_{ab=x}f(a)g(b)
$$

## 下面是表格

| Language | Ratings |
| :------: | :-----: |
|  Python  |  14.83% |
|    C     |  14.73% |
|   Java   |  13.56% |
|   C++    |  13.29% |
|   C#     |   7.57% |

## 下面是代码段

```cpp
// 没啥活好整了，给大家咬个打火机吧
#include <bits/stdc++.h>
using namespace std;
const double alpha = 0.2928;
typedef struct NODE{
    int value;
    int size;
    NODE *lchild,*rchild;
    NODE(int x){
        this->value = x;
        this->size = 1;
        this->lchild = this->rchild = nullptr;
    }
    NODE(){
        this->value = INT_MAX;
        this->size = 0;
        this->lchild = this->rchild = nullptr;
    }
}NODE,*PNODE;

PNODE root;

inline PNODE newNode(int x){
    return new NODE(x);
}

inline void deleteNode(PNODE &T){
    delete T;
    T = nullptr;
}

inline bool isLeaf(PNODE T){
    return T->lchild == nullptr;
}

inline void pushup(PNODE T){
    if(!isLeaf(T)){
        T->value = T->rchild->value;
        T->size = T->lchild->size + T->rchild->size;
    }
}

inline void rotate(PNODE T,bool d){
    PNODE temp;
    if(!d){
        temp = T->rchild;
        T->rchild = T->lchild;
        T->lchild = T->rchild->lchild;
        T->rchild->lchild = T->rchild->rchild;
        T->rchild->rchild = temp;
    }
    else{
        temp = T->lchild;
        T->lchild = T->rchild;
        T->rchild = T->lchild->rchild;
        T->lchild->rchild = T->lchild->lchild;
        T->lchild->lchild = temp;
    }
    pushup(T->lchild);
    pushup(T->rchild);
    pushup(T);
}

inline void maintain(PNODE T){
    bool d = 0;
    if(!isLeaf(T)){
        if(T->lchild->size < T->size * alpha)
            d = 1;
        else if(T->rchild->size < T->size * alpha)
            d = 0;
        else
            return ;
        if(d){
            if(T->rchild->lchild->size >= T->rchild->size * (1-2*alpha)/(1-alpha))
                rotate(T->rchild,!d);
        }
        else{
            if(T->lchild->rchild->size >= T->lchild->size * (1-2*alpha)/(1-alpha))
                rotate(T->lchild,!d);
        }
        rotate(T,d);
    }
}

inline void Insert(PNODE T, int x){
    if(isLeaf(T)){
        T->lchild = newNode(x);
        T->rchild = newNode(T->value);
        if(T->lchild->value>T->rchild->value)
            swap(T->lchild,T->rchild);
        pushup(T);
        return;
    }
    if(T->lchild->value >= x)
        Insert(T->lchild,x);
    else
        Insert(T->rchild,x);
    pushup(T);
    maintain(T);
}

inline void Delete(PNODE &T,int x){
    if(isLeaf(T))
        return ;
    if(x<=T->lchild->value){
        if(isLeaf(T->lchild)){
            if(x!=T->lchild->value)
                return;
            deleteNode(T->lchild);
            PNODE buf = T->rchild;
            *T = *buf;
            deleteNode(buf);
        }
        else
            Delete(T->lchild,x);
    }
    else{
        if(isLeaf(T->rchild)){
            if(x!=T->rchild->value)
                return ;
            deleteNode(T->rchild);
            PNODE buf = T->lchild;
            *T = *buf;
            deleteNode(buf);
        }
        else
            Delete(T->rchild,x);
    }
    pushup(T);
    maintain(T);
}

inline int GetRnk(PNODE T,int x){
    if(isLeaf(T))
        return 1;
    if(x <= T->lchild->value)
        return GetRnk(T->lchild,x);
    return T->lchild->size + GetRnk(T->rchild,x);
}

inline int GetNum(PNODE T,int rnk){
    if(isLeaf(T))
        return T->value;
    if(rnk <= T->lchild->size)
        return GetNum(T->lchild,rnk);
    return GetNum(T->rchild,rnk - T->lchild->size);
}

inline int GetPre(PNODE T,int num){
    return GetNum(T,GetRnk(T,num)-1);
}

inline int GetSuf(PNODE T,int num){
    return GetNum(T,GetRnk(T,num+1));
}

int main(){
    root = new NODE;
    int n;
    cin>>n;
    while(n--){
        int opt;
        cin>>opt;
        switch(opt){
            case 1:{
                int x;
                cin>>x;
                Insert(root,x);
                break;
            }
            case 2:{
                int x;
                cin>>x;
                Delete(root,x);
                break;
            }
            case 3:{
                int x;
                cin>>x;
                cout<<GetRnk(root,x)<<endl;
                break;
            }
            case 4:{
                int x;
                cin>>x;
                cout<<GetNum(root,x)<<endl;
                break;
            }
            case 5:{
                int x;
                cin>>x;
                cout<<GetPre(root,x)<<endl;
                break;
            }
            case 6:{
                int x;
                cin>>x;
                cout<<GetSuf(root,x)<<endl;
                break;
            }
        }
    }
    return 0;
}
```

??? 这是收起来的样子
    === "C++"

        ```cpp
        #define _CRT_SECURE_NO_WARNINGS
        #include <opencv.hpp>
        #include <windows.h>
        #include <conio.h>
        using namespace std;
        using namespace cv;
        #define MAX_COL 1024
        #define MAX_ROW 2048
        //#define MAX_SIZE 6746
        char shadowvector[] = "@$#%&WASGHKBMRDFZXNVCJLQOTPYEUIab987654321~?!^*()<>+-=[]{},.    ";
        long long len;
        char framemat[MAX_ROW][MAX_COL];
        char gray2char(int level) {
            level = 255 - level;
            return shadowvector[(int)level / 4];
        }
        int main() {
            len = strlen(shadowvector)-1;
            HANDLE handle = GetStdHandle(((DWORD)-11));
            CONSOLE_FONT_INFOEX cfi;
            cfi.cbSize = sizeof(CONSOLE_FONT_INFOEX);
            COORD size;
            size.X = 3;
            size.Y = 5;
            cfi.dwFontSize = size;
            wcscpy(cfi.FaceName, L"Arial");
            cfi.FontWeight = 100;
            cfi.FontFamily = TMPF_TRUETYPE;
            cfi.nFont = 0;
            SetCurrentConsoleFontEx(handle, TRUE, &cfi);
            int T = 6574+1;
            _getch();
            double c1 = clock();
            while (--T) {
                int t1 = clock();
                char strbuf[1024];
                sprintf(strbuf, "./frames/%d.png", 6574 - T + 1);
                Mat srcImage = imread(strbuf);
                Mat temImage, dstImage1;
                temImage = srcImage;
                resize(temImage, dstImage1, Size(0, 0), (double)1/4+1, (double)1/8+0.5, INTER_LINEAR);
                cvtColor(dstImage1, dstImage1, COLOR_RGB2GRAY);
                int cnt = 0;
                for (int i = 0,j; i < dstImage1.rows; i++) {
                    for (j = 0; j < dstImage1.cols; j++)
                        framemat[i][j] = gray2char(dstImage1.ptr<uchar>(i)[j]);
                    framemat[i][j] = '\0';
                }
                for (int i = 0; i < dstImage1.rows; i++) {
                    COORD pos;
                    pos.X = 0;
                    pos.Y = i;
                    DWORD real;
                    WriteConsoleOutputCharacter(handle, framemat[i], strlen(framemat[i])*sizeof(char), pos, &real);
                }
                int t2 = clock();
                int sl = 33 - t2 + t1;
                Sleep(sl<0?0:sl);
            }
            double c2 = clock();
            printf("%.3lf", (c2 - c1) / CLOCKS_PER_SEC);
            system("pause");
            return 0;
        }
        ```
    
    === "Python"
    
        ```python
        import cv2 
        ascii_char = list("$@B%8&WM#*oahkbdpqwmZO0QLCJUYXzcvunxrjft/\|()1{}[]?-_+~<>i!lI;:,\"^`'. ") 
        def get_char(gray_number): 
            length = len(ascii_char) 
            unit = (256.0 + 1)/length 
            return ascii_char[int(gray_number/unit)] 
        def convert(source, dest):
            image1 = cv2.imread(source) 
            image = cv2.resize(image1,(232,87)) 
            gray = cv2.cvtColor(image,cv2.COLOR_BGR2GRAY) 
            txt = "" 
            for i in range(image.shape[0]): 
                for j in range(image.shape[1]): 
                    txt += get_char(gray[i,j]) 
                txt += '\n' 
            f = open(dest,'w') 
            f.write(txt)
    
        if __name__ == '__main__':
            for i in range(1,6574):
                convert("%d.png"%i,'.\\txt\\%d.txt'%i);
                print("Finished Converting %d/6574"%i,end='\r');
        ```

~~顺便测试了较大文件传输，还有删除线~~

## 下面是图片

树链剖分捏：

![image](/image/tester.png)

AC 自动机捏：

![GIF](/image/ac-automaton2.gif)

**顺带一提，上述图片均来自于 [OI-Wiki](https://oi-wiki.org)**

