# offset & rotate

Tags: Array, String

offset

```cpp
offset = (offset%str.size() + str.size())%str.size();
```

//rotate

```cpp
 reverse(str.begin(),str.end());
 reverse(str.begin(),str.begin()+offset);
 reverse(str.begin()+offset,str.end());
```

//recover

```cpp
 reverse(str.begin(),str.begin()+offset);
 reverse(str.begin()+offset,str.end());
 reverse(str.begin(),str.end());
```