## json-c
### Compile library
```console
# wget https://s3.amazonaws.com/json-c_releases/releases/json-c-0.12.1.tar.gz
# tar -xf json-c-0.12.1.tar.gz
# ln -sf json-c-0.12.1 json-c
# cd json-c
# ./configure
# make
# cd ..
```
### Makefile
```
CFLAGS = -Ijson-c
LDFLAGS = -Ljson-c/.libs
LIBS = -ljson-c

all:
	$(CC) test.c -o test $(CFLAGS) $(LDFLAGS) $(LIBS)
```
### C program
可以先看下面執行的結果，再回來看程式的細節，先以python的觀點來看這些function

|C|Python|
|---|---|
|json_object *movie = json_object_new_object();|movie = {}|
|json_object *stars = json_object_new_array();|stars = []|
|json_object_object_add(movie, "writer", json_object_new_string("Stephen King"));|movie.update({"writer": "Stephen King"})|
|json_object_array_add(stars, TR);|stars.append(TR)|


```c
#include <stdio.h>
#include <json.h>

int main()
{
    json_object *movie = json_object_new_object();
    json_object *stars = json_object_new_array();
    json_object *TR = json_object_new_object();
    json_object *MF = json_object_new_object();
    json_object_object_add(movie, "title", json_object_new_string("The Shawshank Redemption"));
    json_object_object_add(movie, "writer", json_object_new_string("Stephen King"));
    json_object_object_add(movie, "released", json_object_new_int(1994));
    json_object_object_add(TR, "name", json_object_new_string("Tim Robbins"));
    json_object_object_add(MF, "name", json_object_new_string("Morgan Freeman"));
    json_object_array_add(stars, TR);
    json_object_array_add(stars, MF);
    json_object_object_add(movie, "stars", stars);

    printf("%s\n", json_object_to_json_string(movie));
}
```
用python相對應的程式
```python
#!/usr/bin/env python
import json

movie = {}
stars = []
TR = {}
MF = {}
movie.update({"title": "The Shawshank Redemption"})
movie.update({"writer": "Stephen King"})
movie.update({"released": 1994})
TR.update({"name": "Tim Robbins"})
MF.update({"name": "Morgan Freeman"})
stars.append(TR)
stars.append(MF)
movie.update({"stars": stars})

print json.dumps(movie)
```
經過排版後的執行結果
```console
# make
cc test.c -o test -Ijson-c -Ljson-c/.libs -ljson-c
# LD_LIBRARY_PATH=json-c/.libs ./test
{
  "title": "The Shawshank Redemption",
  "writer": "Stephen King",
  "released": 1994,
  "stars": [
    {
      "name": "Tim Robbins"
    },
    {
      "name": "Morgan Freeman"
    }
  ]
}
```
