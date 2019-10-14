# url

[bridge](http://c.biancheng.net/view/1364.html)

# personal note

以组合代替继承

# intro

在现实生活中，某些类具有两个或多个维度的变化，如图形既可按形状分，又可按颜色分。如何设计类似于 Photoshop 这样的软件，能画不同形状和不同颜色的图形呢？如果用继承方式，m 种形状和 n 种颜色的图形就有 m×n 种，不但对应的子类很多，而且扩展困难。

当然，这样的例子还有很多，如不同颜色和字体的文字、不同品牌和功率的汽车、不同性别和职业的男女、支持不同平台和不同文件格式的媒体播放器等。如果用桥接模式就能很好地解决这些问题。

# definition

桥接（Bridge）模式的定义如下：将抽象与实现分离，使它们可以独立变化。它是用组合关系代替继承关系来实现，从而降低了抽象和实现这两个可变维度的耦合度。

# roles

桥接（Bridge）模式包含以下主要角色。

1. 抽象化（Abstraction）角色：定义抽象类，并包含一个对实现化对象的引用。
2. 扩展抽象化（Refined    Abstraction）角色：是抽象化角色的子类，实现父类中的业务方法，并通过组合关系调用实现化角色中的业务方法。
3. 实现化（Implementor）角色：定义实现化角色的接口，供扩展抽象化角色调用。
4. 具体实现化（Concrete Implementor）角色：给出实现化角色接口的具体实现。

![bridge](C:\Users\wangxueming\Pictures\java\bridge.gif)

# example

用桥接（Bridge）模式模拟女士皮包的选购。

分析：女士皮包有很多种，可以按用途分、按皮质分、按品牌分、按颜色分、按大小分等，存在多个维度的变化，所以采用桥接模式来实现女士皮包的选购比较合适。

本实例按用途分可选钱包（Wallet）和挎包（HandBag），按颜色分可选黄色（Yellow）和红色（Red）。可以按两个维度定义为颜色类和包类。（点此下载本实例所要显示的包的图片）。

颜色类（Color）是一个维度，定义为实现化角色，它有两个具体实现化角色：黄色和红色，通过 getColor() 方法可以选择颜色；包类（Bag）是另一个维度，定义为抽象化角色，它有两个扩展抽象化角色：挎包和钱包，它包含了颜色类对象，通过 getName() 方法可以选择相关颜色的挎包和钱包。

客户类通过 ReadXML 类从 XML 配置文件中获取包信息（点此下载 XML 配置文件），并把选到的产品通过窗体显示出现，图 2 所示是其结构图。

![Bag](C:\Users\wangxueming\Pictures\java\Bag.gif)

# code

```
package bridge;
import java.awt.*;
import javax.swing.*;
public class BagManage
{
    public static void main(String[] args)
    {
        Color color;
        Bag bag;
        color=(Color)ReadXML.getObject("color");
        bag=(Bag)ReadXML.getObject("bag");
        bag.setColor(color);
        String name=bag.getName();
        show(name);
    }
    public static void show(String name)
    {
        JFrame jf=new JFrame("桥接模式测试");
        Container contentPane=jf.getContentPane();
        JPanel p=new JPanel();   
        JLabel l=new JLabel(new ImageIcon("src/bridge/"+name+".jpg"));
        p.setLayout(new GridLayout(1,1));
        p.setBorder(BorderFactory.createTitledBorder("女士皮包"));
        p.add(l);
        contentPane.add(p, BorderLayout.CENTER);
        jf.pack();  
        jf.setVisible(true);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}
//实现化角色：颜色
interface Color
{
    String getColor();
}
//具体实现化角色：黄色
class Yellow implements Color
{
    public String getColor()
    {
        return "yellow";
    }
}
//具体实现化角色：红色
class Red implements Color
{
    public String getColor()
    {
        return "red";
    }
}
//抽象化角色：包
abstract class Bag
{
    protected Color color;
    public void setColor(Color color)
    {
        this.color=color;
    }   
    public abstract String getName();
}
//扩展抽象化角色：挎包
class HandBag extends Bag
{
    public String getName()
    {
        return color.getColor()+"HandBag";
    }   
}
//扩展抽象化角色：钱包
class Wallet extends Bag
{
    public String getName()
    {
        return color.getColor()+"Wallet";
    }   
}
```

```
package bridge;
import javax.xml.parsers.*;
import org.w3c.dom.*;
import java.io.*;
class ReadXML
{
    public static Object getObject(String args)
    {
        try
        {
            DocumentBuilderFactory dFactory=DocumentBuilderFactory.newInstance();
            DocumentBuilder builder=dFactory.newDocumentBuilder();
            Document doc;                           
            doc=builder.parse(new File("src/bridge/config.xml"));
            NodeList nl=doc.getElementsByTagName("className");
            Node classNode=null;
            if(args.equals("color"))
            {
                classNode=nl.item(0).getFirstChild();
            }
            else if(args.equals("bag"))
            {
                classNode=nl.item(1).getFirstChild();
            }          
            String cName="bridge."+classNode.getNodeValue();
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
    <className>Red</className>
    <className>Wallet</className>
</config>
```    