# Physics Engine

Om een interactieve wereld te maken met objecten die reageren zoals in de werkelijke wereld kunnen we dit uitprogrammeren, maar er kan oko gebruik gemaakt worden van een bestaande physics engine. Een voorbeeld van een bekende 2D physics engine is [Box2D](http://www.box2d.org), gebruikt in onder andere Angry Birds. Deze engine is echter geschreven in C++, en kunnen we niet gemakkelijk binnen java gebruiken. Gelukkig zijn er ook engines beschikbaar voor java, zoals [dyn4j](http://www.dyn4j.org). Deze engine wordt nog actief ontwikkeld, en bevat dezelfde concepten als andere physics engines, zoals RigidBodies, Fixtures en Joints

## Installatie

![add library](les5/addlibrary.png?right) Om dyn4j te gebruiken, moeten we deze bij het project toevoegen. Je kunt de jarfile van [dync4j](http://www.dyn4j.org) downloaden en via verkenner in je project zetten. Door de jarfile hierna te markeren als library, kan IntelliJ deze jarfile gebruiken. Dit kun je doen door het betand te rechtsklikken, en hierna op 'mark as library' te klikken. Hierna is de jarfile als library gemarkeerd en kunnen we deze gebruiken.

![classpath](les5/classpath1.png?left) De library zal dan echter nog niet in het classpath van het project staan. De gemakkelijkste manier om dit toe te voegen is door een variabele te maken van het type World, en dan via de autocorrectie te kiezen om de library aan het classpath toe te voegen. Deze instellingen kun je natuurlijk ook in de module instellingen vinden, in het kopje Modules, op het tabblad 'Dependencies'

## World

Alles in de physics engine speelt zich af in een wereld, een World object. Dit world object bevat alle objecten die zich in de wereld bevinden en heeft daarbij methoden om de wereld een tijdsstap vooruit te zetten. Daarnaast heeft een wereld ook een aantal eigenschappen, zoals zwaartekracht, hulpmethoden om collision te bepalen en rays te casten. 

## Body

## Fixture

## Debug Draw

## Plaatjes en echt gebruik

## Meer over fixtures

## Joints

### Distance Joint

### Revolute Joint

### Weld Joint

### Prismatic Joint

## Raycasting en collision testing

## Gebruik - Het slepen van een object met je muis

## Opdrachten

1. Maak angry birds

2. 