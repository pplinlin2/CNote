# Kernel list
## Get Source
下載list.h，在本資料夾下也複製了一份
```console
# wget http://www.mcs.anl.gov/~kazutomo/list/list.h
```
## Example
在我們自定義的struct裡面加入`struct list_head`，它的結構定義如下
```c
struct list_head {
    struct list_head *next, *prev;
};
```
* 用`INIT_LIST_HEAD(struct list_head *)`初始化list
* 用`list_add_tail(struct list_head *new, struct list_head *head)`將new object加入list尾端
* 用`list_for_each_entry_safe(pos, n, head, member)`跑一遍list

|arguments|meanings|
|---|---|
|pos|struct movie *curr|
|n|struct movie *next|
|head|struct list_head *head|
|member|struct movie中struct list_head的變量名|

```c
#include <stdio.h>
#include <stdlib.h>
#include "list.h"

struct movie {
    char *title;
    int released;
    struct list_head list;
};

void append(struct movie *head, char *title, int released)
{
    struct movie *new = (struct movie *)malloc(sizeof(struct movie));
    new->title = title;
    new->released = released;
    list_add_tail(&(new->list), &(head->list));
}

int main()
{
    struct movie imdb100;
    struct movie *curr, *next;

    INIT_LIST_HEAD(&(imdb100.list));

    append(&imdb100, "The Shawshank Redemption", 1994);
    append(&imdb100, "The Godfather", 1972);
    append(&imdb100, "The Godfather: Part II", 1974);

    list_for_each_entry_safe(curr, next, &(imdb100.list), list){
        printf("%s(%d)\n", curr->title, curr->released);
    }

    list_for_each_entry_safe(curr, next, &(imdb100.list), list){
        free(curr);
    }
}
```
執行結果
```console
# gcc test.c -o test
# ./test
The Shawshank Redemption(1994)
The Godfather(1972)
The Godfather: Part II(1974)
#
```
