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
De Graphics klasse slaat op met welke kleur getekend gaat worden. Deze kleur kun je veranderen, en alle opvolgende teken-commandos zullen met deze kleur getekend worden. De kleur kun je aanpassen met de setColor(Color color) methode. Kleuren kunnen op verschillende manieren aangemaakt worden, via de [Color](https://docs.oracle.com/javase/7/docs/api/java/awt/Color.html) klasse:
- ```new Color(float r, float g, float b)```
  
  Maakt een kleur aan met rood, groen, blauw waarden. De parameters liggen tussen 0 en 1. ```new Color(1.0f, 1.0f, 1.0f)``` geeft dus een witte kleur
- ```new Color(int r, int g, int b)``` 

  Maakt een kleur aan met rood, groen, blauw waarden. De parameters liggen tussen 0 en 255
- ```Color.getHSBColor(float hue, float saturation, float brightness)``` 

  Maakt een kleur aan volgens het HSB model. Met de hue kun je een kleur instellen, de saturation is de kleurverzadiging en de brightness de helderheid. De parameters liggen tussen 0 en 1, dus Color.getHSBColor(0.0f, 1.0f, 1.0f) geeft rood
- ```Color.black, Color.white, Color.green``` 
  Kleur-constanten zijn binnen java gedefinieerd als vaste basiskleuren die je gemakkelijk kunt gebruiken. De volledige lijst kun je vinden in [de java documentatie](https://docs.oracle.com/javase/7/docs/api/java/awt/Color.html#black)

Door nu een kleur aan te maken, en deze te zetten in het Graphics object, kun je bijvoorbeeld lijnen tekenen met deze kleur. Door steeds nieuwe kleuren te maken kunnen we verschillende lijnen tekenen met verschillende kleuren
```
import javax.swing.*;
import java.awt.*;

public class HelloColors extends JPanel {
	public static void main(String[] args)
	{
		JFrame frame = new JFrame("Hello Java2D");
		frame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
		frame.setMinimumSize(new Dimension(800, 600));
		frame.setExtendedState(frame.getExtendedState() | JFrame.MAXIMIZED_BOTH);
		frame.setContentPane(new HelloColors());
		frame.setVisible(true);
	}

	public void paintComponent(Graphics g)
	{
		super.paintComponent(g);
		Graphics2D g2d = (Graphics2D)g;

		for(int i = 0; i < 500; i++) {
			g2d.setColor(Color.getHSBColor(i/500.0f, 1, 1));
			g2d.drawLine(i, 10, i, 100);
		}
	}
}
```
Deze code zal dus 500 lijnen tekenen, met ieder een andere kleur op basis van het HSB model. De hue verloopt hierbij van 0° tot 360°, waardoor je het hele kleurenspectrum te zien krijgt

# Transformaties
Het standaard coordinatenstelsel in java2D heeft de oorsprong in de linkerbovenhoek van het scherm zitten, waarbij de Y-as naar beneden gaat. Soms is dit echter onhandig met het tekenen, en is het bijvoorbeeld handiger om dit assenstelsel aan te kunnen passen. Dit kan in de computer graphics met 3 verschillende acties
- **Transleren**

  Transleren ofwel verplaatsen, verplaatst de oorsprong van het coordinatenstelsel. Dit kun je dus bijvoorbeeld gebruiken om de oorsprong op het centrum van het venster te leggen. We kunnen dit doen met de ```translate(double x, double y)``` methode in het Graphics2D object. Door ```g2d.translate(100,100);``` uit te voeren, verplaatsen we de oorsprong naar de pixel-coordinaat (100,100) in het venster. Met ```g2d.translate(getWidth()/2, getHeight()/2);``` kunnen we de hoogte en breedte van het paneel opvragen, en komt de oorsprong in het midden van het venster te liggen.
- **Roteren**

  Rotatie draait het coordinatenstelsel rond de oorsprong
- **Schalen**

  Schalen vergroot of verkleint het coordinatenstelsel vanuit de oorsprong.

## Combineren van transformaties

# Parametrische vergelijkingen

