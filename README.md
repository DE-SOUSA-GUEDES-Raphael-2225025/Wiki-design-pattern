# Wiki-design-pattern
Wiki des designs patterns

# Singleton #

## Intention
L'intention principale du Singleton est de garantir qu'il n'existe qu'une seule instance d'une classe particulière, et de fournir un moyen d'accéder à cette instance à partir de n'importe quel point du programme. Cela peut être utile dans des situations où une seule instance de la classe est suffisante pour coordonner les actions à travers le système, comme la gestion des ressources partagées ou la configuration globale. Le pattern Singleton garantit que la classe n'est instanciée qu'une seule fois en fournissant une méthode qui crée l'instance si elle n'existe pas, sinon, renvoie simplement l'instance existante. De plus, il restreint l'accès aux méthodes de création d'instance, assurant ainsi que la seule instance existante est accessible de manière contrôlée.

## Résolution de problème
- **Garantir une seule instance :** Son fonctionnement est le suivant : vous créez un objet, mais après un certain temps, vous décidez d’en créer un autre. Plutôt que de vous retrouver avec un objet flambant neuf, vous récupérez celui qui existe déjà. Il est impossible d’implémenter ce comportement avec un constructeur normal, puisqu’un constructeur doit théoriquement toujours retourner un nouvel objet.

- **Accès global :** Le Singleton fournit un point d'accès global à cette unique instance. Cela simplifie l'accès à cette instance depuis n'importe quelle partie du code, évitant ainsi la nécessité de passer explicitement l'instance entre différentes parties du programme. Cependant, il protège son instance et l’empêche d’être modifiée.

- **Initialisation paresseuse :** Certains Singleton sont conçus pour une initialisation paresseuse, où une seule instance est créée uniquement lorsqu'elle est réellement nécessaire. Cela évite une initialisation au démarrage du programme. L'initialisation est ainsi faite lorsque du premier appel de la methode ```getInstance()```.

## Solution
1. Limiter l'accès à l'opérateur `new` en déclarant le constructeur par défaut en tant que privé, empêchant ainsi d'autres objets d'invoquer directement la création d'instances de la classe Singleton.
2. Introduire une méthode statique de création agissant comme un constructeur, qui en interne utilise le constructeur privé pour instancier un objet et le stocke dans une variable statique. Tous les appels ultérieurs à cette méthode renvoient l'objet mis en mémoire cache. Une variable static ```Instance``` est donc initialisée et permet d'accéder à la classe avec un getter ```getInstance()```.
3. Si votre code a accès à la classe d'un singleton, vous pouvez appeler ses méthodes statiques. Le même objet est toujours renvoyé à chaque appel de cette méthode.

## Analogie
Imaginons un roi dans un royaume qui veut garantir qu'il n'y ait qu'un seul trésor national. Le roi crée une clé spéciale qu'il garde toujours avec lui. Quand quelqu'un a besoin d'accéder au trésor, il doit demander la clé au roi. Si la clé n'existe pas encore, le roi la crée ; sinon, il la partage. Ainsi, il n'y a qu'une seule clé (instance) pour accéder au trésor (objet), assurant ainsi l'unicité et le contrôle d'accès, tout comme le design pattern Singleton.

