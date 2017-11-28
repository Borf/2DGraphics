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
Een andere manier van 't maken van vormen is Constructive Solid Geometry. Dit is in het kort het combineren van 2 vormen om een nieuwe vorm te maken. Dit kunnen we doen met 3 operaties

## Vereniging (add)
![add](les2/csg_add.png)

Door de vereniging te nemen van 2 vormen, krijg je een nieuwe vorm met een combinatie van beide vormen. Er komt 1 nieuwe vorm uit, en de lijnstukken die tussen de 2 vormen in zitten, vallen weg

## Verschil (subtract)
![subtract](les2/csg_subtract.png)

Geeft eerste vorm, waar de tweede vorm als een hap uitgenomen is. Dit kan gaten opleven in de vorm. Let op de volgorde van de vormen, het verschil tussen vorm A en B, en B en A is anders.

## Doorsnede (intersect)
![intersect](les2/csg_intersect.png)

Geeft alleen de ruimte die beide vormen zit. 

## Exclusieve of (xor)
![xor](les2/csg_xor.png)

Geeft alleen de ruimte die of in de een, of in de andere vorm zit, maar niet allebei. Is gelijk aan ```Verschil(Vereniging(A, B), Doorsnede(A, B))```

## Gebruik van CSG
In java werkt CSG door middel van de [Area](https://docs.oracle.com/javase/7/docs/api/java/awt/geom/Area.html) klasse. Deze klasse is ook een Shape, en kan gemaakt worden op basis van een shape in de constructor. Door een bestaande Shape in een Area te encapsuleren kun je dus gemakkelijk CSG operaties hierop toepassen, en het resultaat kun je meteen tekenen.
```java
public void paintComponent(Graphics g)
{
    super(g);
    Graphics2D g2d = (Graphics2D)g;

    Area a = new Area(new Ellipse2D.Double(0,0,100,100));
    Area b = new Area(new Ellipse2D.Double(50,0,100,100));

    Area added = new Area(a);
    added.add(b);

    Area sub = new Area(a);
    sub.subtract(b);

    Area intersect = new Area(a);
    intersect.intersect(b);

    Area xor = new Area(a);
    xor.exclusiveOr(b);

    g2d.translate(25,25);

    g2d.setColor(Color.lightGray);
    g2d.fill(added);
    g2d.setColor(Color.black);
    g2d.draw(a);
    g2d.draw(b);

    g2d.translate(0,150);
    g2d.setColor(Color.lightGray);
    g2d.fill(sub);
    g2d.setColor(Color.black);
    g2d.draw(a);
    g2d.draw(b);

    g2d.translate(0,150);
    g2d.setColor(Color.lightGray);
    g2d.fill(intersect);
    g2d.setColor(Color.black);
    g2d.draw(a);
    g2d.draw(b);

    g2d.translate(0,150);
    g2d.setColor(Color.lightGray);
    g2d.fill(xor);
    g2d.setColor(Color.black);
    g2d.draw(a);
    g2d.draw(b);
}
```
Het is natuurlijk ook mogelijk om verschillende operaties achter elkaar uit te voeren op 1 Area, bijvoorbeeld door er een aantal keer verschillene stukjes af te hakken. 


Je kunt CSG gebruiken om nieuwe vormen te maken, maar je kunt er nog meer mee doen
- Overlap detecteren
- Onwikkelen van grote complexe vormen (levels)

# Strokes
De ```Graphics2D.draw()``` methode tekent standaard een simpele lijn. Deze lijn noemen we een '[Stroke](https://docs.oracle.com/javase/7/docs/api/java/awt/Stroke.html)'. De stroke kun je instellen met de [```setStroke(Stroke newStroke)```](https://docs.oracle.com/javase/7/docs/api/java/awt/Graphics2D.html#setStroke(java.awt.Stroke)) methode. Een van de stroke-klassen is de [BasicStroke](https://docs.oracle.com/javase/7/docs/api/java/awt/BasicStroke.html). Met deze stroke kun je de dikte instellen, het soort afrondingen aan het einde van de lijn, en hoe de knikken in de lijnstukken getekend worden. Daarnaast kun je ook een streeppatroon instellen. Dit kan allemaal met de ```BasicStroke(float width, int cap, int join, float miterlimit, float[] dash, float dash_phase)``` constructor, maar het is ook mogelijk de laatste opties weg te laten met bijvoorbeeld de ```BasicStroke(float width, int cap, int join)``` constructor. De width geeft de breedte van de lijnen aan. De cap en join geven de eindes en tussenstukken van lijnstukken aan. 

![Caps and Joins](les2/caps_and_joins.png)
- JOIN_ROUND geeft een afgeronde hoek bij scherpe hoeken
- JOIN_BEVEL knipt een hoek af bij scherpe hoeken
- JOIN_MITER laat een scherpe punt bij scherpe hoeken
- CAP_BUTT geeft geen extra stukken bij het uiteinde van de lijnstukken
- CAP_SQUARE zet een extra, recht stuk aan het uiteinde van de lijnstukken met lengte van de helft van de breedte
- CAP_ROUND rond de uiteinden van de lijnstukken af met een cirkel 
```
Stroke s = new BasicStroke(4.0f,
                           BasicStroke.JOIN_ROUND, 
                           BasicStroke.CAP_ROUND);
```
Deze waarden zijn integer constanten in de BasicStroke klasse, en kunnen gebruikt worden in de constructor. In het geval van de JOIN_MITER, kun je ook een extra parameter aan de constructor meegeven die de limiet van de miter-join aangeeft. Deze limiet is de diagonale afstand van de miter, dus de afstand tussen de binnenste en buitenste hoek. Standaard staat deze op 10.0f.

![dashes](les2/dash.png)
Daarnaast is het ook mogelijk een streep-patroon mee te geven. Deze patronen kun je in een array doorgeven en je kunt een verschuiving aangeven in een parameter. In de array staat de lengte van de gekleurde gebieden en hierna de lengte van de nietgekleurde gebieden. Het is dus eigenlijk altijd een array met een even aantal elementen. Door de verschuiving kun je aangeven waar in het patroon begint

# Paints
De ```Graphics2D.fill()``` methode kan een vorm invullen. Standaard is dit met een enkele kleur, maar dit kun je instellen met de ```Graphics2D.setPaint(Paint paint)``` methode. Hier kun je een paint object meegeven, dat bepaalt hoe de vorm gevuld wordt.

## Color
De color klasse implementeerd ook de Paint interface, en kun je gebruiken om een Shape in 1 kleur in te kleuren. De kleur kun je aanmaken met alle constructoren van Color, op dezelfde manier als [setColor](Les1#Kleuren)

## GradientPaint
![Gradientpaint](les2/gradientpaint.png)

Een gradientpaint maak je aan met 2 punten en 2 kleuren. Java zal dan lineair interpoleren over een lijn die tussen deze 2 punten loopt. Je kunt in de constructor ook aangeven of deze gradient cyclisch of asyclisch is. Een cyclische gradient zal zichzelf blijven herhalen na de 2 punten, en een acyclische gradienet blijft dezelfde kleur na deze 2 punten

## MultipleGradientPaint

## LinearGradientPaint

## RadialGradientPaint

## TexturePaint

# Transformeren van shapes

# Muisinteractie

# Opgaven
