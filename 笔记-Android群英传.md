1. 大色块使用

2. Material Design 
	主题

3. Palette
	提取颜色
	依赖：``com.android.support:palette-v7:21.0.2``

4. elevation
	阴影
		xml-->elevation

5. Tinting
	着色
		xml-->tint/tintMode

6. Clipping
	裁剪
		java-->
	        ViewOutlineProvider vopCircle = new ViewOutlineProvider() {
	            @Override
	            public void getOutline(View view, Outline outline) {
	                outline.setOval(0, 0, view.getWidth(), view.getHeight());
	            }
	        };
	        cicle.setOutlineProvider(vopCircle);
		注意：背景会覆盖裁剪

7. RecyclerView
	列表
	依赖：``com.android.support:recyclerview-v7:21.0.2``
	RecyclerView.Adapter

8. CardView
	卡片
	依赖：``com.android.support:cardview-v7:23.0.1``
		xml-->
			(1) xmlns:cardview="http://schemas.android.com/apk/res-auto"
        	(2) cardview:cardBackgroundColor="#aaf"
       		(3) cardview:cardCornerRadius="8dp"

9. 过渡动画
	Explode/Slide/Fade
	ActivityA:
		java-->startActivity(intent, ActivityOptions.makeSceneTransitionAnimation(this).toBundle());
	ActivityB:
		java-->
		getWindow().requestFeature(Window.FEATURE_CONTENT_TRANSITIONS);
        getWindow().setEnterTransition(new Explode());
        getWindow().setExitTransition(new Slide());

10. 共享元素
	ActivityA:
		java-->
		startActivity(intent, ActivityOptions.makeSceneTransitionAnimation(this, Pair.create(view,"fab")).toBundle());
		startActivity(intent, ActivityOptions.makeSceneTransitionAnimation(this, view,"fab").toBundle());
	XMLB-->
		android:transitionName="fab"

11. Ripple
	波纹效果
		有界：android:backgroud="?android:attr/selectableItemBackgroud"
		无界：android:backgroud="?android:attr/selectableItemBackgroudBorderless"
	XML-->
		<ripple 
			xmlns:android="http://schemas.android.com/apk/res/android"
		    android:color="#faa">
		    <!--可省略-->
			<item>
			    <shape android:shape="oval">
			        <solid android:color="#aaf"></solid>
			    </shape>
			</item>
		</ripple>

12. Circular Reveal
	圆形展示
        final Animator animator;
        imageView.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                if(animator!=null && animator.isRunning()){
                    return false;
                }
                animator = ViewAnimationUtils.createCircularReveal(imageView, (int) event.getX(),(int) event.getY(), 0, (float) Math.hypot(imageView.getWidth(), imageView.getHeight()));
                animator.setInterpolator(new AccelerateDecelerateInterpolator());
                animator.setDuration(1000);
                animator.start();
                return false;
            }
        });

13. StateListAnimator
	java-->
		imageView.setStateListAnimator(AnimatorInflater.loadStateListAnimator(this,R.drawable.state_selector));
	state_selector.XML-->
	    <ImageButton
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:src="@mipmap/ic_launcher"
	        android:stateListAnimator="@drawable/state_selector"/>
	selector.XML-->
		<selector xmlns:android="http://schemas.android.com/apk/res/android">
		    <item android:state_pressed="true">
		        <set>
		            <objectAnimator android:propertyName="rotationY"
		                android:duration="1000"
		                android:valueTo="30"
		                android:pivotY="0"
		                android:valueType="floatType"/>
		        </set>
		    </item>
		    <item>
		        <set>
		            <objectAnimator android:propertyName="rotationY"
		                android:duration="1000"
		                android:valueTo="0"
		                android:pivotY="0"
		                android:valueType="floatType"/>
		        </set>
		    </item>
		</selector>

14. animated-selector
		5.X新特性
		-->P290
			无法复现

15. Toolbar
		代替ActionBar
		compile 'com.android.support:appcompat-v7:21.0.3'
		-->P294
			无法复现================================================
				java-->
	        toolbar= (Toolbar) findViewById(android.R.id.toolbar);
	        toolbar.setLogo(R.mipmap.ic_launcher);
	        toolbar.setTitle("主标题");
	        toolbar.setSubtitle("副标题");
	        setSupportActionBar(toolbar);
	        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
	        mDrawerLayout= (DrawerLayout) findViewById(R.id.drawer);
	        mDrawerToggle = new ActionBarDrawerToggle(this,mDrawerLayout, toolbar, R.string.abc_action_bar_home_description,R.string.abc_action_bar_home_description_format);
	        mDrawerToggle.syncState();
	        mDrawerLayout.setDrawerListener(mDrawerToggle);
        style.XML-->
	        <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
		        <item name="colorPrimary">#4876FF</item>
		        <item name="colorPrimaryDark">#3A5FCD</item>
		        <item name="android:windowBackground">@android:color/white</item>
		        <item name="android:searchViewStyle">@style/MySearchView</item>
			    </style>
			    <style name="MySearchView" parent="Widget.AppCompat.SearchView"></style>

