2. In the following program, what are argc and argv? The following program prints number of command-line arguments and each command-line argument on a separate line.
#include <stdio.h>
    ```C
    int main(int argc, char *argv[]) {
        printf("argc=%d\n", argc);
        for (int i = 0; i < argc; i++) {
            printf("argv[%d]=%s\n", i, argv[i]);
        }
        return 0;
    }
    ```

    What will be the output of the following commands?
    ```bash
    $ dcc -o print_arguments print_arguments.c
    $ ./print_arguments I love MIPS
    ```

    <details><summary>Ans</summary>

    ```bash
    $ dcc -o print_arguments print_arguments.c
    $ ./print_arguments I love MIPS
    argc=4
    argv[0]=print_arguments
    argv[1]=I
    argv[2]=love
    argv[3]=MIPS
    ```     

    </details>  
<br>
<br>

3. Why do we need the function atoi in the following program?

    The program assumes that command-line arguments are integers. What if they are not integer values?

    ```C
    #include <stdio.h>
    #include <stdlib.h>

    int main(int argc, char *argv[]) {
        int sum = 0;
        for (int i = 0; i < argc; i++) {
            sum += atoi(argv[i]);
        }
        printf("sum of command-line arguments = %d\n", sum);
        return 0;
    }
    ```

    <details><summary>Ans</summary>
    We are given command line arguments, e.g. argv[1] as a nul-terminated array of ASCII codes.
    We need to convert each argument to the corresponding integer.

    atoi does this conversion.

    ```bash
    $ dcc -o sum_arguments sum_arguments.c
    $ ./sum_arguments 21 6 7
    sum of command-line arguments = 34
    sum_arguments I love MIPS
    sum of command-line arguments = 0
    ```
    See strtol for a more powerful library function which would allow checking.
    </details>
<br>
<br>

4. Write a C program that takes integer command-line arguments and prints the largest number.

    If no extra arguments are provided (i.e. only argv[0]), print:

    ```No numbers provided.```  <br>

    Example runs:

    ```bash
    $ ./a.out 3 10 7 25 4
    Largest number = 25
    $ ./a.out
    No numbers provided.
    ```

    <details><summary>Ans</summary>

    ```C
    #include <stdio.h>
    #include <stdlib.h>

    int main(int argc, char *argv[]) {
        if (argc == 1) {
            printf("No numbers provided.\n");
            return 0;
        }

        int max = atoi(argv[1]);
        for (int i = 2; i < argc; i++) {
            int val = atoi(argv[i]);
            if (val > max) {
                max = val;
            }
        }

        printf("Largest number = %d\n", max);
        return 0;
    }
    ```
    </details>

<br>
<br>

5. Consider the following C program:

    ```C
    #include <stdio.h>

    void print_array(int nums[], int len) {
        for (int i = 0; i < len; i++) {
            printf("%d\n", nums[i]);
        }
    }

    int main(void)
    {
        int nums[] = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3};
        print_array(nums, 10);

        return 0;
    }
    ```

    This program uses a for loop to print out each element in the array

    Rewrite this program using a recursive function

    <details><summary>Ans</summary>

    ```C
    #include <stdio.h>

    void print_array(int nums[], int index, int len) {
        if (index == len) return;

        printf("%d\n", nums[index]);
        print_array(nums, index + 1, len);
    }

    int main(void)
    {
        int nums[] = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3};
        print_array(nums, 0, 10);
        
        return 0;
    }
    ```
    </details>

<br>
<br>

6. By default both stdout and stderr streams print to the screen. In linux we can choose to direct our different output streams to files when we run our executable program.

    ```
    >	redirects stdout to a file
    2>	redirects stderr to a file
    &>	redirects both stdout and stderr to a file
    ```
    Consider the following C program:

    ```C
    #include <stdio.h>
    #include <stdlib.h>

    int main(int argc, char * argv[]) {
        if(argc < 3){
            fprintf(stderr, "Usage: %s num1 num2\n",argv[0]);
            return 1;
        }
        int result = atoi(argv[1])  + atoi(argv[2]);
        printf("Result: %d\n",result);
        return 0;
    }
    ```

    What gets printed on the screen and what gets saved in the file named "someFile" if we run the program in the following way:

    ```bash
    a) ./add
    b) ./add 3 4
    c) ./add 1 > someFile
    d) ./add 1 2 > someFile
    e) ./add 99 2> someFile
    f) ./add 3 2 2> someFile
    g) ./add 1 &> someFile
    h) ./add 10 15 &> someFile
    ```

    <details><summary>Ans</summary>

    ```
    a) The following would be printed to stderr which by default is printed on the screen
        Usage: ./add num1 num2

    b) The following would be printed to stdout which by default is printed on the screen
        Result: 7

    c) The following would be printed to stderr which by default is printed on the screen
        Usage: ./add num1 num2

    d) In linux > redirects stdout to a given file. So the following would be printed to stdout which has been redirected to the file "someFile". So the contents of "someFile" would be
        Result: 3

    e) In linux 2> redirects stderr to a given file. So the following would be printed to stderr which has been redirected to the file "someFile". So the contents of "someFile" would be
        Usage: ./add num1 num2

    f) The following would be printed to stdout, which by default is printed to the screen
        Result: 5

    g) In linux &> redirects both stdin and stderr to a given file. So the following would be printed to stderr which has been redirected to the file "someFile". So the contents of "someFile" would be
        Usage: ./add num1 num2

    h) In linux &> redirects both stdin and stderr to a given file. So the following would be printed to stdout which has been redirected to the file "someFile". So the contents of "someFile" would be
        Result: 25
    ```
    </details>

