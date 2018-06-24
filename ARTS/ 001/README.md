# 1. LeetCode

## 11. Container With Most Water

Given *n* non-negative integers *a1*, *a2*, ..., *an*, where each represents a point at coordinate (*i*, *ai*). *n* vertical lines are drawn such that the two endpoints of line *i* is at (*i*, *ai*) and (*i*, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

```scala
object Solution {
  def maxArea(height: Array[Int]): Int = {
    var left = 0;
    var right = height.length - 1
    var maxArea = 0

    while (left < right) {
      maxArea = Math.max(maxArea, Math.min(height(left), height(right)) * (right - left))

      if (height(left) < height(right)) {
        left += 1
      } else {
        right -= 1
      }
    }
    return maxArea
  }
}
```

##2. Review(https://docs.scala-lang.org/tutorials/scala-for-java-programmers.html)

### A SCALA TUTORIAL FOR JAVA PROGRAMMERS

这份文档主要是面向具有想学习Scala的具有Java基础的程序员。

## 3. Tips
早起可以更高效的生活。

## 4. Shares
最近在看MDN上关于Web开发的基础知识。目前正在阅读HTTP相关知识，正在整理笔记中。
