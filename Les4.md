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

    public void draw(Graphics2D g2d)
    {
        g2d.fill(new Ellipse2D.Double(position.getX()-5, position.getY()-5, 10, 10));
    }
}
```

Door nu iedere keer dat de timer tikt de update aan te roepen, krijgen we een vallend punt. We kunnen een lijst maken met particles, en deze in een panel tekenen en updaten.

```java
class VerletDemo extends JPanel implements ActionListener
{
    public static void main(String[] args)
    {
        JFrame frame = new JFrame();
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setContentPane(new VerletDemo());
        frame.setMinimumSize(new Dimension(800,600));
        frame.setVisible(true);
    }

    private ArrayList<Particle> particles = new ArrayList<>();

    VerletDemo()
    {
        particles.add(new Particle(100,100));
        new Timer(1000/60,this).start();
    }

    public void actionPerformed(ActionEvent e)
    {
        for(Particle p : particles)
            p.update();
        repaint();
    }

    public void paintComponent(Graphics g)
    {
        super.paintComponent(g);
        Graphics2D g2d = (Graphics2D)g;

        for(Particle p : particles)
            p.draw(g2d);
    }
}
```

Deze code zal een particle aanmaken, updaten en tekenen, met de mogelijkheid om er meer te tekenen. Deze voorstelling werkt goed voor simpele particles, maar het is nu erg lastig om direct het gedrag te beïnvloeden. Als een particle stilgezet wordt of verplaatst wordt vanuit de gebruikerscode, moet ook de snelheid opnieuw berekend worden, en past ook de acceleratie aan. We kunnen de snelheid ook integreren, waardoor we de positie en vorige positie opslaan. De snelheid is dan gelijk aan de (huidige positie - vorige positie). We kunnen dit in de volgende code samenvatten

```java
class Particle
{
    private Point2D position;
    private Point2D lastPosition;
    private Point2D acceleration;

    public Particle(Point2D position)
    {
        this.position = position;
        this.lastPosition = position;
        this.acceleration = new Point2D.Double(0,-9.8); // gravity
    }

    public void update()
    {
        Point2D old = position;
        position = new Point2D.Double(
                position.getX() + (position.getX() - lastPosition.getX()) + force.getX(),
                position.getY() + (position.getY() - lastPosition.getY()) + force.getY()
        );
        lastPosition = old;
    }

    public void draw(Graphics2D g2d)
    {
        g2d.fill(new Ellipse2D.Double(position.getX()-5, position.getY()-5, 10, 10));
    }
}
```

Let hierbij op dat de positie eerst opgeslagen wordt voordat deze veranderd wordt, omdat deze nog in de lastPosition gezet moet worden.