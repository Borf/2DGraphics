# Verlet Particles

We hebben vorige les gezien hoe we punten kunnen animeren. Om nu een bewegend punt op te slaan, kunnen we dit punt modelleren met een locatie, snelheid en versnelling. Om dit punt een tijdstap verder te zetten, kunnen we het volgende gebruiken:

```java
class Particle
{
    private Point2D position;
    private Point2D speed;
    private Point2D acceleration;


    public Particle(Point2D position)
    {
        this.position = position;
        this.speed = new Point2D.Double(0,0);
        this.acceleration = new Point2D.Double(0,-9.8); // gravity
    }

    public void update()
    {
        this.position = new Point2D.Double(position.getX() + speed.getX(), position.getY() + speed.getY());
        this.speed = new Point2D.Double(speed.getX() + acceleration.getX(), speed.getY() + acceleration.getY());
    }
}
```

Door nu iedere keer dat de timer tikt de update aan te roepen, krijgen we een vallend punt

