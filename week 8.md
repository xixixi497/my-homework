Week 7

## Week5重做：柱形图和折线图

``` r
p1 <- ggplot(data=efficiency)+ 
  geom_col(mapping=aes(x=year,y=total),col="black",fill="cadetblue2")+ 
  scale_y_continuous(expand = c(0, 0.1),limits=c(0,10000))+ 
  labs(title="日本2012-2016年焚烧厂实际总发电量及发电效率",x=NULL,y="实际总发电量(GWh/年)")+ 
  theme_classic()

p2 <- ggplot(data=efficiency,mapping=aes(x=year,y=efficiency)) +
  geom_line(aes(group=1))+
  geom_point(shape=23,size=2,fill="turquoise")+
  labs(x="年份",y="发电效率(%)")+
  theme_classic()

p1+p2+plot_layout(ncol=1,heights=c(2.5,1))
```

![](images/7_1.png)

原图：

![](images/5_efficiency.jpg)

遇到的问题有：

* 确定柱形对应的几何对象，`geom_bar()`好像只能提供一个x轴，最后确定应该用`geom_col()`
* 改变柱形的样式，边框颜色`col`，填充色`fill`
* 改变y轴的刻度，在 R Graphics Cookbook 网站上看到应该用 `scale_y_continuous`
* 在折线图上添加数据标记，叠加一次`geom_point()`函数生成的图像

另外，因为 ggplot2 好像不支持双坐标轴，我就想用另一种方式将柱形图和折线图结合在一起。最后搜索到了 patchwork 这个包，可以用一种很简单的方法组合 ggplot2 生成的图形，但不能直接输出图形，而是要将 ggplot2 的输出存储在某个变量里，再用加好连接不同的图形变量，最后用`plot_layout()` 调整相对大小。





## Week6重做：圆环图

``` r
ggplot(data=minimum_age,mapping=aes(ymax=ymax,ymin=ymin,xmax=4,xmin=3,fill=age)) +
  geom_rect() +
  geom_label(x=4.5,aes(y=labelPosition,label=label),size=4) +
  scale_fill_brewer(palette=4) +
  coord_polar(theta="y") +
  xlim(c(1, 4)) +
  theme_void() +
  theme(legend.position = "none")
```

![](images/7_2.png)

原图(取整后再数值上有很小的差异)：

![](images/6_r_3.jpg)

在 ggplot2 中做这种圆环图比较麻烦，因为没有直接对应的几何对象，必须做一次坐标系转换。最后在 [R Graphics Gallery](https://www.r-graph-gallery.com/128-ring-or-donut-plot.html) 网掌上找到了一般圆环图对应的代码，模仿出了这张图。
