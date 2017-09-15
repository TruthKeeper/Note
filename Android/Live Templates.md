## Live Templates

代码快速生成模板，Settings->Editor->Live Templates

在Settings->Editor->File and Code Templates可以对File Header 生成默认的注释
```
/**
 * <pre>
 *      author : TK
 *      time : ${DATE}
 *      desc : 
 * </pre>
 */
```

### 原生

- logt -> 快速补全TAG 
- loge -> 快速补全Log.e(TAG , "");
- const，psf -> 生成常量
- .field -> 快速生成成员变量

### 单例

修改变量CLASS_NAME为classname()，应用于Java > declaration 

```
private static volatile $CLASS_NAME$ m$CLASS_NAME$ = null;
private $CLASS_NAME$(){}
public static $CLASS_NAME$ getInstance() {
  if(m$CLASS_NAME$==null){
      synchronized ($CLASS_NAME$.class){
          if(m$CLASS_NAME$ == null){
              m$CLASS_NAME$ = new $CLASS_NAME$();
          }
      }
  }
    return m$CLASS_NAME$;
}
```

### 自定义控件

应用于Java > statement，declaration 

```
Paint mPaint = new Paint();
Path mPath = new Path();

private void init() {
    setLayerType(LAYER_TYPE_SOFTWARE, null);
    mPaint.setDither(true);
    mPaint.setAntiAlias(true);
    mPaint.setFilterBitmap(true);
    mPaint.setStyle(Paint.Style.FILL);
}
```

### 事件分发

应用于Java > statement，declaration 

```
    switch (event.getAction()) {
        case MotionEvent.ACTION_DOWN:
            break;
        case MotionEvent.ACTION_MOVE:
            break;
        case MotionEvent.ACTION_UP:
            break;
        case MotionEvent.ACTION_CANCEL:
            break;
        default:
            break;
    }
```