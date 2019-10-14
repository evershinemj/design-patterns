# definition

装饰（Decorator）模式的定义：指在不改变现有对象结构的情况下，动态地给该对象增加一些职责（即增加其额外功能）的模式，它属于对象结构型模式。

装饰（Decorator）模式的主要优点有：

1. 采用装饰模式扩展对象的功能比采用继承方式更加灵活。

2. 可以设计出多个不同的具体装饰类，创造出多个不同行为的组合。

其主要缺点是：装饰模式增加了许多子类，如果过度使用会使程序变得很复杂。

# analysis

通常情况下，扩展一个类的功能会使用继承方式来实现。但继承具有静态特征，耦合度高，并且随着扩展功能的增多，子类会很膨胀。如果使用组合关系来创建一个包装对象（即装饰对象）来包裹真实对象，并在保持真实对象的类结构不变的前提下，为其提供额外的功能，这就是装饰模式的目标。

# roles

1. 抽象构件（Component）角色：定义一个抽象接口以规范准备接收附加责任的对象。

2. 具体构件（Concrete Component）角色：实现抽象构件，通过装饰角色为其添加一些职责。

3. 抽象装饰（Decorator）角色：继承抽象构件，并包含具体构件的实例，可以通过其子类扩展具体构件的功能。

4. 具体装饰（ConcreteDecorator）角色：实现抽象装饰的相关方法，并给具体构件对象添加附加的责任。

![decorator](C:\Users\wangxueming\Pictures\java\decorator.gif)

# code

```
package decorator;
public class DecoratorPattern
{
    public static void main(String[] args)
    {
        Component p=new ConcreteComponent();
        p.operation();
        System.out.println("---------------------------------");
        Component d=new ConcreteDecorator(p);
        d.operation();
    }
}
//抽象构件角色
interface  Component
{
    public void operation();
}
//具体构件角色
class ConcreteComponent implements Component
{
    public ConcreteComponent()
    {
        System.out.println("创建具体构件角色");       
    }   
    public void operation()
    {
        System.out.println("调用具体构件角色的方法operation()");           
    }
}
//抽象装饰角色
class Decorator implements Component
{
    private Component component;   
    public Decorator(Component component)
    {
        this.component=component;
    }   
    public void operation()
    {
        component.operation();
    }
}
//具体装饰角色
class ConcreteDecorator extends Decorator
{
    public ConcreteDecorator(Component component)
    {
        super(component);
    }   
    public void operation()
    {
        super.operation();
        addedFunction();
    }
    public void addedFunction()
    {
        System.out.println("为具体构件角色增加额外的功能addedFunction()");           
    }
}
```

# example

用装饰模式实现游戏角色“莫莉卡·安斯兰”的变身。

分析：在《恶魔战士》中，游戏角色“莫莉卡·安斯兰”的原身是一个可爱少女，但当她变身时，会变成头顶及背部延伸出蝙蝠状飞翼的女妖，当然她还可以变为穿着漂亮外衣的少女。这些都可用装饰模式来实现，在本实例中的“莫莉卡”原身有 setImage(String t) 方法决定其显示方式，而其 变身“蝙蝠状女妖”和“着装少女”可以用 setChanger() 方法来改变其外观，原身与变身后的效果用 display() 方法来显示（点此下载其原身和变身后的图片），图 2 所示是其结构图。

![Morrigan](C:\Users\wangxueming\Pictures\java\Morrigan.gif)

```
package decorator;
import java.awt.*;
import javax.swing.*;
public class MorriganAensland
{
    public static void main(String[] args)
    {
        Morrigan m0=new original();
        m0.display();
        Morrigan m1=new Succubus(m0);
        m1.display();
        Morrigan m2=new Girl(m0);
        m2.display();
    }
}
//抽象构件角色：莫莉卡
interface  Morrigan
{
    public void display();
}
//具体构件角色：原身
class original extends JFrame implements Morrigan
{
    private static final long serialVersionUID = 1L;
    private String t="Morrigan0.jpg";
    public original()
    {
        super("《恶魔战士》中的莫莉卡·安斯兰");                
    }
    public void setImage(String t)
    {
        this.t=t;           
    }
    public void display()
    {   
        this.setLayout(new FlowLayout());
        JLabel l1=new JLabel(new ImageIcon("src/decorator/"+t));
        this.add(l1);   
        this.pack();       
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);  
        this.setVisible(true);
    }
}
//抽象装饰角色：变形
class Changer implements Morrigan
{
    Morrigan m;   
    public Changer(Morrigan m)
    {
        this.m=m;
    }   
    public void display()
    {
        m.display();
    }
}
//具体装饰角色：女妖
class Succubus extends Changer
{
    public Succubus(Morrigan m)
    {
        super(m);
    }   
    public void display()
    {
        setChanger();
        super.display();   
    }
    public void setChanger()
    {
        ((original) super.m).setImage("Morrigan1.jpg");           
    }
}
//具体装饰角色：少女
class Girl extends Changer
{
    public Girl(Morrigan m)
    {
        super(m);
    }   
    public void display()
    {
        setChanger();
        super.display();   
    }
    public void setChanger()
    {
        ((original) super.m).setImage("Morrigan2.jpg");           
    }
}
```