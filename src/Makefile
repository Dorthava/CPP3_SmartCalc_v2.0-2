CC = g++
CPPFLAGS = -std=c++17 -lgtest -lgtest_main -fprofile-arcs -ftest-coverage --coverage
CHECKFLAGS = -Wall -Werror -Wextra

ifeq ($(shell uname), Linux)
	CPPFLAGS+=-lstdc++ -pthread -lgcov
else
	CPPFLAGS+=-lpthread 
endif

all: clean test

test: clean
	@$(CC) $(CHECKFLAGS) test.cc -o $@ $(CPPFLAGS) && ./test

gcov_report: test
	@lcov -t "gcov_report" -o report.info --no-external -c -d .
	@genhtml -o report report.info
	@open ./report/index.html

build:
	@mkdir build	
	@cd build && cmake ../ && cmake --build . --target all

install: uninstall build
	
uninstall:
	@rm -rf build

dvi:
	open ../README.md

dist:
	@mkdir ./Archive_SmartCalc_v2.0
	@mkdir ./Archive_SmartCalc_v2.0/src 
	@cp -r credit deposit graph smartcalc ui ./Archive_SmartCalc_v2.0/src
	@cp Makefile CMakeLists.txt *.cpp *.cc *.h ./Archive_SmartCalc_v2.0/src
	@tar cvzf Archive_SmartCalc_v2.0.tgz Archive_SmartCalc_v2.0/
	@mv Archive_SmartCalc_v2.0.tgz $(HOME)/Desktop/
	@rm -rf ./Archive_SmartCalc_v2.0

clean: uninstall
	@rm -rf *.gcda *.gcno *.info test report CMakeLists.txt.user

clang:
	@cp ../materials/linters/.clang-format .
	@echo "===> Formatting your code for Google Style <==="
	@clang-format -i -style=GOOGLE *.cc *.cpp *.h credit/*.cc credit/*.cpp credit/*.h deposit/*.cc deposit/*.h deposit/*.cpp graph/*.cc graph/*.cpp graph/*.h smartcalc/*.cc smartcalc/*.h ui/*.ui
	@rm -rf .clang-format

valgrind: test
	@CK_FORK=no valgrind --vgdb=no --leak-check=full --show-leak-kinds=all --track-origins=yes --verbose ./test .qt