<br>
<br>

7. Explain the differences between the properties of the variables s1 and s2 in the following program fragment:

    ```C
    #include <stdio.h>

    char *s1 = "abc";

    int main(void) {
        char *s2 = "def";
        // ...
    }
    ```
    Where is each variable located in memory? Where are the strings themselves located?

    <details><summary>Ans</summary>

    The s1 variable is a global variable and would be accessible from any function in this .c file. It would also be accessible from other .c files that referenced it as an extern'd variable.

    C implementations typically store global variables in the data segment (region of memory).

    The s2 variable is a local variable, and is only accessible within the main() function.

    C implementations typically store local variables on the stack, in a stack frame created for function — in this case, for main().

    C implementations typically place string literals such as "abc" in the text segment with the program's code.

    </details>

8. What is wrong with the following code?

    ```C
    #include <stdio.h>

    int *get_num_ptr(void);

    int main(void) {
        int *num = get_num_ptr();

        printf("%d\n", *num);
    }

    int *get_num_ptr(void) {
        int x = 42;
        return &x;
    }
    ```

    Assuming we still want get_num_ptr to return a pointer, how can we fix this code?

    How does fixing this code effect each variable's location in memory?

    <details><summary>Ans</summary>

    _What is wrong with the following code?_

    This is a classic stack use-after-return bug. int x is allocated on the stack, which means it will automatically be cleaned up when get_num_ptr returns. This invalidates the address returned by the function.

    _Assuming we still want get_num_ptr to return a pointer, how can we fix this code?_

    We need to make the value at that address live longer, so we should malloc that memory instead. Fundamentally, malloc allows us to allocate memory with a programmer-controlled lifetime, as opposed to stack memory which is controlled automatically by the compiler, or something like global memory which is always live.

    _How does fixing this code effect each variable's location in memory?_

    The num pointer variable's location in memory does not change -- it is still a stack-allocated address. However, x has been moved to the heap, where malloc'ed memory is stored.

    Example fixed code:
    ```C
    #include <stdio.h>
    #include <stdlib.h>

    int *get_num_ptr(void);

    int main(void) {
        int *num = get_num_ptr();

        printf("%d\n", *num);
        
        free(num);
    }

    int *get_num_ptr(void) {
        int *x = malloc(sizeof(int));
        *x = 42;

        return x;
    }
    ```

    </details>
    
<br>

9. Consider the following C program:
    ```C
    #include <stdio.h>

    int main(void) {
        char str[10];
        str[0] = 'H';
        str[1] = 'i';
        printf("%s", str);
        return 0;
    }
    ```

    What will happen when the above program is compiled and executed?

    In particular, what does this look like in memory?

    <details><summary>Ans</summary>

    The above program will compile without errors . printf, like many C library functions expects strings to be null-terminated.
    In other words printf, expects the array str to contain an element with value ```'\0'``` which marks the end of the sequence of characters to be printed.

    printf will print ```str[0] ('H')```, ```str[1]``` then examine ```str[2]```.

    Code produced by dcc will then stop with an error because ```str[2]``` is uninitialized.

    The code with gcc will keep executing and printing element from str until it encounters one containing '\0'. Often str[2] will by chance contain '\0' and the program will work correctly.

    Another common behaviour will be that the program prints some extra "random" characters.

    It is also possible the program will index outside the array which would result in it stopping with an error if it was compiled with dcc.

    If the program was compiled with gcc and uses indexes well outside the array it may be terminated by the operating system because of an illegal memory access.

    </details>
<br>
<br>

10. How could you correct the above program?

    <details><summary>Ans</summary>

    ```C
    #include <stdio.h>

    int main(void) {
        char str[10];
        str[0] = 'H';
        str[1] = 'i';
        str[2] = '\0';
        printf("%s", str);
        return 0;
    }
    ```
    </details>

<br>
<br>

11. What is a pointer? How do pointers relate to other variables?

    <details><summary>Ans</summary>

    Pointers are variables that refer (point) to another variable.
    Typically they implement this by storing a memory address of the variable they refer to.

    Given a pointer to a variable you can get its value and also assign to it.

    </details>

<br>
<br>

