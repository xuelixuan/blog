# grid 布局笔记


grid布局的属性分为两种:
- 容器上的属性 container
- 项目上的属性 item

## container 容器属性:
- display: grid 容器布局
- grid-template-columns 每一列的宽
    - repeat
    - auto-fill
    - fr
    - minmax
    - auto
    - 网格线名称
    - 布局实例
- grid-template-rows 每一行的高
- grid-template-areas 指定区域,一个区域由单个或多个单元格组成

- grid-row-gap 行与行的间距
- grid-column-gap 列与列的间距
- grid-gap 行与列间距的简写形式

- grid-auto-flow 项目的排放顺序,默认先行后列
    - :row
    - :column 先列后行
    - 指定某些项目以后,剩下的项目怎么自动放置
        - :row dense
        - :column dense

- justify-items 单元格里内容的水平位置
- align-items 单元格里内容的垂直位置
- place-items 是上面两个属性的简写
    - align
    - justify

- justify-content, align-content, place-content 整个内容区域在容器里的水平和垂直位置


- grid-auto-columns 指定在现有网格之外的项目的宽 
- grid-auto-rows 指定在现有网格之外的项目的高

## item上的属性

指定当前item的四个边框分别在那根网格线上
- grid-column-start 左边框所在的垂直网线
- grid-column-end 又边框所在的垂直网线
- grid-row-start 上边框所在的水平网线
- grid-row-end 下边框所在的水平网线

也可以使用span表示"跨越",左右边框或上下边框之间跨越多少个网格

```css
- grid-column-start: span 2; 左右边框之间跨越2个网格
```

```css
.item-1 {
    grid-column-start: 2; // 左边框是第二根垂直网线,宽度
    grid-column-end: 4; // 有边框是第四根垂直网线,宽度
    grid-row-start: 1; // 上边框距离水平网线,高度
    grid-row-end:1;// 下边框距离水平网线,高度
}
```

- grid-area 指定项目放到哪个区域

```css
.item-1 {
    grid-area: e;
}
```


- justify-self 当前单元格内容的水平位置
- align-self 当前单元格内容的垂直位置
- place-self 简写