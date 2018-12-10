---
title: RecyclerView 控件的使用
date: 2017-08-02 10:19:42
tags:
- RecyclerView
---

### RecyclerView 控件的使用

> 前言 最近在做一个滑动列表的需求，现将 RecyclerView 的使用知识记录总结下

### 概述

[RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView) 是 android.support.v7 包下的一个组件，根据官方文档说明，它是一个可以在有限屏幕区域提供大量数据集的灵活视图组件。

那么它和我们熟悉的 ListView 和 GridView 有什么区别呢，或者说对比已有的两个组件，RecyclerView 有什么优势呢？整体来说，RecyclerView 提供了更为灵活的体验，高度解耦，开发者可以使用不同的 LayoutManager，ItemDecoration，ItemAnimator 来实现各种自定义效果。

<!-- more -->

- LayoutManager 通过布局管理器，可以轻松实现类似 ListView，GridView 和 流式布局等效果
- ItemDecoration 通过它可以控制 Item 间的间隔，实现自定义间隔样式
- ItemAnimator 使用它来控制 Item 的增删等动画效果
- 点击事件 RecyclerView 的点击事件需要自己来实现（提供了 OnItemTouchListener 接口）

### 使用

#### 导入库包

在 gradle 文件中引入 v7 包

```
compile 'com.android.support:recyclerview-v7:21.0.3'
```
#### 布局中使用

在 xml 布局文件中使用

```
<android.support.v7.widget.RecyclerView
    android:layout_width="match_parent"
    android:layout_height="match_parent">
</android.support.v7.widget.RecyclerView>
```

#### Activity 中设置

在 Activity 中获取 RecyclerView 组件，进行以下设置：

- 设置数据适配器（必须）
- 设置布局管理器（必须）
- 设置 Item 增加、移除动画（非必须）
- 添加分割线（非必须）

#### Adapter

继承实现 `RecyclerView.Adapter<VH extends ViewHolder>` 类，作为 `RecyclerView` 的数据适配器，主要实现以下几个方法

- onCreateViewHolder（创建 ViewHoldre，初始化填充 ItemView 布局）
- onBindViewHolder（绑定 ViewHolder，进行数据的展示处理）
- getItemViewType（获取 ItemView 类型，主要运用于多布局）
- getItemCount（获取 Item 个数）

***注意:*** 在 onCreateViewHolder 中，填充 View 时建议使用如下格式：

```
View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_1, parent, false);
```
#### LayoutManager

`RecyclerView.LayoutManager` 是一个抽象类，系统默认提供了三个实现类：

- LinearLayoutManager 线性布局管理器，支持横向及纵向布局
- GridLayoutManager 网格式布局管理器
- StaggeredGridLayoutManager 瀑布流式布局管理器

将不同布局管理器设置到 RecyclerView 中，即可实现不同的布局；当然也可以继承 RecyclerView.LayoutManager 从而实现自定义布局

#### ItemDecoration

通过 `RecyclerView.addItemDecoration()` 方法可以添加分割线，该类为一个抽象类，开发者需要自己来进行实现

```
public static abstract class ItemDecoration {

    public void onDraw(Canvas c, RecyclerView parent, State state) {
        onDraw(c, parent);
    }

    @Deprecated
    public void onDraw(Canvas c, RecyclerView parent) {
    }

    public void onDrawOver(Canvas c, RecyclerView parent, State state) {
        onDrawOver(c, parent);
    }

    @Deprecated
    public void onDrawOver(Canvas c, RecyclerView parent) {
    }

    @Deprecated
    public void getItemOffsets(Rect outRect, int itemPosition, RecyclerView parent) {
        outRect.set(0, 0, 0, 0);
    }

    public void getItemOffsets(Rect outRect, View view, RecyclerView parent, State state) {
        getItemOffsets(outRect, ((LayoutParams) view.getLayoutParams()).getViewPosition(),parent);
    }
}
```

当我们使用 `RecyclerView.addItemDecoration`` 方法时，RecyclerView 会进行绘制，主要是通过 onDraw()` 和 `onDrawOver()` 方法

- onDraw() : 绘制分割线
- onDrawOver() : 在 onDraw() 之后绘制，一般覆写其中一个即可
- getItemOffsets() : 通过 outRect.set() 为每个 item 设置偏移量，即用来设置分割线的宽、高

#### Item Animator

RecyclerView 能够通过 `mRecyclerView.setItemAnimator(ItemAnimator animator)` 设置添加、删除、移动、改变的动画效果。RecyclerView 提供了默认的 ItemAnimator 实现类：`DefaultItemAnimator`

#### 多布局实现

RecyclerView 可以通过 getItemViewType 获取不同的布局类型，进行对应布局的填充，有点类似 ListView ；利用这个接口可以轻松实现头布局和尾布局的添加

### 结语

综合来看，RecyclerView 提供了一种低耦合，插拔式的滑动布局体验，通过 LayoutManager 可以只需一行代码实现布局格式的切换，轻松实现 ListView、GridView 等布局效果，更是可以支持自定义分割线处理。

### 参考

- [Android RecyclerView 使用完全解析](https://blog.csdn.net/lmj623565791/article/details/45059587)
- [DividerItemDecoration](https://gist.github.com/alexfu/0f464fc3742f134ccd1e)
- [RecyclerView 优秀文集](https://github.com/CymChad/CymChad.github.io)
