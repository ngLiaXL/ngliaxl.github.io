---
layout: post
title:  "[Java]	List根据属性去重"
date:   2018-07-10 11:53:27 +0800
categories: android
---


Remove duplicates from a list of objects based on property


```
public class Wrapper<T, U> {
    private T t;
    private Function<T, U> equalityFunction;

    public Wrapper(T t, Function<T, U> equalityFunction) {
        this.t = t;
        this.equalityFunction = equalityFunction;
    }

    public T unwrap() {
        return this.t;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        @SuppressWarnings("unchecked")
        Wrapper<T, U> that = (Wrapper<T, U>) o;
        return Objects.equals(equalityFunction.apply(this.t), that.equalityFunction.apply(that.t));
    }

    @Override
    public int hashCode() {
        return Objects.hash(equalityFunction.apply(this.t));
    }

    public interface Function<T, R> {
        R apply(T t);
    }

}
```





```
public static <T, U> void distinct(List<T> sourceList, Wrapper.Function<T, U> equalityFunction) {
        List<Wrapper<T, U>> wrapperList = new ArrayList<>();
        for (T t : sourceList) {
            wrapperList.add(new Wrapper<>(t, equalityFunction));
        }
        LinkedHashSet<Wrapper<T, U>> set = new LinkedHashSet<>(sourceList.size());

        set.addAll(wrapperList);
        wrapperList.clear();
        wrapperList.addAll(set);

        sourceList.clear();
        for (Wrapper<T, U> wrapper : wrapperList) {
            sourceList.add(wrapper.unwrap());
        }
    }
```
