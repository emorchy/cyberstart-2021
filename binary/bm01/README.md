# bm01
 **250 points**

There is a strcmp function that compares the input to a password. Don't forget to translate the flag from Russian (translation in parentheses)!

```$ ltrace ./program
puts("\033[36m\320\232\320\260\320\272\320\276\320\271 \320\277\320\260\321\200\320\276\320\273\321\214\357\274\237\033"...Какой пароль？
) = 36
printf("> ")                                = 2
fgets(> 123
"123\n", 60, 0x7f82f2be7980)          = 0x7ffcb9443d50
strcmp("\320\274\320\276\320\273\320\276\321\202\320\276\320\272123\n", "123\n") = 159
puts("\033[31m\320\275\320\265\320\262\320\265\321\200\320\275\321\213\320\271.\033[0m"неверный.
) = 27
+++ exited (status 0) +++
```
```
python -c 'print "\320\274\320\276\320\273\320\276\321\202\320\276\320\272123"'
молоток123
```
Какой пароль？(What password?)

\> молоток123 (hammer123)

верный! (right!)

флаг: wh1te%BluE$R3d (flag: wh1te%BluE$R3d)
