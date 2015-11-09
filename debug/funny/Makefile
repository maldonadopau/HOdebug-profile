default: ddebug_off.e ddebug_on.e

ddebug_off: ddebug_off.e

ddebug_on: ddebug_on.e

ddebug_off.e: test_oob2.c
	gcc $^ -o $@ -O0 -g

ddebug_on.e: test_oob2.c
	gcc $^ -o $@ -DDEBUG -O0 -g
