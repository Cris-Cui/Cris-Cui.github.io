# 图形视图框架

整个图形视图结构主要包含三部分：

- 场景（`Scene`）

- 视图（`View`）

- 图形项（`Item`）

**它们分别对应 `QGraphicsScene` 、`QGraphicsView` 、`QGraphicsItem`三个类。**

`Scene`是管理`Item`的容器，但其本身无法可视化, 需要使用`View`.

## QGraphicsItem图形项

- （一）自定义图形项
- （二）光标和提示
- （三）拖放
- （四）键盘与鼠标事件
- （五）碰撞检测
- （六）移动
- （七）动画
- （八）右键菜单

`QGraphicsItem`类是所有图形项的基类, 支持以下功能：

- 鼠标的按下、移动、释放和双击事件，也支持鼠标悬停、滚轮和右键菜单事件。
- 键盘输入焦点和键盘事件
- 拖放
- 利用QGraphicsItemGroup进行分组
- 碰撞检测

要继承`QGraphicsItem`类实现自定义的图形项，必须先实现两个纯虚函数

- `boundingRect()` 定义`Item`的绘制范围
- `paint()` 绘制图形项

## QGraphicsScene场景

它有以下功能：

- 提供了一个管理大量图形项的快速接口
- 向每个图形项传播事件
- 管理图形项的状态，比如选择和焦点处理
- 提供无转换的渲染功能，主要用于打印

### 场景坐标

  场景表示其所有项的基础坐标系。场景坐标系描述了每个顶层项目的位置，也构成了从视图传递到场景的所有场景事件的基础。场景中的每个项目都有一个场景位置和边界矩形(QGraphicsItem::scenePos()和  QGraphicsItem::sceneBoundingRect())，除了其本地项pos和边界矩形之外。场景位置描述了项目在场景坐标中的位置，其场景边界矩形构成了QGraphicsScene如何确定场景的哪些区域已更改的基础。场景中的更改通过QGraphicsScene::changed()信号进行通信，参数是场景矩形的列表。

### 成员

#### 类型成员

1、QGraphicsScene::ItemIndexMethod：场景的索引算法。

QGraphicsScene::BspTreeIndex：二进制空间分区树算法。所有图形项定位算法的数量级都接近对数复杂度。添加，移动和删除图形项是对数的。这种方法最适合静态场景（即大多数图形项不移动的场景）。
QGraphicsScene::NoIndex：不保存图形项索引。定位图形项具有线性复杂性，因为会搜索场景中的所有图形项。但是，添加，移动和删除图形项是在固定时间内完成的。这种方法最适合动态场景，在动态场景中，要连续添加，移动或删除许多图形项。
2、QGraphicsScene::SceneLayer：该枚举描述了场景中的渲染层。 当场景在绘制内容时，它将按顺序分别渲染每个图层。每一层代表一个标志。

绘制顺序：背景 > 图形项 > 前景。

QGraphicsScene::ItemLayer：图形项层。 场景通过调用虚函数drawItems()来渲染的所有图形项在此层中。图形项层绘制在背景层之后，但在前景层之前。
QGraphicsScene::BackgroundLayer：背景层。 场景通过调用虚函数drawBackground()在此层中渲染场景的背景。首先绘制的是背景层。
QGraphicsScene::ForegroundLayer：前景层。 场景通过调用虚函数drawForeground()在此层中渲染场景的前景。前景层绘制在所有层的最后。
QGraphicsScene::AllLayers：所有层，该值代表所有三层的组合。

#### 属性成员

1、backgroundBrush : QBrush 背景层画刷。例如可以它设置场景的背景网格：

#### 成员函数

1、void advance()

> 调用场景中所有图形项的QGraphicsItem::advance()函数。

2、[信号] void changed(const QList<QRectF> ®ion)

> 场景内容发生更改时发出的信号（场景任何更改都会发此信号，此信号发送极其频繁）。参数包含场景矩形的列表，这些矩形指示已更改的区域。

3、void clear()