16. DrawerLayout
		侧滑菜单-->
	    <android.support.v4.widget.DrawerLayout
	        android:id="@+id/drawer"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        >
	        <LinearLayout
	            android:layout_width="match_parent"
	            android:layout_height="match_parent"
	            android:background="@android:color/holo_blue_light"
	            android:orientation="vertical">
	            <Button
	                android:layout_width="match_parent"
	                android:layout_height="wrap_content"
	                android:text="内容界面"/>
	        </LinearLayout>
	        <ScrollView
	            android:layout_width="match_parent"
	            android:layout_height="match_parent"
	            android:layout_gravity="start"
	            >
	            <LinearLayout
	                android:layout_width="wrap_content"
	                android:layout_height="match_parent"
	                android:layout_gravity="start"
	                android:background="@android:color/holo_blue_light"
	                android:orientation="vertical">
	                <Button
	                    android:layout_width="200dp"
	                    android:layout_height="wrap_content"
	                    android:text="菜单界面_1"/>
	            </LinearLayout>
	        </ScrollView>
	        <ScrollView
	            android:layout_width="match_parent"
	            android:layout_height="match_parent"
	            android:layout_gravity="end"
	            >
	            <LinearLayout
	                android:layout_width="wrap_content"
	                android:layout_height="match_parent"
	                android:layout_gravity="end"
	                android:background="@android:color/holo_blue_light"
	                android:orientation="vertical">
	                <Button
	                    android:layout_width="200dp"
	                    android:layout_height="wrap_content"
	                    android:text="菜单界面_2"/>
	            </LinearLayout>
	        </ScrollView>
	    </android.support.v4.widget.DrawerLayout>

17. Notification
通知
折叠式
		notification.contentIntent = RemoteViews;
展开式
		notification.bigContentView = RemoteViews;
悬挂式
		builder.setFullScreenIntent(pendingIntent, true);
等级式
		builder.setVisibility(Notification.VISIBILITY_PUBLIC);

18. Layer
		图层
			XML-->
				<?xml version="1.0" encoding="utf-8"?>
				<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
				    <item android:drawable="@drawable/ic_launcher"></item>
				    <item android:drawable="@drawable/ic_launcher"
				        android:left="20dip"
				        android:right="20dip"
				        android:bottom="20dip"
				        android:top="20dip"></item>
				</layer-list>
			Java-->
        Paint p =new Paint();
        p.setColor(Color.RED);
        canvas.drawCircle(getWidth()/3,getHeight()/3,getWidth()/2,p);

        canvas.saveLayerAlpha(0, 0 , getWidth() ,getHeight(),127);	<----
        p.setColor(Color.GREEN);
        canvas.drawCircle(getWidth()*2/3,getHeight()*2/3,getWidth()/2,p);
        canvas.restore();
			
19. Bitmap
		XML-->
			<?xml version="1.0" encoding="utf-8"?>
			<bitmap xmlns:android="http://schemas.android.com/apk/res/android"
			    android:src="@drawable/ic_launcher">
			</bitmap>

20. ViewStub
可以在XML中插入<ViewStub />标签，在初次渲染时不会填充这个控件
只能调用一次inflate()方法，因为调用一次后，viewStub就变为了指定的控件

XML
```
	<ViewStub
		android:id="@+id/view_stub"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:layout="@layout/activity_main"/>
```
java
```
ViewStub viewStub = (ViewStub) findViewById(R.id.view_stub);
//viewStub.setVisibility(View.VISIBLE);
LinearLayout ll = (LinearLayout) viewStub.inflate();
```

***
## 色彩特效
### 色彩矩阵分析
* 色调--物体传播的颜色
* 饱和度--颜色的纯度，从0（灰）到100%（饱和）来进行描述
* 亮度--颜色的相对明暗程度

### 颜色矩阵
ColorMatrix 4X5数字矩阵
1. 处理色调                                                 
		cm.setRotate()
2. 处理饱和度
        cm.setSaturation()
3. 处理亮度
        cm.setScale();
