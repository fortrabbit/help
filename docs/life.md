---

template:      article
reviewed:      2024-02-15 08:21:03
title:         Meaning of life
lead:          The universe, and everything.
naviTitle:     Life
noToc:         1
group:         tips
stack:         all
hideExamples:  yes

keywords:
    - 42
    - Hitchhiker's Guide to the Galaxy
    - Hitchhiker
    - Galaxy
    - Guide

---

```php
echo (array_sum(array_map(function ($num) use (&$count) {
    return $num / $count;
}, array_filter(range(1, 99), function ($num) use (&$count) {
    return !($num % 3) && $count ++ > 10;
}))) >> 3) + $count / 528 * -- $count;
```

Of course: only an approximation.