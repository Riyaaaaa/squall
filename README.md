# squall

A squirrel C++11 binding

## Make VM but do nothing

```
#include "squall/vm.hpp"

int main() {
    squall::VM vm; // may throw squall::squirrel_error
    return 0;
}
```

## Make VM with standard library but do nothing

```
#include "squall/vmstd.hpp"

int main() {
    squall::VMStd vm; // may throw squall::squirrel_error
    return 0;
}
```

## Call squirrel function from C++

```test.nut
function foo() {
  print("==== foo0 called\n");
  return 0;
}
```

```
int main() {
    try {
        squall::VM vm;
        vm.dofile("test.nut");

        int n1 = vm.call<int>("foo", 7);
        std::cout << "**** return value: " << n1 << std::endl;
    }
    catch(squall::squirrel_error& e) {
        std::cerr << e.what() << std::endl;
    }

    return 0;
}
```

```test.nut
function baz() {
  print("==== baz called\n");
  return bar(4649);
}
```

## Call C++ from squirrel

```
int main() {
    try {
        squall::VM vm;
        vm.dofile("test.nut");

        vm.defun("bar", [=](int x)->int {
                std::cout << "**** lambda: " << x << std::endl;
                return 7777;
            });

        int n2 = vm.call<int>("baz");
        std::cout << "**** return value: " << n2 << std::endl;
    }
    catch(squall::squirrel_error& e) {
        std::cerr << e.what() << std::endl;
    }

    return 0;
}
```

## Call squirrel method from C++

In development