#### 例
```
Paint p =new Paint();
Bitmap bm = BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher);
Bitmap bmp = Bitmap.createBitmap(bm);
ColorMatrix cm = new ColorMatrix();
//指定颜色矩阵数值4X5，灰度效果
float[] matrix = {
        0.33f,0.59f,0.11f,0,0,
        0.33f,0.59f,0.11f,0,0,
        0.33f,0.59f,0.11f,0,0,
        0,    0,    0,    1,0
};
cm.set(matrix);
ColorMatrix cm2 = new ColorMatrix();
cm2.setScale(1f,1f,0f,1);
cm.postConcat(cm2);
p.setColorFilter(new ColorMatrixColorFilter(cm));
canvas.drawBitmap(bmp,0,0,p);
```

## 图形特效
### 变形矩阵
Matrix 3X3数字矩阵

前乘和后乘是不同的运算，影响变换的顺序
```
m.postXXX();
m.preXXX();
```
#### 图形变换
1. 平移变换
```
m.setTranslate();
```
```
| 1 0 Δx |
| 0 1 Δy |
| 0 0  1 |
```
2. 旋转变换
```
m.setRotate();
```
```
| cosα -sinα  0 |
| sinα  cosα  0 |
|    0     0  1 |
```
3. 缩放变换
```
m.setScale();
```
```
| k1  0  0 |
|  0 k2  0 |
|  0  0  1 |
```
4. 错切变换
```
m.setSkew();
```
```
|  1 k1  0 |
| k2  1  0 |
|  0  0  1 |
```

