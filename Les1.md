# Java2D
Java2D is een API om binnen java met 2D graphics te werken. Deze API bestaat uit een aantal klassen in de java.awt.graphics namespace, en zijn gemakkelijk te benaderen via de [Graphics2D](https://docs.oracle.com/javase/7/docs/api/java/awt/Graphics2D.html) klasse. Deze Graphics2D klasse slaat intern een aantal attributen op die bepalen hoe getekend gaat worden, zoals welke kleur gebruikt gaat worden. Daarnaast is er een uitgebreide [Shape](https://docs.oracle.com/javase/7/docs/api/java/awt/Shape.html)-library die gebruikt kan worden om verschillende vormen te combineren. 

# Makkelijk gebruiken
Java2D is te gebruiken door via een [Graphics2D](https://docs.oracle.com/javase/7/docs/api/java/awt/Graphics2D.html) object. Dit object komt intern uit java, en kan gebruikt worden om op verschillende dingen te tekenen, zoals een JPanel. Om op een JPanel te tekenen kunnen we de volgende code gebruiken:

```java
public class Java2DDemo extends JPanel
{
    public static void main(String[] args)
    {
        JFrame frame = new JFrame("Java2D");
        frame.setSize(800, 600);
        frame.setContentPane(new Java2DDemo());
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }

    Java2DDemo()
    {
//init
    }

    public void paintComponent(Graphics g)
    {
        super.paintComponent(g);
        Graphics2D g2d = (Graphics2D)g;
//teken
    }

}
```

# Lijnen


# Kleuren


# Transformaties


# Parametrische vergelijkingen
