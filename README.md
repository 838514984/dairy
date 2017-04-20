# dairy<br>
2017/4/20 10:24<br>
在测量完View并使用setMeasuredDimension函数之后View的大小基本上已经确定了,因为View的大小不仅由View本身控制，而且受父控件的影响，所以我们在确定View大小的时候最好使用系统提供的onSizeChanged回调函数。<br>
在oncreate中直接获得view的宽高是为0，可以用MyView.post添加队列获取
