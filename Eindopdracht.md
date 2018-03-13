# Eindopdracht

De module 2D graphics wordt beoordeeld met de bespreking van een individueel gemaakte eindopdracht. In lesweek 6/7 worden de individuele opdrachten uitgereikt, de besprekingen vinden plaats in lesweek 8/9. In de tussentijd is er voor iedereen de gelegenheid de individuele opdracht uit te werken.
De individuele opdrachten zijn gecategoriseerd in moeilijkheid en/of complexiteit. De student mag zelf kiezen uit welke categorie een opdracht wordt gekozen. Wel is het zo dat een opdracht van een lagere complexiteit natuurlijk een minder hoog cijfer biedt. Dit maakt het mogelijk dat iedereen een opdracht op zijn/haar niveau kan kiezen en deze module met succes kan afronden. **Daarnaast worden de gemaakte opgaven meegenomen in de beoordeling**

<!-- TOC -->

- [Eindopdracht](#eindopdracht)
    - [Categorie 1 - Makkelijke opgave (cijfer richtlijn 6-7)](#categorie-1---makkelijke-opgave-cijfer-richtlijn-6-7)
        - [Geanimeerde tekst](#geanimeerde-tekst)
            - [Requirements:](#requirements)
        - [Robotarm](#robotarm)
            - [Requirements:](#requirements-1)
    - [Categorie 2 - Gemiddelde moelijkheid (cijfer richtlijn 7-8.5)](#categorie-2---gemiddelde-moelijkheid-cijfer-richtlijn-7-85)
        - [Particle Simulatie](#particle-simulatie)
            - [Requirements:](#requirements-2)
        - [Fractals](#fractals)
            - [Requirements:](#requirements-3)
        - [2D physics sandbox](#2d-physics-sandbox)
            - [Requirements:](#requirements-4)
    - [Categorie 3 - Moeilijk (8.5+)](#categorie-3---moeilijk-85)
        - [2D Physics game](#2d-physics-game)
            - [Requirements:](#requirements-5)
        - [Particle Flow](#particle-flow)
            - [Requirements:](#requirements-6)
        - [Vrije opdracht](#vrije-opdracht)

<!-- /TOC -->

## Categorie 1 - Makkelijke opgave (cijfer richtlijn 6-7)

### Geanimeerde tekst

Implementeer een applicatie waar een (grote) tekst in staat. Deze tekst moet voorzien zijn van een geanimeerde GradientPaint texture. Ook moet de tekst rondgesleept kunnen worden met de linkermuisknop (verplaatsen), en met de rechtermuisknop geschaald kunnen worden. De gradient schaalt dan mee

#### Requirements:

- Gescheiden update/draw code
- Tekst op het scherm
- Gradientpaint
- Animatie op de GradientPaint
- Slepen van de tekst, zonder verspringen
- Schalen van de tekst, met meeschalen van GradientPaint

### Robotarm

[![robot](eindopdracht/robotarm.png?thumbright)](eindopdracht/robotarm.png)Een robotarm bestaat uit een aantal onderdelen die aan elkaar vast zitten, maar wel kunnen ronddraaien met scharnierpunten. Modelleer een robotarm, teken deze met behulp van afbeeldingen en shapes, en zorg dat deze arm geanimeerd kan worden. Denk hierbij aan het hiernaast staande voorbeeld

#### Requirements:

- Gescheiden update/draw code
- Er moet een robotarm te zien zijn die bestaat uit minimaal 3 segmenten (3 scharnierpunten)
- De hoek van ieder scharnierpunt moet in te stellen zijn
- Gebruik maken van afbeeldingen voor de onderdelen van de robotarm

## Categorie 2 - Gemiddelde moelijkheid (cijfer richtlijn 7-8.5)

### Particle Simulatie

[![particles](eindopdracht/particles.jpg?thumbright)](eindopdracht/particles.jpg)Implementeer een applicatie waarin je explosies, rook en andere zaken kunt visualiseren door middel van particles. Voor verschillende mogelijkheden van particles, zie [youtube](https://www.youtube.com/watch?v=heW3vn1hP2E)
Voor inspiratie:
- https://www.youtube.com/watch?v=KQ9T1o4_Ha8
- http://www.youtube.com/watch?v=PT4awV311Bs
- https://www.youtube.com/watch?v=Cxu1riffkHA

#### Requirements:

- Gescheiden update/draw code
- Particle systeem met eigen code
- Verschillende kleurmogelijkheden
- Mogelijkheid tot gebruik van textures (afbeeldingen)
- Verschillende eigenschappen van particles voor verschillende effecten
  - Rook
  - Vuur
  - Explosie

### Fractals

[![fractals](eindopdracht/fractals.jpg?thumbright)](eindopdracht/fractals.jpg)Een fractal is een wiskundige vorm of set met een herhalende structuur. Maak een applicatie om een of verschillende fractals weer te geven. Een aantal bekende fractals zijn

- [Mandelbrot set](https://www.youtube.com/watch?v=PD2XgQOyCCk)
- [Sierpinski tapijt](https://www.youtube.com/watch?v=HGGwHqrW9Nc)
- [Koch sneeuwvlok](https://www.youtube.com/watch?v=PKbwrzkupaU)
- [Fractal boom](https://www.youtube.com/watch?v=yWRFCSIzej0)

#### Requirements:

- Gescheiden update/draw code
- Fractal systeem (bonuspunten voor herbruikbaarheid tussen verschillende fractals)
- Verschillende fractals (zie hierboven)
- Camerasysteem om in te zoomen op details
- Animatie in de opbouw van de fractal (niet voor alle fractals)
- Controle over de animatie van de opbouw van de fractal (niet voor alle fractals)

### 2D physics sandbox

[![physics](eindopdracht/physics.png?thumbright)](eindopdracht/physics.png)Maak een applicatie om te experimenteren met de dyn4j physics engine. In de applicatie kun je dynamisch objecten toevoegen van verschillende vormen, met verschillende eigenschappen, en deze kun je ook manipuleren.

#### Requirements:

- Gescheiden update/draw code
- Maakt gebruik van de dyn4j physics library
- Heeft een user interface om verschillende vormen toe te voegen
- Tekent afbeeldingen over de physics bodies heen
- Heeft een werkende debugdrawer
- Bodies kunnen met de muis versleept worden
- Bodies kunnen aangeklikt worden waarna de eigenschappen in een panel weergegeven worden
- De eigenschappen van deze bodies moeten aangepast kunnen worden
  - Restitution
  - Friction

## Categorie 3 - Moeilijk (8.5+)

### 2D Physics game

[![physics](eindopdracht/physicsgame.png?thumbright)](eindopdracht/physicsgame.png)Maak een game die gebruik maakt van de verschillende mogelijkheden van de dyn4j physics engine. Je bent vrij in het bepalen van de gameplay, maar het is belangrijk om de verschillende mogelijkheden van de physics engine te laten zien. Je kunt bijvoorbeeld denken aan 2D topdown racegame, een platformer game of een flipperkast

#### Requirements:

- Gescheiden update/draw code
- Maakt gebruik van de dyn4j physics library
- Maakt gebruik van forces om objecten te bewegen
- Maakt gebruik van joints om objecten aan elkaar te koppelen
- Maakt gebruik van afbeeldingen over de bodies
- Heeft een werkende debugdrawer

### Particle Flow

Maak een particlesysteem dat gebruik maakt van pathfinding voor de particles. Particles hebben natuurlijk een dynamisch gedrag (snelheid, versnelling). Deze particles hebben verder dezelfde mogelijkheden als die bij de particle simulatie opdracht zijn gegeven (blending, kleur, texture etc). Voor inspiratie, zie [youtube](https://www.youtube.com/watch?v=Bspb9g9nTto)

#### Requirements:

- Gescheiden update/draw code
- Particle systeem met eigen code
- Alphablending van de particles
- Verschillende kleurmogelijkheden
- Mogelijkheid tot gebruik van textures (afbeeldingen)
- Particles kunnen als een zwerm richting een punt navigeren
- De omgeving van de particles kan runtime worden aangepast om de pathfinding te demonstreren


### Vrije opdracht

Indien je zelf een goed idee hebt voor een opdracht, **overleg dit met de docent**. Stel in het kort de opdrachtomschrijving op, en kom **samen met de docent** tot een lijstje met requirements. **Na** goedkeuren van de opdracht kun je deze inleveren. Het is belangrijk dat deze opdracht uitdagend is, dus het kan zijn dat de docent een aantal extra requirements (in overleg) toevoegt
