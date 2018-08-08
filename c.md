Array initialization. If you initialize an array with one element only, other elements are set to `0`.

```c
int a[5] = { 1 };
printf("{ %d %d %d %d %d }\n", a[0], a[1], a[2], a[3], a[4]);
```
Output:
```txt
{ 1 0 0 0 0 }
```