CC = gcc
FLAGS = -Wall -Werror -Wextra -std=c11
GCOV = --coverage
UNIT = unit_tests
STLIB = s21_viewer.a
OS = $(shell uname -s)

ifeq ($(OS), Darwin)
    LIBS := -lcheck 
	APPLICATION = 3dviewer10.app 
	OPEN = open
else
    LIBS := -lcheck_pic -lrt -lpthread -lsubunit -lm -g
	APPLICATION = 3dviewer10
	OPEN = ./
endif

all: clean s21_viewer.a test

s21_viewer.a: build_s21_viewer

build_s21_viewer:
	$(CC) -c source_code/s21_viewer.c source_code/aff_transformation.c
	ar rcs $(STLIB) s21_*.o aff*.o
	ranlib $(STLIB)

unit_tests.o:
	$(CC) $(FLAGS) -c source_code/3DViewer_test.c

test: $(STLIB)
	$(CC) $(FLAGS) source_code/3DViewer_test.c $(STLIB) -o $(UNIT) $(LIBS)
	./$(UNIT)

gcov_report: $(STLIB) unit_tests.o
	$(CC) $(FLAGS) $(GCOV) 3DViewer_test.o source_code/s21_viewer.c source_code/aff_transformation.c -o $(UNIT) $(LIBS)
	./$(UNIT)
	lcov -t "test" -o test.info -c -d .
	genhtml -o report test.info
	open report/index.html

clang:
	cp ../materials/linters/.clang-format ./source_code
	clang-format -n source_code/*.c source_code/*.cpp
	rm source_code/.clang-format

install:
	make clean
	mkdir 3dviewer
	cmake source_code -B 3dviewer
	make -C 3dviewer/
open:
	cd 3dviewer && $(OPEN)$(APPLICATION)

uninstall:
	rm -rf 3dviewer

dvi:
	open dvi.html

dist:
	tar -cvzf Archive_3dviewer1.0.tar Makefile dvi.html source_code

coverage:
	open report/index.html

clean:
	rm -rf *.o *.a *.conf *.gc* *.info test report *dSYM $(UNIT) *tar 3dviewer source_code/*.user
