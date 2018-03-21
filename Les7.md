# Platformer game

## Achtergrond inladen

[![smb](les7/smb.png?thumbright)](les7/smb.png)De achtergrond die we gaan gebruiken is een tiled map. Deze map kan in tiled gemaakt worden, en hierna via de json file in code ingeladen worden. Een tiled file bestaat uit 2 belangrijke elementen, layers en tilemaps. Omdat de focus van deze applicatie niet op de tiled map ligt, worden wat aannames gemaakt. Door aan te nemen dat de map maar 1 layer heeft, en 1 tileset gebruikt, wordt de code om deze map in te laden en de tilesets te tekenen een stuk simpeler. De collision kan straks op basis van tileIDs gedaan worden. De attributen die opgeslagen worden zijn

```java
BufferedImage[] tiles;
int[][] data;
int width;
int height;
```

Deze kunnen hierna met de volgende code ingeladen worden:
```java
JsonReader reader = Json.createReader(getClass().getResourceAsStream(fileName));
JsonObject levelObject = reader.readObject();
//lees de afmetingen in
width = levelObject.getInt("width");
height = levelObject.getInt("height");

try {
    //knip afbeelding op in stukjes van 16x16
    BufferedImage image = ImageIO.read(getClass().getResource("/smb.png"));
    int tileCount = image.getWidth()/16 * image.getHeight()/16;
    tiles = new BufferedImage[tileCount];
    int i = 0;
    for(int y = 0; y < image.getHeight(); y+=16)
        for(int x = 0; x < image.getWidth(); x+=16)
            tiles[i++] = image.getSubimage(x,y,16,16);
} catch (IOException e) {
    e.printStackTrace();
}
//laad de map in
data = new int[width][height];
int i = 0;
for(int y = 0; y < height; y++)
    for(int x = 0; x < width; x++)
        data[x][y] = levelObject.getJsonArray("layers").getJsonObject(0).getJsonArray("data").getInt(i++)-1;
```

Het tekenen is simpelweg doorlopen van de data-array en het op het scherm zetten van de juiste tiles. De draw methode heeft een extra parameter, cameraX, die aangeeft hoever de camera gescrolled is. De camera scrollpositie is een losse variabele die gebruikt wordt om het level te scrollen. Dit wordt niet in een globale camera-affinetransform gedaan, maar bij het tekenen. De tileset is wat klein voor hedendaagse schermen, dus wordt deze geschaald met een factor 3. 

```java
public void draw(Graphics2D g2d, double cameraX) {
    int tileStart = (int) (cameraX / 16);
    for(int x = tileStart; x < tileStart + 20; x++)
    {
        for(int y = 0; y < height; y++)
        {
            AffineTransform tx = new AffineTransform();
            tx.scale(3,3);
            tx.translate(16*x - cameraX, 16*y);
            g2d.drawImage(tiles[data[x][y]], tx, null);
        }
    }
}
```

## De speler

[![player](les7/player.png?thumbright)](les7/player.png)De physics van de speler komen in een losse klasse. Deze klasse heeft een update- en teken-methode die we moeten implementeren. De graphics van de speler bestaat uit een spritesheet met verschillende sprites naast elkaar. In de spelerklasse kunnen we opslaan welk frame op dit moment getekend moet worden, de positie en de snelheid.

```java
private BufferedImage[] images;
private int frame = 0;
private Point2D position;
private Point2D speed;
private int height = 16;
```

In de constructor moet de spritesheet opgeknipt worden in de losse sprites.