12. Consider the following small C program:
    ```C
    #include <stdio.h>

    int main(void) {
        int n[4] = { 42, 23, 11, 7 };
        int *p;

        p = &n[0];
        printf("%p\n", p); // prints 0x7fff00000000
        printf("%lu\n", sizeof (int)); // prints 4

        // what do these statements print ?
        n[0]++;
        printf("%d\n", *p);
        p++;
        printf("%p\n", p);
        printf("%d\n", *p);

        return 0;
    }
    ```

    Assume the variable n has address 0x7fff00000000.

    Assume sizeof (int) == 4.

    What does the program print?

    <details><summary>Ans</summary>

    Program output:

    ```bash
    0x7fff00000000
    4
    43
    0x7fff00000004
    23
    ```

    The n[0]++ changes the value by one, because n is an int variable.

    The p++ changes the value by four, because p is a pointer to an int, and addition of one to a pointer changes it to the point to the next element of the array.

    Each array element is four bytes, because sizeof (int) == 4

    </details>

<br>
<br>

13. C's sizeof operator is a prefix unary operator (precedes its 1 operand) - what are examples of other C unary operators?

    <details><summary>Ans</summary>

    |name	|operator	|C	|note
    |-------|-----------|---|----------
    unary minus	|-	|int i = -5;	|
    unary plus	|+	|int j = +5;	|
    Decrement	|--	|int l = --i;	|prefix or postfix
    Increment	|++	|int k = ++j;	|prefix or postfix
    Logical negation	|!	|if (true == ! false)	|
    Bitwise negation	|~	|int m = ~0;	|
    Address of	|&	|int *n = &m;	|
    Indirection	|*	|int o = *n;	|
    </details>

<br>
<br>

14. Why is C's sizeof operator different to other C unary & binary operators?

    <details><summary>Ans</summary>

    sizeof can be given a variable, value or a type as an argument.

    The syntax to distinguish this is weird, a type must be surrounded by brackets to be passed to sizeof.

    Note the value of sizeof can mostly be pre-calculated by the compiler when it compiles the program.

    </details>

<br>
<br>

15. Discuss errors in this code:
    ```C
    struct node *a = NULL;
    struct node *b = malloc(sizeof b);
    struct node *c = malloc(sizeof struct node);
    struct node *d = malloc(8);
    c = a;
    d.data = 42;
    c->data = 42;
    ```

    <details><summary>Ans</summary>

    sizeof b should be sizeof *b
    sizeof struct node should be sizeof (struct node)

    malloc(8) might be correct (depending on what struct ndoe is) but is definitely non-portable, struct node might be 8 bytes on a 32-bit OS and 12 or 16 bytes on a 64-bit OS

    d.data is incorrect as d is not a struct its a pointer to a struct

    c->data is illegal as c will be NULL when its executed

    </details>

<br>
<br>

16. For each of the following commands, describe what kind of output would be produced:
    ```bash
    clang -E x.c
    clang -S x.c
    clang -c x.c
    clang x.c
    ```
    In particular, how do these commands relate to what we will be studying in DPST1092?

    You can use the following simple C code as an example:

    ```C
    #include <stdio.h>
    #define N 10

    int main(void) {
        char str[N] = { 'H', 'i', '\0' };
        printf("%s\n", str);
        return 0;
    }
    ```
    <details><summary>Ans</summary>

    clang -E x.c
    Executes the C pre-processor, and writes modified C code to stdout containing the contents of all #include'd files and replacing all #define'd symbols.

    clang -S x.c
    Produces a file x.s containing the assembly code generated by the compiler for the C code in x.c. Clearly, architecture dependent.

    clang -c x.c
    Produces a file x.o containing relocatable machine code for the C code in x.c. Also architecture dependent. This is not a complete program, even if it has a main() function: it needs to be combined with the code for the library functions (by the linker ld).

    clang x.c
    Produces an executable file called a.out, containing all of the machine code needed to run the code from x.c on the target machine architecture. The name a.out can be overridden by specifying a flag -o filename.

    </details>

<br>
<br>

17. Rewrite the following code without using break or continue:
    ```C
    #include <stdio.h>
    #define N 10

    int main(void){
    int a[] = {4, 3, 9, -8, 5, -4, 3, 1, 0, 4};
    int sum = 0;
    int i;
    for (i = 0; i < N; i++) {
        if (a[i] == 0) break;
        if (a[i] < 0) continue;
        sum += a[i];
    }
    printf("The sum is %d\n",sum);
        return 0;
    }
    ```

    <details><summary>Ans</summary>

    ```C
    #include <stdio.h>
    #define N 10

    int main(void){
    int a[] = {4, 3, 9, -8, 5, -4, 3, 1, 0, 4};
    int sum = 0;
    int i;
    for (i = 0; i < N && a[i] != 0; i++) {
        if (a[i] > 0) {
            sum += a[i];
        }
    }

    printf("The sum is %d\n",sum);
        return 0;
    }
    ```
    </details>