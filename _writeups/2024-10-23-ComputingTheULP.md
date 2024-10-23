---
title: "Computing ULP programmatically"
layout: post
---


The code to compute $$ulp$$ of 1 is as follows. 

```
#include <fenv.h>
#include <iostream>
#include <bitset>

void printBits(double d) {

  uint64_t u;
  std::memcpy(&u, &d, sizeof(u));
  std::cout << std::bitset<64>(u) << "\n";
}

int main()
{

  // this is to compute ulp with subnormal computation disabled. If we want to get the ulp with subnormal enabled, we comment out this line.
  fesetenv(FE_DFL_DISABLE_DENORMS_ENV);
 
  double r = 1.0;
  double p = 1.0;
  while(r != 0.0) {
    p = r;
    r = r/2;
  }

  std::cout << p << "\n";
  printBits(p);

  return 0;
}
```


Output:

```
2.22507e-308
0000000000010000000000000000000000000000000000000000000000000000
```

Instead of starting with ```r=1.0``` we can start with any number we expect to be greater than $$ulp$$. Recommend to start with a number that is representable as double precision floating point as it becomes easy to see how the answer will be $$ulp$$.

In the method, we have disabled the subnormal computation. This is to compute the ulp disregarding subnormal numbers. We can skip disabling it as well.

Also, the macro
```
FE_DFL_DISABLE_DENORMS_ENV
```
seems to be defined for MacOS.

Intel/AMD architectures seem to have similar MACROS defined. Check for DAZ and FTZ [https://www.intel.com/content/www/us/en/docs/cpp-compiler/developer-guide-reference/2021-8/set-the-ftz-and-daz-flags.html](ocs/cpp-compiler/developer-guide-reference/2021-8/set-the-ftz-and-daz-flags.html)

