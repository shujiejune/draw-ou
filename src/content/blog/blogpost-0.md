---
pubDatetime: 2025-01-02T11:11:47Z
title: Blog Post 0
slug: blogpost-0
featured: false
draft: false
tags:
  - test
  - rendering
  - markdown
description:
  Blog post 0 to test markdown features. 
---

# Blog Post 0

Hello Blog! This is my 0th blogpost to test features of markdown in Astro.

## Table of contents

## plain text

1. **English**: My personal blog.

2. 中文：我的个人博客。

3. **Making Blog Posts**: This is my first blog post! I now have Astro pages and Markdown posts!

## code snippet

Test how the code snippet looks like in my blog:

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        int l = 0, r = n - 1, mid = 0;
        while (nums[r] < nums[l]) {
            mid = l + (r - l) / 2;
            if (nums[mid] < nums[r]) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return nums[l];
    }
};
```

## LaTeX equation

This is an inline equation: $k_{n+1} = n^2 + k_n^2 - k_{n-1}$

This is a math block:
$$
\frac{n!}{k!(n-k)!} = \binom{n}{k} \\
\frac{\frac{1}{x}+\frac{1}{y}}{y-z} \\
\displaystyle\sum_{i=1}^{10} t_i \\
\int_0^\infty \mathrm{e}^{-x}\,\mathrm{d}x \\
P\left(A=2\middle|\frac{A^2}{B}>4\right) \\
\frac{\mathrm d}{\mathrm d x} \big( k g(x) \big) \\
\lim\limits_{x \to \infty} \exp(-x) = 0
$$

## Image

This is an image:

![taichi](@assets/images/taichi.jpeg)

## Table

| expression | meaning                                             | usage example                                                |
| ---------- | --------------------------------------------------- | ------------------------------------------------------------ |
| bring up   | to raise for discussion                             | You bring up a good point for others.                        |
| savvy      | having practical knowledge and understanding of sth | John is tech-savvy, he knows everything about the latest technology trends. |
| gentrify   | to develop                                          | Wealthy residents have gentrified the area in the last decade. |

The above is a sample table.

## Other

- [x] ~~This is a checked checkbox.~~
- [ ] This is an unchecked checkbox.

- Strong

- emphasis

- underline

- ==highlight==

- <!--comment-->

- [astro website](https://astro.build/)

> This is a quote.

> [!NOTE]
>
> This is a note.

> [!TIP]
>
> This is a tip.

> [!IMPORTANT]
>
> This is an important item.

> [!WARNING]
>
> This is a warning.

> [!CAUTION]
>
> This is a caution notice.

