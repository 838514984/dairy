# dairy<br>
## 2017/4/20 10:24<br>
在测量完View并使用setMeasuredDimension函数之后View的大小基本上已经确定了,因为View的大小不仅由View本身控制，而且受父控件的影响，所以我们在确定View大小的时候最好使用系统提供的onSizeChanged回调函数。<br>
在oncreate中直接获得view的宽高是为0，可以用MyView.post添加队列获取<br><br>
## 2017/4/24 15.50<br>
事件传递顺序是 activity-phoneWindow-DecoreView-rootView-childView.如果没有消费则返回，View是可点击的则一定消费事件<br>
容器事件消费伪代码<br>
public boolean dispatchTouchEvenr(MotionEvent event){<br>
　　  boolean request=false;<br>
  　　if(!oninterceptTouchEvent(event)){<br>
     　　request=child.dispatchTouchEvent(event)<br>
  　　}<br>
  　　if(!request){<br>
    　　request=this.touchEvent(event)<br>
  　　}<br>
  　　return request;<br>
}<br>

<br>自定义view点击事件，比如画一个圆，只接受圆内的点击事件，而不是接受全部Rect中的点击事件，可以用Region.setPath，Path与Regin的交集，然后可以在onTouchEvent中获取点击事件坐标，判断是不是在Region之间，region.contain.<br>
## 2017/4/25 17.25<br>
今天学了pathMeasure，很实用。
mPopupWindow.setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_ADJUST_RESIZE);<br>
可以解决从底部弹出，被虚拟按键挡住。<br>
