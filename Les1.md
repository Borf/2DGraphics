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
Deze code maakt een JFrame aan met een aantal standaard opties en zet hier een nieuw contentpanel in. Dit contentpaneel is van het type Java2DDemo. Deze klasse override de paintComponent methode, en hier kun je code in zetten om te tekenen. Door de super.paintComponent aan te roepen, wordt nog wel de originele tekencode uitgevoerd voor het tekenen van andere componenten, en wordt 't scherm leeggemaakt.

# Lijnen
Java2D werkt met een [Carthesisch Coordinatenstelsel](https://nl.wikipedia.org/wiki/Cartesisch_coördinatenstelsel). In het kort betekent dit dat er gebruik wordt gemaakt van een X- en een Y-as. De oorsprong van het coordinatenstelsel ligt standaard linksboven in het venster, waarbij de Y-as naar beneden loopt. Dit is dus gespiegeld ten opzichte van het wiskundige assenstelsel dat we gewend zijn. Standaard, is de eenheid een pixel. Dit betekent dus dat punt (200,100) 200 pixels naar rechts, en 100 pixels naar beneden ten opzichte van de linkerbovenhoek ligt.

Om een lijn te tekenen kunnen we gebruik maken van de draw methode in het Graphics2D object. Deze methode wil een Shape als parameter, en een [Line2D](https://docs.oracle.com/javase/7/docs/api/java/awt/geom/Line2D.html) is een voorbeeld van een Shape. De Line2D klasse heeft geen public constructor, en we zullen gebruik moeten maken van een van de subklassen van Line2D, zoals [Line2D.Double](https://docs.oracle.com/javase/7/docs/api/java/awt/geom/Line2D.Double.html). Deze kunnen we direct aan de draw methode meegeven, voor de volgende code

```java
public class Line2DDemo extends JPanel
{
    public static void main(String[] args)
    {
        JFrame frame = new JFrame("Java2D");
        frame.setSize(800, 600);
        frame.setContentPane(new Java2DDemo());
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }

    Line2DDemo()
    {
    }

    public void paintComponent(Graphics g)
    {
        super.paintComponent(g);
        Graphics2D g2d = (Graphics2D)g;
        g2d.draw(new Line2D.Double(200, 100,    500, 200));
    }

}
```
Deze code zal een lijn tekenen van de coordinaat (200,100) naar (500,200)

# Kleuren
De Graphics klasse slaat op met welke kleur getekend gaat worden. Deze kleur kun je veranderen, en alle opvolgende teken-commandos zullen met deze kleur getekend worden. 

# Transformaties
## 2D Rendering pipeline


# Parametrische vergelijkingen

