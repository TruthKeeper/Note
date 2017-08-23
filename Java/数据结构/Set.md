> 继承于Collection接口，存放无序，不可重复的数据集合

## HashSet

基于`HashMap`的原理存放KEY，VALUE都是一个new Object()，通过哈希运算（对hashcode进行二次哈希运算，将hashcode与低16位的进行异或运算）

> 特点：无序，不可重复，可以用new HashSet<>(Collection c)来处理重复条目

## LinkedHashSet

基于`LinkedHashMap`的原理实现，所以数据结构是有序的

## TreeSet

基于`TreeMap`的原理实现，升序排列，存放基本类型、String以及实现`Comparable`接口的类



