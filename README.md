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
(1)GetTop getBottom getright getleft 都是获取相对父容器的、<br>
 ![](https://github.com/838514984/dairy/blob/master/image/%E7%9B%B8%E5%AF%B9%E5%9D%90%E6%A0%87.png)<br><br>
(2)e.getX和e.getY的区别：<br>
 ![](https://github.com/838514984/dairy/blob/master/image/%E7%94%9F%E5%9D%90%E6%A0%87.jpg)<br>
（3）安卓中的颜色<br>
 
(4) Websocket.<br>
在websocket的世界中总共有4种事件—— onopen、onclose、onmessage和onerror<br>

    【onopen】websocket建立连接完成<br>

    【onclose】websocket连接被关闭或无法建立连接<br>

    【onmessage】websocket收到数据，发送数据对应socket.send方法<br>

【onerror】websocket发生错误<br>

## 2017/05/25
（1）	使用Zxing解析生成二维码，并生成bitmap，可以给ImageView使用。<br>
    public static Bitmap Create2DCode(String text) throws WriterException {    <br>
        //生成二维矩阵,编码时指定大小,不要生成了图片以后再进行缩放,这样会模糊导致识别失败    <br>
　　Hashtable<EncodeHintType, Object> hints = new Hashtable<EncodeHintType, Object>();  <br>　
　　hints.put(EncodeHintType.CHARACTER_SET, "utf-8");  <br>
　　hints.put(EncodeHintType.MARGIN, 0);  　<br>
　　BitMatrix matrix = new MultiFormatWriter().encode(text,BarcodeFormat.QR_CODE, 400, 400,hints);   <br> 
　　int width = matrix.getWidth();    　<br>
　　int height = matrix.getHeight();    <br>
　　//二维矩阵转为一维像素数组,也就是一直横着排了    　<br>
　　int[] pixels = new int[width * height];    <br>
　　for (int y = 0; y < height; y++) {    　<br>
　　　　for (int x = 0; x < width; x++) {<br>
　　　　　　　if(matrix.get(x, y)){    　<br>
　　　　　　　　　pixels[y * width + x] = 0xff000000;    <br>
　　　　　　　} else {  　 <br>
　　　　　　　　　pixels[y * width + x] = 0xffffffff;   <br>
　　　　　　　}    　　<br>　　　 
 　                   <br>
　　　　　　　}    　<br>
　　　　　 }    <br>
            
　　　　Bitmap bitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);    <br>　
　　　　//通过像素数组生成bitmap,具体参考api    　<br>
　　　　bitmap.setPixels(pixels, 0, width, 0, 0, width, height);    <br>
　　　　return bitmap;    　<br>
　　　}  　<br>

## 2017/06/01

(1) 事件收集之后最先传递给 Activity， 然后依次向下传递，大致如下：<br>
Activity －> PhoneWindow －> DecorView －> ViewGroup －> ... －> View<br>
(2)	ExpandListView<br>
## 2017/06/07
(1) Matrix matrix = new Matrix();<br>
// 各种操作，旋转，缩放，错切等，可以执行多次。<br>
matrix.postTranslate(pivotX,pivotY);<br>
matrix.preTranslate(-pivotX, -pivotY);<br>
（2）触摸点坐标和画布坐标转换。<br>
![](https://github.com/838514984/dairy/blob/master/image/QQ%E6%88%AA%E5%9B%BE20170607175139.png)


## 2017/06/14
ViewGragHelper中如果子view可点击的，需要重写getViewHorizontalDragRange和getViewVerticalDragRange返回的值大于0，才能拖动。<br>
## 2017/06/28
将moudle做成其他moudle的依赖，open module seetings，中添加moudle依赖。<br>
然后在moudle依赖的gradle中修改一下配置。<br>
apply plugin:’com.android.application’<br>
改为：<br>
apply plugin: 'com.android.library'<br>
并且去掉ApplicationId.<br>
<br>**singleInstance启动模式！！！**<br>
当FirstActivity启动SecondActivity（singleinstance），在启动到firstActivity，此时FirstActivity又会到原来的栈中，此时若按下返回键，退回的界面不是Seconedactivity，而是FirstActivity！！<br>
最后一个界面才是Seconedactivity。<br>
![](https://github.com/838514984/dairy/blob/master/image/singleinstance.png)<br>
<br>**缩小Apk体积**<br>
1.在发行版构建配置（release）中添加minifyEnabled属性为true，启用proguard。<br>
2.在发行版构建配置（release）中，启用shrinkResources，删除不使用的文件。<br>
3.通过在“resConfigs”中自定义设置所需要的语言配置资源。<br>
4.将所有图像转换为webp或vector drawables（矢量图）。<br>


## 2017/7/12
AnimationDrawable<br>
1.	ImageView rocketImage = (ImageView) findViewById(R.id.rocket_image);  <br>
2.	rocketImage.setBackgroundResource(R.drawable.rocket_thrust); //roket_trust为定义的XML文件 <br> 
3.	rocketAnimation = (AnimationDrawable) rocketImage.getBackground();  <br>
然后可以start(),stop()<br>

## 2017/7/17
clipToPadding	flase,可以设置一个控件内容是否显示在padding距离中<br>
例如： <br>
　　Listview设置了PaddingTop100dp，那刚开始的第一个item是距离顶部100dp的，然后设置clipToPadding为false。那么向上滑的时候，item会填充到padding的区域中，如果为true，那么还是距离paddingtop100dp

## 2017/7/28
我了个大草，大坑<Br>
用RelativeLayout，当子view设置centerinparent true的时候，relativelayout设置paddingTOp是无效的！！！！！ 还是居中，只不过是平均吧padding分散到了top和bottom。<Br>
这个是用statusBarUtil，setPaddingSmart中发现，纠结了两天，后来换了framelayout就没问题了。坑
  
## 2017/8/1
又一个坑爹货！！！！！<br>
Camera预览的时候，黑屏<br>
在调用camera preview的时候延迟它个一点延迟就解决了，又TM是个大坑。<br>

