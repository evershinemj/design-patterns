# url

[factory method](http://c.biancheng.net/view/1348.html)

# definition

工厂方法（FactoryMethod）模式的定义：定义一个创建产品对象的工厂接口，将产品对象的实际创建工作推迟到具体子工厂类当中。这满足创建型模式中所要求的“创建与使用相分离”的特点。

# merits

工厂方法模式的主要优点有：

- 用户只需要知道具体工厂的名称就可得到所要的产品，无须知道产品的具体创建过程；

- 在系统增加新的产品时只需要添加具体产品类和对应的具体工厂类，无须对原工厂进行任何修改，满足开闭原则；

# drawbacks

其缺点是：每增加一个产品就要增加一个具体产品类和一个对应的具体工厂类，这增加了系统的复杂度。

# roles

工厂方法模式的主要角色如下。

1. 抽象工厂（Abstract Factory）：提供了创建产品的接口，调用者通过它访问具体工厂的工厂方法 newProduct() 来创建产品。

2. 具体工厂（ConcreteFactory）：主要是实现抽象工厂中的抽象方法，完成具体产品的创建。

3. 抽象产品（Product）：定义了产品的规范，描述了产品的主要特性和功能。

4. 具体产品（ConcreteProduct）：实现了抽象产品角色所定义的接口，由具体工厂来创建，它同具体工厂之间一一对应。

![FactoryMethod diagram](C:\Users\wangxueming\Pictures\java\FactoryMethod.gif)
---
![AnimalFarm diagram](C:\Users\wangxueming\Pictures\java\AnimalFarm.gif)

# code

## FactoryMethod

```
package FactoryMethod;
public class AbstractFactoryTest
{
    public static void main(String[] args)
    {
        try
        {
            Product a;
            AbstractFactory af;
            af=(AbstractFactory) ReadXML1.getObject();
            a=af.newProduct();
            a.show();
        }
        catch(Exception e)
        {
            System.out.println(e.getMessage());
        }
    }
}
//抽象产品：提供了产品的接口
interface Product
{
    public void show();
}
//具体产品1：实现抽象产品中的抽象方法
class ConcreteProduct1 implements Product
{
    public void show()
    {
        System.out.println("具体产品1显示...");
    }
}
//具体产品2：实现抽象产品中的抽象方法
class ConcreteProduct2 implements Product
{
    public void show()
    {
        System.out.println("具体产品2显示...");
    }
}
//抽象工厂：提供了厂品的生成方法
interface AbstractFactory
{
    public Product newProduct();
}
//具体工厂1：实现了厂品的生成方法
class ConcreteFactory1 implements AbstractFactory
{
    public Product newProduct()
    {
        System.out.println("具体工厂1生成-->具体产品1...");
        return new ConcreteProduct1();
    }
}
//具体工厂2：实现了厂品的生成方法
class ConcreteFactory2 implements AbstractFactory
{
    public Product newProduct()
    {
        System.out.println("具体工厂2生成-->具体产品2...");
        return new ConcreteProduct2();
    }
}
```

```
package FactoryMethod;
import javax.xml.parsers.*;
import org.w3c.dom.*;
import java.io.*;
class ReadXML1
{
    //该方法用于从XML配置文件中提取具体类类名，并返回一个实例对象
    public static Object getObject()
    {
        try
        {
            //创建文档对象
            DocumentBuilderFactory dFactory=DocumentBuilderFactory.newInstance();
            DocumentBuilder builder=dFactory.newDocumentBuilder();
            Document doc;                           
            doc=builder.parse(new File("src/FactoryMethod/config1.xml"));        
            //获取包含类名的文本节点
            NodeList nl=doc.getElementsByTagName("className");
            Node classNode=nl.item(0).getFirstChild();
            String cName="FactoryMethod."+classNode.getNodeValue();
            //System.out.println("新类名："+cName);
            //通过类名生成实例对象并将其返回
            Class<?> c=Class.forName(cName);
              Object obj=c.newInstance();
            return obj;
         }  
         catch(Exception e)
         {
                   e.printStackTrace();
                   return null;
         }
    }
}
```

```
<?xml version="1.0" encoding="UTF-8"?>
<config>
	<className>ConcreteFactory1</className>
</config>
```

# AnimalFarm

```
package FactoryMethod;
import java.awt.*;
import javax.swing.*;
public class AnimalFarmTest
{
    public static void main(String[] args)
    {
        try
        {
            Animal a;
            AnimalFarm af;
            af=(AnimalFarm) ReadXML2.getObject();
            a=af.newAnimal();
            a.show();
        }
        catch(Exception e)
        {
            System.out.println(e.getMessage());
        }
    }
}
//抽象产品：动物类
interface Animal
{
    public void show();
}
//具体产品：马类
class Horse implements Animal
{
    JScrollPane sp;
    JFrame jf=new JFrame("工厂方法模式测试");
    public Horse()
    {
        Container contentPane=jf.getContentPane();
        JPanel p1=new JPanel();
        p1.setLayout(new GridLayout(1,1));
        p1.setBorder(BorderFactory.createTitledBorder("动物：马"));
        sp=new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1=new JLabel(new ImageIcon("src/A_Horse.jpg"));
        p1.add(l1);       
        jf.pack();       
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);    //用户点击窗口关闭 
    }
    public void show()
    {
        jf.setVisible(true);
    }
}
//具体产品：牛类
class Cattle implements Animal
{
    JScrollPane sp;
    JFrame jf=new JFrame("工厂方法模式测试");
    public Cattle()
    {
        Container contentPane=jf.getContentPane();
        JPanel p1=new JPanel();
        p1.setLayout(new GridLayout(1,1));
        p1.setBorder(BorderFactory.createTitledBorder("动物：牛"));
        sp=new JScrollPane(p1);
        contentPane.add(sp,BorderLayout.CENTER);
        JLabel l1=new JLabel(new ImageIcon("src/A_Cattle.jpg"));
        p1.add(l1);       
        jf.pack();       
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);    //用户点击窗口关闭 
    }
    public void show()
    {
        jf.setVisible(true);
    }
}
//抽象工厂：畜牧场
interface AnimalFarm
{
    public Animal newAnimal();
}
//具体工厂：养马场
class HorseFarm implements AnimalFarm
{
    public Animal newAnimal()
    {
        System.out.println("新马出生！");
        return new Horse();
    }
}
//具体工厂：养牛场
class CattleFarm implements AnimalFarm
{
    public Animal newAnimal()
    {
        System.out.println("新牛出生！");
        return new Cattle();
    }
}
```

```
package FactoryMethod;
import javax.xml.parsers.*;
import org.w3c.dom.*;
import java.io.*;
class ReadXML2
{
    public static Object getObject()
    {
        try
        {
            DocumentBuilderFactory dFactory=DocumentBuilderFactory.newInstance();
            DocumentBuilder builder=dFactory.newDocumentBuilder();
            Document doc;                           
            doc=builder.parse(new File("src/FactoryMethod/config2.xml"));
            NodeList nl=doc.getElementsByTagName("className");
            Node classNode=nl.item(0).getFirstChild();
            String cName="FactoryMethod."+classNode.getNodeValue();
            System.out.println("新类名："+cName);
            Class<?> c=Class.forName(cName);
              Object obj=c.newInstance();
            return obj;
        }  
        catch(Exception e)
        {
               e.printStackTrace();
               return null;
        }
    }
}
```

```
<?xml version="1.0" encoding="UTF-8"?>
<config>
	<className>HorseFarm</className>
</config>
```