## Structure
La classe Singleton déclare une méthode `getInstance` statique et renvoie la même instance de sa propre classe. Le code client ne peut pas référencer les constructeurs singleton. Seule la méthode `getInstance` doit autoriser l'accès aux objets Singleton.
![Image montrant la structure de singleton](https://refactoring.guru/images/patterns/diagrams/singleton/structure-fr.png?id=c61f45af3dee82ffdbe7a737fa33efa3)

## Exemple de Code Singleton en Java

Dans cet exemple, la classe ```GameManager``` est le Singleton. Cette classe n'a pas de constructeur public, vous ne pouvez y accéder que grâce à la méthode `getInstance`. On peut remarquer que la classe ```Player``` peut acceder a la classe GameManager et a ses variables/methodes static sans créer de nouvelle instance, il suffit d'utiliser la methode ```getInstance```. L'etat de GameManager est donc conserver entre chaque acccès par les autres classes, il y a une unique classe ```GameManager```

```java
// On a ici la class GameManger qui permet le controller l'etat d'un jeu
public class GameManager {

    // La variable instance du singleton est déclarer ici en static 
    private static GameManager instance;
    private static GameState gameState = GameState.MENU;

    // Ce getter permet d'acceder a l'instance de la classe ou de crer l'instance si elle n'existe pas
    public static GameManager getInstance() {
        if (instance == null) { // L'instance n'existe pas (est null)
            instance = new GameManager(); // Declaration de la variable instance
        }
        return instance; // On renvoie l'instance de la classe 
    }

    // Getter qui nous permet de connaitre l'avancement du jeu
    public static GameState getGameState() {
        return gameState;
    }
}

public class Player {
    
    public void takeDamage(int damage) {
        GameManager gameManager = GameManager.getInstance(); // On recupere l'instance de la classe GameManager
        GameState gameState = gameManager.getGameState();
        
        if(gameState == GameState.PLAYING) {
            // Take damage
        }else{
            return;
        }
    }
}



```
## Utilisation de Singleton

### Gestion des configurations
Utiliser un Singleton pour stocker et fournir l'accès aux configurations de l'application, garantissant ainsi qu'il n'y a qu'une seule source de configurations.

### Connexion à une base de données
Pour éviter la création de plusieurs connexions à une base de données, un Singleton peut être utilisé pour gérer l'instance de connexion, assurant ainsi une utilisation efficace des ressources.

### Gestion des connexions réseau
Pour des applications qui nécessitent une seule instance pour gérer les connexions réseau, le Singleton offre une solution en évitant la surutilisation des ressources réseau.

## Avantages et Inconvénients du Singleton

### Avantages du Singleton

1. **Contrôle de l'instance unique :** Assure qu'une classe n'a qu'une seule instance, fournissant ainsi un point d'accès global à cette instance.
2. **Accès global :** Facilite l'accès à l'instance unique depuis n'importe quel point de l'application, ce qui peut être pratique pour des ressources partagées.
3. **Initialisation tardive (Lazy Loading) :** Permet l'initialisation de l'instance unique uniquement lorsque cela est nécessaire, ce qui peut améliorer les performances lors du démarrage de l'application.

### Inconvénients du Singleton

1. **Testabilité :** La présence d'un Singleton peut rendre le code moins testable, car il peut être difficile de remplacer l'instance unique par une version mock ou de test.
2. **Violation du principe d'ouverture/fermeture :** Peut rendre difficile l'extension de certaines classes, car l'accès à l'instance unique est souvent codé en dur dans le code existant.
3. **Problèmes de multithreading :** Dans des environnements multithreadés, des problèmes de concurrence peuvent survenir si des précautions appropriées ne sont pas prises lors de l'accès et de la création de l'instance unique.

</br> </br> </br> </br>
# Façade

## Intention

La Façade agit comme une façade (d'où le nom) qui expose une interface simplifiée aux clients tout en gérant les interactions complexes avec les composants du sous-système. Cela facilite l'interaction des clients avec le système, car ils n'ont plus besoin de connaître les détails internes et les subtilités du sous-système.

## Résolution de Problèmes

### Complexité du Système
Lorsque votre système est composé de nombreux sous-systèmes interdépendants et que l'interaction directe avec ces sous-systèmes est complexe, la Façade offre une interface simplifiée pour l'utilisation du système global.

### Dépendances Client
Si les clients sont fortement couplés avec les détails d'implémentation du système, tout changement interne peut avoir un impact important sur les clients. La Façade réduit cette dépendance en fournissant une interface stable.

### Séparation des Responsabilités
La Façade permet de déplacer la complexité de la gestion des interactions entre les composants du sous-système à l'intérieur de la façade, aidant ainsi à maintenir une structure de code plus claire et à séparer les responsabilités.

## Solution

Une façade se révèle très pratique si votre application n’a besoin que d’une partie des fonctionnalités d’une librairie sophistiquée parmi les nombreuses qu’elle propose. Par exemple, une application qui envoie des petites vidéos de chats comiques sur des réseaux sociaux peut potentiellement utiliser une librairie de conversion vidéo professionnelle. Mais la seule chose dont vous ayez réellement besoin, c’est d’une classe dotée d’une méthode `encoder(fichier, format)`. Après avoir créé cette classe et intégré la librairie de conversion vidéo, votre façade est opérationnelle.

## Analogie

Imaginons que vous entrez dans un restaurant sophistiqué pour commander un repas. À première vue, vous ne voyez que le serveur, qui agit comme une sorte de façade pour l'ensemble du processus culinaire qui se déroule en cuisine.

- **La Cuisine (Sous-système complexe) :** La cuisine représente le sous-système complexe avec ses chefs, équipements, et processus de cuisson. C'est là que toute la magie de la préparation des repas se produit, mais en tant que client, vous n'avez pas besoin de connaître les détails de chaque étape.

- **Le Serveur (Facade) :** Le serveur agit comme une façade. Il prend votre commande, la transmet à la cuisine, gère les détails complexes tels que la coordination des plats, et finalement vous présente votre repas. Vous interagissez principalement avec le serveur, qui simplifie votre expérience de restauration.

- **Votre Interaction (Client) :** En tant que client, vous n'avez pas à vous soucier des détails complexes de la cuisine. Vous communiquez simplement avec le serveur en utilisant une interface simple (menu), et le serveur se charge de faire le lien avec le sous-système complexe en cuisine.

Dans cette analogie, la Façade (le serveur) simplifie votre interaction avec un système complexe (la cuisine) en fournissant une interface unifiée et conviviale. Vous n'avez pas besoin de connaître les détails internes de la préparation des plats, car le serveur gère cela pour vous. De même, dans le design pattern de Façade en programmation, la façade simplifie l'interaction avec un système complexe en fournissant une interface simplifiée aux clients.

## Structure
![Image montrant la structure de facade](https://refactoring.guru/images/patterns/diagrams/facade/structure.png?id=258401362234ac77a2aaf1cde62339e7)

1. **Accès Pratique aux Différentes Parties des Fonctionnalités du Sous-système :**
   - Façade offre un accès pratique aux différentes parties des fonctionnalités du sous-système. Elle sait où orienter les besoins de ses clients et comment utiliser les différentes pièces mobiles.

2. **Classe Façade Supplémentaire :**
   - Une classe Façade supplémentaire peut être créée afin de prévenir toute complexité excessive dans une façade existante. Cette nouvelle façade additionnelle peut inclure des fonctionnalités supplémentaires sans surcharger la première, et elle peut être utilisée à la fois par le client directement et par d'autres façades.

3. **Sous-système Complexe :**
   - Le sous-système complexe est constitué d'une multitude d'objets divers. Afin de leur conférer une véritable utilité, il est nécessaire de se plonger dans les détails de l'implémentation du sous-système, tels que l'initialisation des objets dans un ordre précis et la fourniture des données dans un format approprié.
   - Les classes du sous-système ne sont pas conscientes de l'existence de la façade. Elles fonctionnent et interagissent directement à l'intérieur de leur propre système.

4. **Utilisation de la Façade par le Client :**
   - Le client utilise la façade pour interagir avec le sous-système au lieu d'appeler directement les objets du sous-système.
  
## Exemple de Code Facade

Dans cet exemple la classe ```ClientAchatFacade``` utlise le design patter **façade**. Cette classe permet de simplifier l'achat d'un produit en une methode qui appelle tous les sous composants necessaire a l'achat. L'utilisation de la façade permet aussi de ne pas dupliquer de code si on a besoin d'effectuer un paiment depuis des classes différentes. La façade permet également qu'un utilisateur puisse avoir avoir aux sous composants utilisés pour l'achat.

```java

public class ClientAchatFacade {
    
    public void achatProduit(String idClient, String idProduit){
        Client client = Database.getInstance().getClient(idClient);
        Produit produit = Database.getInstance().getProduit(idProduit);
        
        if(client.getSolde() >= produit.getPrix()){
            client.setSolde(client.getSolde() - produit.getPrix());
            System.out.println("Achat effectué");
        }else{
            System.out.println("Solde insuffisant");
        }
    }
}

public class Client {

    private double solde;

    public Client(){
        this.solde = 0;
    }

    public double getSolde() {
        return solde;
    }

    public void setSolde(double solde) {
        this.solde = solde;
    }
}


public class Produit {

    private double prix;

    public Produit(){
        this.prix = 0;
    }

    public double getPrix() {
        return prix;
    }
}


public class Main {

    public static void main(String[] args) {
        ClientAchatFacade clientAchatFacade = new ClientAchatFacade();
        clientAchatFacade.achatProduit("32D10", "52");
    }
}
```

## Utilisation de Facade

La **façade** est une conception permettant de simplifier l'interaction avec un sous-système complexe en offrant une interface plus conviviale pour le code client. Elle agit comme un intermédiaire en redirigeant les appels du code client vers les objets du sous-système. Voici comment créer et mettre en œuvre une façade :

1. **Vérification de la possibilité d'offrir une interface plus simple que celle du sous-système.**

2. **Création et implémentation de cette interface comme une nouvelle façade, redirigeant les appels du code client vers les objets du sous-système.**

3. **Gestion de l'initialisation du sous-système et de son cycle de vie, si le code client ne le fait pas déjà.**

4. **Assurer que toute communication du code client avec le sous-système passe par la façade pour protéger contre les effets de bord en cas de modifications du sous-système.**

5. **En cas d'expansion excessive, envisager de déplacer des comportements vers une nouvelle façade spécialisée pour maintenir la clarté de la structure.**

## Avantages de la Façade :

- **Simplification de l'Interface :** Fournit une interface unifiée et conviviale.
  
- **Réduction de la Complexité :** Masque les détails internes, facilitant l'utilisation du système.

- **Dépendance Réduite :** Les clients n'ont besoin que de connaître l'interface de la façade.

- **Facilité de Maintenance :** Les modifications internes au sous-système n'affectent pas les clients.

## Inconvénients de la Façade :

- **Complexité de la Façade :** Risque de devenir complexe si surchargée.

- **Surcoût Initial :** La création peut entraîner un surcoût initial, mais cela est souvent compensé par une facilité de maintenance à long terme.

  # Sources
  (https://refactoring.guru/fr/design-patterns/facade)
  (https://fr.wikipedia.org/wiki/Singleton_(patron_de_conception))
  (https://refactoring.guru/fr/design-patterns/singleton)   