>  从场景中删除所有图形项。（移除并且delete删除）

4、void clearSelection()

>  清除当前选择状态。

5、[信号] void focusItemChanged(QGraphicsItem *newFocusItem, QGraphicsItem *oldFocusItem, Qt::FocusReason reason)

>  每当场景中的焦点发生变化时（某项获得或失去输入焦点时（涉及一个图形项），或者当焦点从一个图形项传递到另一个图形项时（涉及两个图形项）），场景都会发出此信号。 如果需要跟踪其他图形项何时获得输入焦点，则可以连接到此信号。
>
> oldFocusItem是指向以前具有焦点的图形项的指针，如果在发出信号之前没有图形项具有焦点，则为nullptr。newFocusItem是指向获得输入焦点的图形项的指针；如果焦点丢失，则返回nullptr。参数3见QGraphicsItem详解的第61个成员函数。

6、bool focusNextPrevChild(bool next)

>  查找一个新的QGraphicsWidget，以使键盘焦点对准Tab和Shift + Tab键，如果可以找到则返回true；否则返回false。如果next为true则此函数向前搜索，否则向后搜索。

 7、void invalidate(const QRectF &rect = QRectF(), QGraphicsScene::SceneLayers layers = AllLayers)

>  使场景中的rect无效并重绘。图层中的所有缓存内容都将无条件无效并重新绘制。 

 8、[信号] void sceneRectChanged(const QRectF &rect)

>  场景矩形发生变化时发出此信号。参数是新的场景矩形。

9、[信号] void selectionChanged() 

>  选择改变时发出这个信号。可以调用selectedItems()来获取所选图形项的新列表。无论何时选择或取消选择图形项、设置、清除或以其他方式更改选择区域、将预选图形项添加到场景或从场景中移除选定图形项，选择都会改变。
>
> 对于组选择操作，场景只发出一次此信号。

10、void update(const QRectF &rect = QRectF())

>  重绘参数中的区域。

11、void drawBackground(QPainter *painter, const QRectF &rect) （虚函数）

>  绘制场景的背景层。所有绘制都在场景坐标中完成。rect参数是暴露的矩形。

12、void drawForeground(QPainter *painter, const QRectF &rect) （虚函数）

>  绘制场景的前景层。所有绘制都在场景坐标中完成。rect参数是暴露的矩形。

13、QGraphicsItem *focusItem()

>  当场景处于活动状态时，此函数将返回场景的当前焦点项，如果当前没有焦点，则返回nullptr。当场景处于非活动状态时，此函数返回在场景变为活动状态时将获得输入焦点的图形项。当场景接收到按键事件时，焦点项将接收键盘输入。22、bool hasFocus()
>
> 场景是否有焦点。如果场景有焦点，它将把按键事件从QKeyEvent转发到有焦点的图形项。

### 事件处理和传播

场景可以传播来自视图的事件，将事件传播给该点最顶层的图形项。但是就像我们在讲图形项时所说的那样，如果一个图形项要接收键盘事件，那么它必须获得焦点。而且，如果我们在场景中重写了事件处理函数，那么在该函数的最后，必须调用场景默认的事件处理函数，只有这样，图形项才能接收到该事件。

## QGraphicsView视图

`QGraphicsView` 提供了视图窗口部件，它使场景的内容可视化。你可以给一个场景关联多个视图，从而给一个数据集提供多个视口。视图部件是一个滚动区域，就是说，它可以提供一个滚动条来显示大型的场景。如果要使用OpenGL，你可以使用`QGraphicsView::setViewport()`函数来添加`QGLWidget` 。

### 视图坐标

  视图坐标是小部件的坐标。视图坐标中的每个单元对应一个像素。这个坐标系的特殊之处在于它相对于小部件或视区，并且不受观察到的场景的影响。**QGraphicsView的视区的左上角始终是(0，0)，右下角始终是(视区宽度，视区高度)。所有鼠标事件和拖放事件最初都作为视图坐标接收，您需要将这些坐标映射到场景，以便与项目交互。**