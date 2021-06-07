# CSS十六进制颜色

## 十六进制运算方式

十进制运算是当个位数的值>=10时，给十位数的值+1，比如 9+1=10   13+17=30。

十六进制中添加了 A、B、C、D、E、F 分别用来表示 10、11、12、13、14、15 这6个数值。

![img](https://i.loli.net/2021/06/07/yrCkJEjRe1zmcPZ.png)

故名思义，十六进制就是当个位数的值>=16时，给十位数的值加1。

举几个🌰：

- 1F = 1✖️16 + 15 = 31 （ 一个十六和一个十五 ）
- 2B = 2✖️16 + 11 = 43 （ 两个十六和一个十一 ）
- 41 = 4✖️16 + 1 = 65 （ 四个十六和一个一 ）
- AA = 10✖️16 + 10 = 170（十个十六和一个十）
- F0 = 15✖️16 + 0 = 240（十五个十六和一个零）

## 设置颜色的方法

比如现在要设置一个红色背景黄色字体的样式

![image-20210607144546161](https://i.loli.net/2021/06/07/xesrOHaUi2gIWjq.png)

**颜色名**

```css
p {
  color:yellow;
  background-color:red;
}
```

通过颜色名设置颜色是一种简单的方式，但当你设置一些如 “薰衣草紫”、“桃红”等颜色时，就会很难想到对应的单词。

**RGB**

```css
p {
  color:rgb(255,255,0);
  background-color:rgb(255,0,0);
}
```

或者

```css
p {
  color:rgb(100%,100%,0%);
  background-color:rgb(100%,0%,0%);
}
```

这种写法，冗长并复杂，有额外的括号，逗号，和/或百分比符号，很容易写成不是你想要的颜色或者出错。

**十六进制表示**

```css
p {
  color:#ffff00;
  background-color:#ff0000;
}
```

这里的 ff 就等于 15✖️16 + 15 = 255

看一下Wiki中的解释：

> 在[HTML](https://zh.wikipedia.org/wiki/HTML)和[CSS](https://zh.wikipedia.org/wiki/CSS)中使用3[位元組](https://zh.wikipedia.org/wiki/位元组)共6个[十六进制](https://zh.wikipedia.org/wiki/十六进制)数字表示一种颜色，每位元組从00到FF，相当十进位数字从0到255，按顺序前两位是[红色](https://zh.wikipedia.org/wiki/红色)的值，中间两位是[绿色](https://zh.wikipedia.org/wiki/绿色)的值，最后两位是[蓝色](https://zh.wikipedia.org/wiki/蓝色)的值。
>
> 由于[网页](https://zh.wikipedia.org/wiki/网页)是基于计算机浏览器开发的媒体，所以颜色以光学[颜色](https://zh.wikipedia.org/wiki/颜色)RGB（红、绿、蓝）为主。

![img](https://i.loli.net/2021/06/07/D2MlV5Z7ktUTYSO.png)

![img](https://i.loli.net/2021/06/07/M3Ea7qLPpvt2gXW.png)

## 颜色的亮度

**更高的数字 = 更多的光线 = 接近白色 = 明亮的颜色。**

**较低的数字 = 较少的光线 = 接近黑色 = 较暗的颜色。**



十六进制的 80 是在 00 和 FF 中间，所以我们可以使用这个值来显示一些 **半光** 的颜色，可以像这样。

![img](https://pic3.zhimg.com/80/v2-93e3063592f199ab042af090c29d214e_720w.png)

我们在这里做的，通过改变三个分量的值，仅仅是改变了光的数量，我们把代码放入每种颜色的代码中，从没有（00） 到一半（80） 到全部（FF）。

## 例子

1. 浅灰色背景，深蓝色文字

![image-20210607151406361](https://i.loli.net/2021/06/07/1gTlBEmXQL769fS.png)

```css
p {
  /* 80=8*16+0=128 表示蓝色的色值为128 */ 
  color:#000080;  
  /* f0=15*16+0=240 表示红绿蓝三种色值都为240*/
  background-color: #f0f0f0;
}
```

**三种颜色值相同并且不为0和255时，一定是灰色，亮度由值的大小决定**

2. 绿色背景，黄色文字

   ![image-20210607152542504](https://i.loli.net/2021/06/07/zCe78NGvrhg6a5Q.png)

```css
p {
  /* 红+绿=黄，ff=15*16+15=255，红255，绿255，蓝0 */
  color:#ffff00;
  background-color:#00ff00;
}
```

3. 黑色背景，橙色文字

   ![image-20210607153522708](https://i.loli.net/2021/06/07/PCW8xtdwKvLy7mD.png)

```css
p {
  /* ff=15*16+15=255，80=8*16+0=128，红255，绿128，蓝0 */
  color:#ff8000; 
  /* 红0 绿0 蓝0 */
  background-color: #000000;
}
```

## 总结

- 十六进制并不难，只是相比十进制多了几个数字。用十进制表示数值时用的是 0 - 255，而写成十六进制时则是用 00-FF。

- 它总是3对十六进制的数字，总是以红-绿-蓝的顺序排列,更高的数字总是意味着更加明亮的颜色（反之亦然）。