+++
title = "Makefile"
description = "The way to compiling them easily"
date = 2022-03-03T07:00:00+09:00
updated = 2022-03-03T00:00:00+09:00
template = "blog/page.html"
draft = false

[taxonomies]
authors = ["Seongsu Park"]

[extra]
lead = "The way to compiling them easily"
images = []
tags = ["makefile", "c", "42Seoul"]
toc = true
+++
[Makefile 직접 만들어보기](https://www.notion.so/Makefile-5515ac58527c481cb67f00d30a19a7f9)

```Makefile
NAME := main

SRC_DIR = ./src/
SRC_NAME = main.c \
			fourth.c
SRC = $(addprefix $(SRC_DIR),$(SRC_NAME))

OBJ_DIR = ./obj/
OBJ_NAME = $(SRC_NAME:.c=.o)
OBJ = $(addprefix $(OBJ_DIR),$(OBJ_NAME))

CC := gcc
CFLAGS := -Wall -Wextra -Werror
INCLUDE := -I./include -I./lib

LIB_DIR = ./lib/
LIB_NAME = ft
LIB = $(LIB_DIR)lib$(LIB_NAME).a

all : $(NAME)

clean :
	rm -f $(OBJ)
	make -C $(LIB_DIR) clean
fclean : clean
	rm -f $(NAME)
re : fclean all

run : all
	./$(NAME)

$(NAME) : $(LIB) $(OBJ)
	$(CC) $(CFLAGS) $(INCLUDE) -L$(LIB_DIR) -l$(LIB_NAME) -o $@ $^

$(LIB) :
	make -C $(LIB_DIR) all

$(OBJ_DIR)%.o : $(SRC_DIR)%.c
	@mkdir -p $(OBJ_DIR)
	$(CC) $(CFLAGS) $(INCLUDE) -c $< -o $@

.PHONY : all clean fclean re run
```
