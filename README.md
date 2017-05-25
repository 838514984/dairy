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
今天学了pathMeasure，很实用，可以用来画轨迹动图。
mPopupWindow.setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_ADJUST_RESIZE);<br>
可以解决从底部弹出，被虚拟按键挡住。<br>
## 2017/4/26 15.46<br>
（1）matrix 的ployToploy可以用来制作一些动图的特效，拉拽。<br>
（2）在onmeasure中如果区别多个点的动作，可以判断收点击的坐标，和目标点击坐标进行判断，设置个临界值，相减如果小于临界值则进行下一步惭怍。<br>
（3）NumberPicker.setDisplayedValues(new String[]{"天","周","年"});<br>
 　　NumberPicker.setMinValue(0);<br>
 　　NumberPicker.setMaxValue(2);<br>
   　可以显示任意单位<br>
    
## 2017/4/28 15.43<br>
（1）转圈圈的自定义View可以用pathMeasure，不断截取，stop为length*duration，start=stop-从（0,1,0）乘以一个固定数。<br>
(2) 今日重点！！！<br>
　　matrix.postTranslate(100,100);<br>
　　matrix.postScale(0.5f,0.5f);<br>
　　matrix.postRotate(15);<br>
　　matrix.postTranslate(-100,-100);<br>
  等价于：<br>
　　matrix.postScale(0.5f,0.5f);<br>
　　matrix.postRotate(15);<br>
　　matrix.preTranslate(100,100);<br>
　　matrix.postTranslate(-100,-100);<br>
  前程和后乘的区别。<br>

## 2017/05/24
(1)GetTop getBottom getright getleft 都是获取相对父容器的、
 
(2)e.getX和e.getY的区别：
 
（3）安卓中的颜色
 
(4) Websocket.
在websocket的世界中总共有4种事件—— onopen、onclose、onmessage和onerror

    【onopen】websocket建立连接完成

    【onclose】websocket连接被关闭或无法建立连接

    【onmessage】websocket收到数据，发送数据对应socket.send方法

【onerror】websocket发生错误

2017/05/25
（1）	使用Zxing解析生成二维码，并生成bitmap，可以给ImageView使用。
    public static Bitmap Create2DCode(String text) throws WriterException {    
        //生成二维矩阵,编码时指定大小,不要生成了图片以后再进行缩放,这样会模糊导致识别失败    
        Hashtable<EncodeHintType, Object> hints = new Hashtable<EncodeHintType, Object>();  
        hints.put(EncodeHintType.CHARACTER_SET, "utf-8");  
        hints.put(EncodeHintType.MARGIN, 0);  
        BitMatrix matrix = new MultiFormatWriter().encode(text,BarcodeFormat.QR_CODE, 400, 400,hints);    
        int width = matrix.getWidth();    
        int height = matrix.getHeight();    
        //二维矩阵转为一维像素数组,也就是一直横着排了    
        int[] pixels = new int[width * height];    
        for (int y = 0; y < height; y++) {    
            for (int x = 0; x < width; x++) {    
                if(matrix.get(x, y)){    
                    pixels[y * width + x] = 0xff000000;    
                } else {   
                    pixels[y * width + x] = 0xffffffff;   
                }     
                    
            }    
        }    
            
        Bitmap bitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);    
        //通过像素数组生成bitmap,具体参考api    
        bitmap.setPixels(pixels, 0, width, 0, 0, width, height);    
        return bitmap;    
    }  
