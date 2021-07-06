# Matplotlib

## 一、简介

> 画二维图表的python库，数据可视化的关键工具

mat - matrix矩阵   plot - 画图   lib - library库

matlab = mat + lab（矩阵实验室）



## 二、Matplotlib三层架构

### 1 容器层

- Canvas（画板，用于放置画布）
- Figure（画布）
- Axes（画图区或坐标系）

> 特点：
>
> ​		（1）一个figure可有多个axes，但一个axes只能属于一个figure
>
> ​		（2）一个axes可有多个axis，包含两个即为2d坐标系，3个即为3d坐标系



### 2 辅助显示层

其为axes内的除了根据数据绘制出的图像以外的内容，包括：

- 外观 facecolor
- 坐标轴 axis
- 坐标轴名称 axis label
- 坐标轴刻度 tick
- 网格线 grid
- 标题 title



### 3 图像层

根据数据绘制出的图像



## 三、折线图与基础绘图

#### 1. matplotlib.pyplot

```python
# 类似于matlab画图函数
import matplotlib.pyplot as plt
```



#### 2. demo

```python
# 一周内温度的情况

# 1. 创建画布
plt.figure()
# 2. 绘制图像
plt.plot([1,2,3,4,5,6,7],[16,14,16,11,12,33,25])
# 3. 显示图像
plt.show()
```



#### 3. 设置画布属性与图片保存

```python
# figsize:指定画布长宽，dip:画布的清晰度
plt.figure(figsize=(20,8), dip=80)
# 保存图像到path
plt.savefig(path)
```

注意：plt.show()后会释放figure资源，所以若在显示图像后再保存，则保存的是空图片。



## 四、总结

![image-20210524163627172](image/image-20210524163627172.png)