#### 像素块分析
	/**
	 * WIDTH 像素块的横向个数 
	 * HEIGHT 像素块的纵向个数 
	 * verts 分割线的交叉点坐标数组，包括与边界的交叉点，长度为 （WIDTH + 1）*（HEIGHT + 1）*2
	 * /
	canvas.drawBitmapMesh(bitmap,WIDTH,HEIGHT,verts,0,null,0,null);
#### 例
```
    public MyTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }
    private final int WIDTH = 200;
    private final int HEIGHT = 200;
    private Bitmap bm = BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher);
    private float mWidth;
    private float mHeight;
    private float[] orig = new float[(WIDTH+1)*(HEIGHT+1)*2];
    private float[] verts = new float[(WIDTH+1)*(HEIGHT+1)*2];
    private void init(){
        mWidth=bm.getWidth();
        mHeight=bm.getHeight();
        float dx = mWidth/WIDTH;
        float dy = mHeight/HEIGHT;
        for (int y=0;y<=HEIGHT;y++){
            for (int x=0;x<=WIDTH;x++){
                int index = (WIDTH+1)*y+x;
                orig[index*2]=verts[index*2]=dx*x;
                orig[index*2+1]=verts[index*2+1]=dy*y+100;
            }
        }
    }
    @Override
    protected void onDraw(Canvas canvas) {
        wave();
        canvas.drawBitmapMesh(bm,WIDTH,HEIGHT,verts,0,null,0,null);
        super.onDraw(canvas);
        invalidate();
    }
    private double k;
    private final int A = 10;
    private void wave(){
        for (int y=0;y<=HEIGHT;y++){
            for (int x=0;x<=WIDTH;x++){
                int index = (WIDTH+1)*y+x;
                verts[index*2] = orig[index*2]+getMeasuredWidth()/2-bm.getWidth()/2;
                float dy = (float) Math.sin(2*Math.PI/WIDTH*x + Math.PI*k);
                verts[index*2+1]=orig[index*2+1]+dy*A;
            }
        }
        k+=0.1f;
    }
```

## 画笔特效（极其重要）
### PorterDuffXfermode
遮盖效果
```
mPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DST_IN));
```
更好的效果可以使用贝塞尔曲线，当前只使用圆滑效果
```
mPaint.setStyle(Paint.Style.STROKE);
mPaint.setStrokeJoin(Paint.Join.ROUND);
mPaint.setStrokeWidth(100);
mPaint.setStrokeCap(Paint.Cap.ROUND);
mPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DST_IN));
```
***注意***：
* 最好关闭在绘图时关闭硬件加速，有些模式不支持硬件加速

#### 例：
```
private void init(){
	//低层的就是dst，上层的就是src
    src = BitmapFactory.decodeResource(getResources(), R.drawable.ic_launcher);
    dst =Bitmap.createBitmap(src.getWidth(),src.getHeight(), Bitmap.Config.ARGB_8888);
    mCanvas = new Canvas(dst);
    mPaint = new Paint();
    mPaint.setColor(Color.RED);
    mCanvas.drawCircle(src.getWidth()/2,src.getHeight()/2,src.getHeight()/3,mPaint);
    mPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));
    mCanvas.drawBitmap(src,0,0,mPaint);
    mPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DST_OUT));
    mCanvas.drawCircle(src.getWidth()/2,src.getHeight()/2,src.getHeight()/10,mPaint);
}

@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    canvas.drawBitmap(dst,0,0,null);
}
```

### Shader
着色器，渲染器
实现一系列的渐变和渲染效果

**Shader**
1. BitmapShader--位图Shader
2. LinearGradient--线性Shader
3. RadialGradient--光束Shader
4. SweepGradient--梯度Shader
5. ComposeShader--混合Shader

#### 例--LinearGradient：
```
    @Override
    protected void onDraw(Canvas canvas) {
        mPaint.setShader(new LinearGradient(0,0,getMeasuredWidth(),0,Color.RED,Color.GREEN, Shader.TileMode.MIRROR));
        canvas.drawRect(0,0,getMeasuredWidth(),getMeasuredHeight(),mPaint);
        super.onDraw(canvas);
    }
```
#### 例--PorterXfermode和Shader混合使用：
```
private void init(){
    src = BitmapFactory.decodeResource(getResources(), R.drawable.ic_launcher);
    Matrix matrix = new Matrix();
    matrix.setScale(1f,-1f);
    dust = Bitmap.createBitmap(src,0,0,src.getWidth(),src.getHeight(),matrix,true);
    mPaint = new Paint();
    mPaint.setShader(new LinearGradient(0,src.getHeight(),0,src.getHeight()*2,0xFF000000,0xffffffff, Shader.TileMode.CLAMP));
}

@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    int saveCount = canvas.saveLayer(0,0,getMeasuredWidth(),getMeasuredHeight(),null,Canvas.ALL_SAVE_FLAG & (~Canvas.CLIP_SAVE_FLAG));
    canvas.drawBitmap(src,0,0,null);
    canvas.drawBitmap(dust,0,src.getHeight(),null);
    mPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));
    canvas.drawRect(0,src.getHeight(),src.getHeight(),src.getHeight()*2,mPaint);
    mPaint.setXfermode(null);
    canvas.restoreToCount(saveCount);
```

### PathEffect
路径效果
1. null
无效果

2. CornerPathEffect
将拐角变得圆滑

3. DisctetePathEffect
线段上有很多的杂点

4. DashPathEffect
绘制虚线，可以控制每个点的间隔

5. PathDashPathEffect
DashPathEffect增强版，可以设置点的样式，矩形，圆点等

6. ComposePathEffect
组合方式

#### 例：
```
public class MyPathView extends View{
    private Path mPath;
    private PathEffect[] mPathEffects = new PathEffect[6];
    private Paint mPaint;
    public MyPathView(Context context, AttributeSet attrs) {
        super(context, attrs);
        mPath = new Path();
        mPath.moveTo(0,200);
        for(int i=1;i<50;i++){
            mPath.lineTo(i*20, (float) (Math.random()*100)+200);
        }
        mPaint = new Paint();
        mPaint.setColor(Color.parseColor("#000000"));
        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setStrokeWidth(5);
        mPathEffects[0] = null;
        mPathEffects[1] = new CornerPathEffect(30);
        mPathEffects[2] = new DiscretePathEffect(3f,5f);
        mPathEffects[3] = new DashPathEffect(new float[]{50f,10f,5f},0);
        Path path = new Path();
        path.addRect(0,0,8,8,Path.Direction.CCW);
        mPathEffects[4] = new PathDashPathEffect(path,12,0,PathDashPathEffect.Style.ROTATE);
        mPathEffects[5] = new ComposePathEffect( mPathEffects[3], mPathEffects[1]);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        canvas.save();
        for (int i = 0 ;i< mPathEffects.length;i++){
            mPaint.setPathEffect(mPathEffects[i]);
            canvas.drawPath(mPath,mPaint);
            canvas.translate(0, 200);
        }
        canvas.restore();
    }
}
```
***效果图如下***
![PathEffect](http://47.92.77.1/open/notes/raw/master/image/PathEffect.png)

## 动画
### 布局动画
LinearLayout
#### 例
```
LinearLayout ll = (LinearLayout) findViewById(R.id.ll);
ScaleAnimation sa = new ScaleAnimation(0, 1, 0, 1);
sa.setDuration(2000);
LayoutAnimationController lac = new LayoutAnimationController(sa, 0.5f);
lac.setOrder(LayoutAnimationController.ORDER_RANDOM);
ll.setLayoutAnimation(lac);
```



## SVG