```java
Player(String filename, Point2D position)
{
    this.position = position;
    this.speed = new Point2D.Double(0,0);
    try {
        BufferedImage image = ImageIO.read(getClass().getResource(filename));
        int frameCount = image.getWidth()/16;
        images = new BufferedImage[frameCount];
        for(int i = 0; i < frameCount; i++)
            images[i] = image.getSubimage(16*i,0,16,image.getHeight());

    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

De draw methode heeft nog een extra feature, deze kan een sprite flippen als de horizontale snelheid naar links is (negatief)

```java
public void draw(Graphics2D g, double cameraX)
{
    AffineTransform tx = new AffineTransform();

    tx.scale(3,3);
    tx.translate(position.getX(), position.getY());
    tx.translate(-cameraX, 0);
    tx.translate(0, -images[frame].getHeight());

    if(speed.getX() < 0)
    {
        tx.translate(images[frame].getWidth(),0);
        tx.scale(-1,1);
    }

    g.drawImage(images[frame], tx, null);

}
``` 

Om de speler te updaten moet er meer code geschreven worden. In pseudocode ziet het bewegen er als volgt uit:

```pseudo
Bereken de nieuwe X positie op basis van de snelheid
controleer of er een collision is op de nieuwe positie
    controleer alleen aan de rechterkant als de speler naar rechts gaat
    controlleer alleen aan de linkerkant als de speler naar links gaat
Als er geen collision is, update dan de positie naar (newX, oldY)
Als er wel een collision is, is er tegen een muur gebotst


Bereken de nieuwe Y positie op basis van de snelheid
Controleer of er een collision is op de nieuwe positie
    Controleer of er een collision onder de speler is
    Controleer of er een collision boven de speler is
Als er geen collision is, laat de speler vallen
Als er wel een collision is, zet de Y op een op 16-tallen-afgeronde positie (y = floor(y/16)*16), dit zorgt ervoor dat de speler netjes op de grond blijft staan
```

Hiervoor moet er vaak gekeken worden of er collision is tussen een speler en het level. Hiervoor is het handig een extra hulpmethode toe te voegen aan het level, de ```hasCollision(double x, double y)```. De x en y zijn in speler-coördinaten (niet in tilecoördinaten of pixelcoördinaten), en er wordt dus gekeken of dit punt binnen een blokkerende tile

```java
int[] blocking = { 0, 1, 2, 3, 10, 11, 26, 27, 32 };
public boolean hasCollision(double x, double y) {
    int tileX = (int)(x / 16);
    int tileY = (int)(y / 16);

    if(tileX < 0 || tileX >= width || tileY < 0 || tileY >= height)
        return false;
    int tile = data[tileX][tileY];

    int index = Arrays.binarySearch(blocking, tile);
    return index >= 0 && index < blocking.length;
}
```

Voor de collision moeten we bepaalde punten bekijken in de collisionmap. Als de speler op (x,y) staat, gebruiken we een rechthoek van (x+1, y-1) - (x+15, y-height). Deze rechthoek is dus net iets kleiner dan de sprite zelf. De Y waarde is negatief, omdat een negatieve Y-waarde omhoog gaat. Met de volgende code kan nu een update uitgevoerd worden:
```java
public void update(double elapsedTime, Level level) {
    boolean collision;

    double newX = position.getX() + speed.getX() * elapsedTime;
    collision = false;

    if(speed.getX() > 0) {
        if (level.hasCollision(newX + 15, position.getY()-1))
            collision = true;
        if (level.hasCollision(newX + 15, position.getY()-height))
            collision = true;
    }
    else if(speed.getX() < 0) {
        if (level.hasCollision(newX + 1, position.getY()-1))
            collision = true;
        if (level.hasCollision(newX + 1, position.getY()-height))
            collision = true;
    }
    //niet tegen de muur gebotst
    if(!collision || !hasCollision)
        position = new Point2D.Double(newX, position.getY());

    double newY = position.getY() + speed.getY() * elapsedTime;
    collision = false;

    //collision feet
    if(level.hasCollision(position.getX()+1, newY))
        collision = true;
    if(level.hasCollision(position.getX()+14, newY))
        collision = true;

    //collision head
    if(level.hasCollision(position.getX()+1, newY-height)) {
        collision = true;
    }
    if(level.hasCollision(position.getX()+14, newY-height)) {
        collision = true;
    }
    if(!collision || !hasCollision) {
        position = new Point2D.Double(position.getX(), newY);
        speed = new Point2D.Double(speed.getX(), speed.getY() + 300 * elapsedTime);
    }
    else {
        speed = new Point2D.Double(speed.getX(), 0);
        position = new Point2D.Double(position.getX(), Math.round(position.getY()/16)*16);
    }
}
```

### Toetsenbord


## De camera 

## Het gameobject

## Vijanden

## Interactie met vijanden

## Interactie met blokken