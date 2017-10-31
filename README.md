#### 这边只说功能的实现，具体观察者模式请自行去了解,因为最近在熟悉kotlin所以以下代码用kotlin书写

#### 首先创建被观察者接口

```
open interface Observer {

    fun update(obj: Object)

}
```

#### 创建观察者基类


```
open abstract class Subject {
    
    //被观察者的集合
    protected val subjectList = arrayListOf<Observer>()

    //注册被观察者，添加到集合
    abstract fun register(observer:Observer)
    //删除集合
    abstract fun unRegister(observer: Observer)
    //发送消息
    abstract fun notifyObservers(obj: Object)

}
```

#### 具体观察者实现方法,这里kotlin已经实现了单例


```
object ConcreteSubject : Subject() {
    override fun register(observer: Observer) {
        subjectList.add(observer)
    }

    override fun unRegister(observer: Observer) {
        subjectList.remove(observer)
    }
    //调用集合所有的方法并调用
    override fun notifyObservers(obj: Object) {
        for (observer in subjectList) {
            observer.update(obj)
        }
    }
}
```
#### 发送消息页面

```
class TestActivity :AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_text)
        button.setOnClickListener {
            finish()
            //发送消息
            ConcreteSubject.notifyObservers("text发送的消息" as Object)

        }

    }
}
```
#### 接收消息，注册，取消注册，这里必须取消注册 否则会出现内存泄漏

```
class MainActivity : AppCompatActivity() ,Observer{
    @RequiresApi(Build.VERSION_CODES.KITKAT)
    override fun update(obj: Object) {
        tv.setText(obj as String)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        ConcreteSubject.register(this)
        button.setOnClickListener {
            startActivity(Intent(this,TestActivity::class.java))
        }
    }

    override fun onDestroy() {
        super.onDestroy()
        ConcreteSubject.unRegister(this)
    }
}
```


```
class MainActivity : AppCompatActivity() ,Observer{
    @RequiresApi(Build.VERSION_CODES.KITKAT)
    override fun update(obj: Object) {
        tv.setText(obj as String)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        ConcreteSubject.register(this)
        button.setOnClickListener {      
            startActivity(Intent(this,TestActivity::class.java)) 
        }
    }

    override fun onDestroy() {
        super.onDestroy()
        ConcreteSubject.unRegister(this)
    }
}
```
#### [项目地址](https://github.com/AndroidCrow/Observer)
