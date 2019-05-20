# Tests




## Is year a leap year?

Leap year from 463.

```q
q)ly:{sum[0=x mod/:4 100 400]mod 2}
```


## Is matrix x antisymmetric?

```q
q)show x:(0 -7 1; 7 0 -4; -1 4 0)
0  -7 1
7  0  -4
-1 4  0
q)x~neg flip x
1b
```


## Is matrix x symmetric?

```q
q)show x:(0 4 7 1; 4 8 6 4; 7 6 2 0; 1 4 0 6)
0 4 7 1
4 8 6 4
7 6 2 0
1 4 0 6
q)x~flip x
1b

q)show x:4 4#16?10
6 6 3 3
9 7 9 4
4 7 9 9
4 7 8 9
q)x~flip x
0b
```


## Is range of x 1?

Are all items of x the same?

```q
q)x:1 1 1 1 1
q)1=count distinct x
1b
q)y:1 1 0 1 1
q)1=count distinct y
0b
```


## Is x in range [y)

```q
q)x:19 20 21 39 40 41
q)y:20 40
q)x<\:y
11b
01b
01b
01b
00b
00b
q)01b~/:x<\:y
011100b
q)((<)over)each x<\:y
011100b
q)(</')x<\:y            / elide keywords when composing iterators
011100b
```



## Is x upper triangular?


See 195, 196.

```q
q)zm:{(2#count x)#0}  / zero matrix (square matrix)
q)show x:(1 0 0 1;0 2 1 0;0 0 1 2;0 0 0 0)
1 0 0 1
0 2 1 0
0 0 1 2
0 0 0 0
q)tc:('[til;count])
q){x>\:x}tc x
0000b
1000b
1100b
1110b
q)x*{x>\:x}tc x
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0
q)zm x
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0
q)zm[x]~x*{x>\:x}tc x
1b
q){zm[x]~x*{x>\:x}tc x}flip x
0b
```


## Is x lower triangular?

See 195, 196.

```q
)show x:(1 0 0 0;0 2 0 0;0 1 1 0;1 0 2 0)
1 0 0 0
0 2 0 0
0 1 1 0
1 0 2 0
q){x<\:x}tc x
0111b
0011b
0001b
0000b
q)zm[x]~x*{x<\:x}tc x
1b
q){zm[x]~x*{x<\:x}tc x}flip x
0b
```


## Is x an integer in interval [g,h)?

```q
q)g:6;h:12
q)x:3 5 6 7.5 11 12 13
q)x<\:g,h
11b
11b
01b
01b
01b
00b
00b
q)(</)each x<\:g,h
0011100b
q)x where(</)each x<\:g,h
6 7.5 11
q)(x=floor x)&(</)each x<\:g,h
1110111b
```


## Is x within range [ y )?

```q
q)x:9
q)show y:(1 9;9 16;5 7;10 20;6 10)
1  9
9  16
5  7
10 20
6  10
q)x<y
00b
01b
00b
11b
01b
q)(</)each x<y
01001b
```


## Is x within range ( y ]?

```q
q)show y:(1 9;9 16;5 7;10 20;6 10)
1  9
9  16
5  7
10 20
6  10
q)x<=y
01b
11b
00b
11b
01b
q)(</)each x<=y
10001b
```


## Is y a row of x?

```q
q)show x:("aaa";"bbb";"ooo";"ppp";"kkk")
q)y:"ooo"
q)y in x
1b
```

==DROP: keyword not idiom==


## Is x in ascending order?

```q
q)all(>=)prior 0 1 1 1 7 8 9
1b
q)all(>=)prior 2 5 7 9 6 8 3
0b
```

Or. 

```q
q)x~asc x:2 5 7 9 6 8 3
0b
q)x~asc x:0 1 1 1 7 8 9
1b
```


## Quick membership for non-negative integers

```q
q)x:5 3 7 2
q)y:8 5 2 6 1 9
q)max x,y
9
q)(1+max x,y)#0
0 0 0 0 0 0 0 0 0 0
q)a:(1+max x,y)#0
q)a
0 0 0 0 0 0 0 0 0 0
q)@[a;y;:;1]
0 1 1 0 0 1 1 0 1 1
q)(@[a;y;:;1])[x]
1 0 0 1
q)@[(1+max x,y)#0;y;:;1][x]
1 0 0 1
```

Instructive to study, but not actually faster than `x in y` (V3.6).



## Do ranges of x and y match?

```q
q)x:1 2 3
q)y:3 1 2 1
q)(~)over('[asc;distinct])each(x;y)
1b
q)y:3 1 2 1 4
q)(~)over('[asc;distinct])each(x;y)
0b
```


## Do x and y have items in common?

```q
q)x:"abc"
q)y:"cdeac"
q)any x in y
1b
```


## Is x 1s and 0s only?

```q
q)ib:{t:abs type x; $[t~1;1b;t in 5 6 7 9h;all x in t$0 1;0b]}
q)ib each (101b;1 0 1h;1 0 1i;1 0 1j;1 0 1 2;1 0 1f)
011101b
q)ib each (101b;1 0 1h;1 0 1i;1 0 1j;1 0 1 2;1 0 1f;1 1.2 0)
0111010b
```


## Is x a subset of y?

```q
q)x:"abgk"
q)y:"abcdefghijkl"
q)all x in y
1b
```


## Are items unique?

```q
q)x:"abcdefg"
q)x~distinct x
1b
q)x:"abcdefa"
q)x~distinct x
0b
```


## Does x match y?

```q
q)x:("abc";`sy;1 3 -7)
q)y:("abc";`sy;1 3 -7)
q)x~y
1b
q)x:1 2 3
q)y:1 4 3
q)x~y
0b
```

