# C语言结构

## `pl=(struct point){5,10}`

结构变量的名字不是地址必须取地址加&

## 结构可以作为函数参数

### `int numberofdayss(struct date d)`

### 输入结构：结构的值可以传过去，但传的只是克隆体

### 解决：用这样的结构

```int main(int argc, char const *argv[])
#include<stdio.h>

struct point {
	int x;
	int y;
};
struct point getstruct(void);

int main(int argc,char const *argv[])
{
	struct point y={0,0};
	y=getstruct();
	output(y);
}

struct point getstruct(void)
{
	struct point p;
	scanf("%d",&p.x);
	scanf("%d",&p.y);
	printf("%d, %d",p.x,p.y);
	return p;
}

void output(struct point p)
{
	printf("%d, %d",p.x,p.y);
}
```

所以可能更好的方式是传指针

```pointer to the struct
struct date{
	int month;
	int day;
	int year;
}myday;

struct date*p=&myday;
(*p).month=12;
p->month=12;//这两个等价表示指针所指的结构的结构变量的成员 
```



.的左边必须是一个结构

```sizhongdengjia
```







