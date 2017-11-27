# Shapes
Naast het tekenen van lijnen, kunnen we ook allerlei andere vormen tekenen. Al deze vormen erven over van de Shape klasse, en kunnen we met dezelfde ```draw``` methode tekenen. In de Java2D library zitten al een aantal klassen die deze shape klasse implementeren.
![shapes](les2/shapes.png)
- Line2D
  Tekent een lijnstuk. Dit lijnstuk heeft een beginpunt en eindpunt
- Rectangle2D
  Tekent een rechthoek. De constructor ```new Rectangle2D.Double(double x, double y, double width, double height)``` kun je gebruiken om een rectangle te maken op een bepaalde positie
- Ellipse2D
  Tekent een ellipse. Je kunt de constructor ```new Ellipse2D.Double(double x, double y, double width, double height)``` een ellipse maken. Om een cirkel te tekenen kun je dit ook object gebruiken, met een gelijke breedte en hoogte.
- Arc2D
  Tekent een stuk van een cirkel of een ellipse. Je kunt de constructor ```new Arc2D.Double(double x, double y, double width, double height, double start, double extend, int type)``` gebruiken om een stuk van een ellipse te tekenen. Je kunt de positie aangeven, de grootte en waar begonnen moet worden. Deze hoek begint rechts met 0°, en gaat tegen de klok in. Je moet deze deze hoek in graden opgeven (0° - 360°). 
- CubicCurve2D
  Je kunt met de ```CubicCurve2D.Double(double x1, double y1, double ctrlx1, double ctrly1, double ctrlx2, double ctrly2, double x2, double y2)``` een [Bézierkromme](https://nl.wikipedia.org/wiki/Bézierkromme) met 2 steunpunten tekenen. Je geeft een beginpunt op (x1,y1), een eindpunt (x2,y2), en 2 steunpunten. De curve zal door het begin en eindpunt gaan, maar niet door de steunpunten
- QuadCurve2D
  Je kunt met de ```QuadCurve2D.Double(double x1, double y1, double ctrlx, double ctrly, double x2, double y2)``` een [Bézierkromme](https://nl.wikipedia.org/wiki/Bézierkromme) met 1 steunpunt tekenen. Je geeft een beginpunt op (x1,y1), een eindpunt (x2,y2), en 1 steunpunt. De curve zal door het begin en eindpunt gaan, maar niet door het steunpunt
- RoundRectangle2D
  Je kunt met de ```RoundRectangle.Double(double x, double y, double width, double height, double arcwidth, double archeight)``` een rechthoek tekenen met afgeronde hoeken. De arcwidth en archeight geven aan hoe breed en hoog het hoekje zijn dat afgerond wordt. 

Je kunt deze vormen tekenen met de ```Graphics.draw(Shape shape)``` of de ```Graphics.fill(Shape shape)``` methoden. De draw methode tekent een lijn in de vorm van de Shape die is opgegeven, en de fill methode vult de vorm op. Deze lijn of opvulling kun je een kleur geven met de ```setColor``` methode, maar kunnen we ook een beter opvulling geven. Hierover meer in de hoofdstukken over [Strokes](#Strokes) en [Paints](#Paints). 

Daarnaast kun je aan shapes no een aantal vragen stellen. Zo kun je bijvoorbeeld kijken of een punt binnen de shape is met de ```contains``` methode. Deze methode kun je bijvoorbeeld gebruiken om te kijken of de gebruiker op een vorm heeft geklikt.
Daarnaast kun je ook een bounding-rectangle opvragen met de ```getBounds()``` methode. Dit is de rechthoek die de volledige shape omlijnt.

# Paths
Een speciale shape is een Path2D. Een Path is een combinatie van lijnen die gebruikt kunnen worden veel verschillende vormen te maken. De Path klasse wordt geïmplementeerd door de GeneralPath klasse, dus deze kunnen we gebruiken. Een generalpath werkt als een soort pen. Je kunt de pen over het canvas bewegen om zo een vorm te tekenen
```java
public void paintComponent(Graphics g)
{
	super.paintComponent(g);
	Graphics2D g2d = (Graphics2D)g;

	GeneralPath path = new GeneralPath();
	path.moveTo(100, 100);
	path.lineTo(200,100);
	path.lineTo(100,200);
	path.closePath();

	g2d.setColor(Color.green);
	g2d.fill(path);
	g2d.setColor(Color.black);
	g2d.draw(path);
}
```
![GeneralPath](les2/generalpath.png)

De GeneralPath klasse heeft de volgende methoden
- ```moveTo(double x, double y)``` beweegt de cursor naar positie (x,y) zonder een lijn te tekenen
- ```lineTo(double x, double y)``` Tekent een lijn van de huidige locatie naar locatie (x,y)
- ```quadTo(double x1, double y1, double x2, double y2)``` Tekent een bezier curve naar (x1,y1) met steunpunt (x2,y2)
- ```curveTo(double x1, double y1, double x2, double y2, double x3, double y3)``` Tekent een bezier curve naar (x1,y1) met steunpunten (x2,y2) en (x3,y3)
- ```closePath()``` Sluit het pad met een rechte lijn naar het beginpunt

Om een vorm te vullen, moet je deze altijd afsluiten met closePath, anders kan de inkt weglekken en zou het hele scherm gevuld worden

Het is niet altijd voordehandliggend welk gedeelte van de shape gevuld wordt, zeker als de lijnen van het pad elkaar kruisen. Bekijk de volgende voorbeeldcode met de lijnen die hieruit komen:
```java
myShape = new GeneralPath();
myShape.moveTo(-2f, 0f);
myShape.quadTo(0f, 2f, 2f, 0f);
myShape.quadTo(0f, -2f, -2f, 0f);
myShape.moveTo(-1f, 0.5f);
myShape.lineTo(-1f, -0.5f);
myShape.lineTo(1f, 0.5f);
myShape.lineTo(1f, -0.5f);
myShape.closePath();
```
![myShape](les2/weirdshape.png)

Als we deze vorm gaan opvullen, krijgen we een probleem, welke onderdelen worden er nu gevuld? In java kunnen we kiezen uit 2 verschillende manieren van vullen
- even-odd rule
  
  ![even-odd](les2/even-odd.png)
  
  We trekken een virtuele lijn door de vorm heen, en verhogen een teller op iedere plek waar de lijn door de shape heengaat. Buiten de shape staat de teller op 0. De plaatsen waar de teller even is zijn 'buiten' de vorm, de plaatsen waar de teller oneven is zit 'binnen' de vorm. 
- nonzero rule

  ![nonzero](les2/nonzero.png)

  We trekken weer een virtuele lijn door de vorm heen. Daarnaast geven we alle lijnen een richting (de richting waarin ze zijn getekent). Als we nu onze virtuele lijn volgen en de rand van de shape kruis van rechts, halen we 1 van de teller af, van links tellen we 1 bij de teller op. De plaatsen waar de teller 0 is zijn buiten de shape, op plaatsen waar de teller iets anders dan 0 is is binnen de shape.

# Areas en Constructive Solid Geometry

# Strokes

# Paints

# Transformeren van shapes

# Muisinteractie

# Opgaven
