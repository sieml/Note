## View 事件
    http://blog.csdn.net/windskier/article/details/6966264
    http://blog.csdn.net/android_jiangjun/article/details/45798221
    http://blog.csdn.net/innost/article/details/47660193
    http://www.tuicool.com/articles/MjAjIfU
    (http://blog.csdn.net/nugongahou110/article/details/49662211)DecorView继承于FrameLayout，然后它有一个子view即LinearLayout，方向为竖直方向，其内有两个FrameLayout，上面的FrameLayout即为TitleBar之类的，下面的FrameLayout即为我们的ContentView，所谓的setContentView就是往这个FrameLayout里面添加我们的布局View的！现在我们可以画出第二层了！

    事件由硬件感应,会被Android系统(基于Linux)存储在/dev/input/各个目录下(event0,event1..),然后由WindowManagerService捕获
    用户输入的事件(按键/触摸),WMS通过共享内存和管道的方式传递给ViewRoot(ViewRoot extends
    Handler),ViewRoot再dispatch给Application的View.
    当有事件从硬件设备输入时,system_service端检测到事件发生时,通过管道通知ViewRoot事件发生,此时ViewRoot再去内存中读取这个事件信息.
    WindowManagerService->ViewRoot(ViewRoot extends Handler implements
    ViewParent)->Activity(->PhoneWindow直接执行的是mDecor的分发分发)->DecorView(PhoneWindow的子类);另:(WindowManager为接口,管理PhoneWindow)
    View事件处理:
    WindowManagerService->Activity->DecorView(PhoneWindow的子类)->ViewGroup->View
    简述为:
    Activity->ViewGroup->View

    1. 事件截断处理的View: onInterceptTouchEvent() return true;(表示不再向子View传递事件,否则传递给子View[当其子View
    的dispatchTouchEvent()返回为false则继续往上])
    2. 事件分发处理的View: dispatchTouchEvent() return true;(表示当前View要处理[返回true是告诉父View被 我/我的子View
    处理了,处理的过程是 我/我的子View 调用onTouchEvent()而且返回true处理的],反之则自己不处理分发给子View)
    3. 事件消费处理的View: onTouchEvent() return true;(表示事件的终点,否则则不是终点.注:
    如果都不处理最后会由Activity返回false作为最后一个调用者,不可点击的View只能返回false)

    代码流程:
    if(ViewGroup.onInterceptTouchEvent()==false ) {View.dispatchTouchEvent()}
    if(View.onTouchEvent()==true )
    {View.dispatchTouchEvent()->ViewGroup.dispatchTouchEvent()->Activity.dispatchTouchEvent()}
    if(View.onTouchEvent()==false ) {ViewGroup.onTouchEvent()->Activity.onTouchEvent()}
    if(ViewGroup.onInterceptTouchEvent()==true )
    {ViewGroup.onTouchEvent()}->只进行第一次判断之后下次不会调用ViewGroup.onInterceptTouchEvent(),下次而是直接调用ViewGroup.onTouchEvent()
    if(ViewGroup.onTouchEvent()==true ){if(View.onTouchEvent()==true and action == ACTION_DOWN) {if(某一刻
    ViewGroup.onInterceptTouchEvent()==true)那么下次子View就会收到一个ACTION_CANCEL} }