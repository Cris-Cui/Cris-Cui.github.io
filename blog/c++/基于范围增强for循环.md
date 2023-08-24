# 基于范围增强for循环

```cpp
for (decl : coll) {	// Range-Base for loop
    statement...;
}

// 即:
for (auto _pos = coll.begin, _end = coll.end(); _pos != _end; ++ _pos) {
	decl = *_pos;
    statement...;
}
// 或:
for (auto _pos = begin(coll), _end = end(coll); _pos != _end; ++ _pos) {	// begin() and end() are global
	decl = *_pos;
    statement...;
}
```

