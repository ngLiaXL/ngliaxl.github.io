---
layout: post
title:  "List多属性排序"
date:   2018-07-15 11:53:27 +0800
categories: android
---




```
public class MultiComparator<T> implements Comparator<T> {

    private final List<Comparator<T>> comparators;

    public MultiComparator(List<Comparator<T>> comparators) {
        this.comparators = comparators;
    }

    @SafeVarargs
    public MultiComparator(Comparator<T>... comparators) {
        this(Arrays.asList(comparators));
    }

    @Override
    public int compare(T o1, T o2) {
        for (Comparator<T> c : comparators) {
            int result = c.compare(o1, o2);
            if (result != 0) {
                return result;
            }
        }
        return 0;
    }

    @SafeVarargs
    public static <T> void sort(List<T> list, Comparator<T>... comparators) {
        Collections.sort(list, new MultiComparator<>(comparators));
    }
}
```
