NAME 名称
    tk_dragtool

DESCRIPTION 简介
    提供用鼠标拖动、缩放tkinter控件工具的模块。
    A module providing tools to drag and resize
    tkinter window and widgets with the mouse.

FUNCTIONS 函数
    bind_drag(tkwidget, dragger)
        绑定拖曳事件。
        tkwidget: 被拖动的控件或窗口,
        dragger: 接收鼠标事件的控件,
        调用bind_drag后,当鼠标拖动dragger时, tkwidget会被带着拖动, 但dragger
        作为接收鼠标事件的控件, 位置不会改变。
        Binds a drag event.
        tkwidget: The widget or window to be dragged.
        dragger: The widget that receives mouse events.
        After calling bind_drag, when the mouse drags the dragger, the tkwidget will be dragged along, but the dragger, as the widget receiving mouse events, will not change its position.

    bind_resize(tkwidget, dragger, anchor, min_w=0, min_h=0, move_dragger=True)
        绑定缩放事件。
        anchor: 缩放的方位, 取值为N,S,W,E,NW,NE,SW,SE,分别表示东、西、南、北。
        min_w,min_h: 该方向tkwidget缩放的最小宽度(或高度)。
        move_dragger: 缩放时是否移动dragger。
        其他参数同bind_drag函数。
        Binds a resize event.
        anchor: The direction of resizing, with possible values N, S, W, E, NW, NE, SW, SE.
        min_w, min_h: The minimum width (or height) for resizing the tkwidget in that direction.
        move_dragger: Whether to move the dragger during resizing.
        Other arguments are the same as for the bind_drag function.

    draggable(tkwidget)
        调用draggable(tkwidget) 使tkwidget可拖动。
        tkwidget: 一个控件(Widget)或一个窗口(Wm)。
        x 和 y: 只允许改变x坐标或y坐标。
        Calling draggable(tkwidget) makes the tkwidget draggable.
        tkwidget: A widget (Widget) or a window (Wm).
        x and y: Allows changing only the x-coordinate or y-coordinate.

    move(widget, x=None, y=None, width=None, height=None)
        移动控件或窗口widget至指定坐标, 参数都为可选参数。
        Moves the widget or window widget to the specified coordinates. All parameters are optional.

EXAMPLES 示例

.. code-block:: python

    import tkinter as tk
    from tk_dragtool import draggable
    
    root=tk.Tk()
    btn=tk.Button(root,text="Drag")
    draggable(btn)
    btn.place(x=0,y=0)
    root.mainloop()

运行效果 Screenshot:

.. image:: https://img-blog.csdnimg.cn/47f8708a1eef42d591e922b8b0eb12d7.png
    :alt: 效果图

更复杂的示例, 实现了8个缩放手柄的功能:
A more complex example implementing 8 scaling handles:

.. code-block:: python

    btns=[] # 用btns列表存储创建的按钮
    def add_button(func,anchor):
        # func的作用是计算按钮新坐标
        b=ttk.Button(root)
        b._func=func
        bind_resize(btn,b,anchor)
        x,y=func()
        b.place(x=x,y=y,width=size,height=size)
        b.bind('<B1-Motion>',adjust_button,add='+')
		b.bind('<B1-ButtonRelease>',adjust_button,add='+')
        btns.append(b)
    def adjust_button(event=None):
        # 改变大小或拖动后,调整手柄位置
        for b in btns:
            x,y=b._func()
            b.place(x=x,y=y)
    root=tk.Tk()
    root.title("Test")
    root.geometry('500x350')
    btn=ttk.Button(root,text="Button")
    draggable(btn)
    btn.bind('<B1-Motion>',adjust_button,add='+')
	btn.bind('<B1-ButtonRelease>',adjust_button,add='+')
    x1=20;y1=20;x2=220;y2=170;size=10
    btn.place(x=x1,y=y1,width=x2-x1,height=y2-y1)
    root.update()
    # 创建各个手柄, 这里是控件缩放的算法
    add_button(lambda:(btn.winfo_x()-size, btn.winfo_y()-size),
               'nw')
    add_button(lambda:(btn.winfo_x()+btn.winfo_width()//2,
                       btn.winfo_y()-size), 'n')
    add_button(lambda:(btn.winfo_x()+btn.winfo_width(), btn.winfo_y()-size),
               'ne')
    add_button(lambda:(btn.winfo_x()+btn.winfo_width(),
                       btn.winfo_y()+btn.winfo_height()//2),'e')
    add_button(lambda:(btn.winfo_x()+btn.winfo_width(),
                       btn.winfo_y()+btn.winfo_height()), 'se')
    add_button(lambda:(btn.winfo_x()+btn.winfo_width()//2,
                       btn.winfo_y()+btn.winfo_height()),'s')
    add_button(lambda:(btn.winfo_x()-size, btn.winfo_y()+btn.winfo_height()),
               'sw')
    add_button(lambda:(btn.winfo_x()-size,
                    btn.winfo_y()+btn.winfo_height()//2), 'w')
    root.mainloop()

效果图 Screenshot:

.. image:: https://img-blog.csdnimg.cn/a64c54ff7c7148d7b943ff194dbc5292.gif
    :alt: 更复杂示例的效果图

版本:1.1.4.1 (更新: 修复了文档中的小错误)

Version: 1.1.4.1 (Update: Fixed small mistakes in the documentation)

作者:``七分诚意 qq:3076711200``

作者CSDN主页: https://blog.csdn.net/qfcy\_/

Github: https://github.com/qfcy