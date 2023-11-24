# Wiki-design-pattern
Wiki des designs patterns

# Singleton #

## Intention
L'intention principale du Singleton est de garantir qu'il n'existe qu'une seule instance d'une classe particulière, et de fournir un moyen d'accéder à cette instance à partir de n'importe quel point du programme. Cela peut être utile dans des situations où une seule instance de la classe est suffisante pour coordonner les actions à travers le système, comme la gestion des ressources partagées ou la configuration globale. Le pattern Singleton garantit que la classe n'est instanciée qu'une seule fois en fournissant une méthode qui crée l'instance si elle n'existe pas, sinon, renvoie simplement l'instance existante. De plus, il restreint l'accès aux méthodes de création d'instance, assurant ainsi que la seule instance existante est accessible de manière contrôlée.

## Résolution de problème
- **Garantir une seule instance :** Son fonctionnement est le suivant : vous créez un objet, mais après un certain temps, vous décidez d’en créer un autre. Plutôt que de vous retrouver avec un objet flambant neuf, vous récupérez celui qui existe déjà. Il est impossible d’implémenter ce comportement avec un constructeur normal, puisqu’un constructeur doit théoriquement toujours retourner un nouvel objet.

- **Accès global :** Le Singleton fournit un point d'accès global à cette unique instance. Cela simplifie l'accès à cette instance depuis n'importe quelle partie du code, évitant ainsi la nécessité de passer explicitement l'instance entre différentes parties du programme. Cependant, il protège son instance et l’empêche d’être modifiée.

- **Initialisation paresseuse :** Certains Singleton sont conçus pour une initialisation paresseuse, où une seule instance est créée uniquement lorsqu'elle est réellement nécessaire. Cela évite une initialisation coûteuse au démarrage du programme et améliore les performances.

## Solution
1. Limiter l'accès à l'opérateur `new` en déclarant le constructeur par défaut en tant que privé, empêchant ainsi d'autres objets d'invoquer directement la création d'instances de la classe Singleton.
2. Introduire une méthode statique de création agissant comme un constructeur, qui en interne utilise le constructeur privé pour instancier un objet et le stocke dans une variable statique. Tous les appels ultérieurs à cette méthode renvoient l'objet mis en mémoire cache.
3. Si votre code a accès à la classe d'un singleton, vous pouvez appeler ses méthodes statiques. Le même objet est toujours renvoyé à chaque appel de cette méthode.

## Analogie
Imaginons un roi dans un royaume qui veut garantir qu'il n'y ait qu'un seul trésor national. Le roi crée une clé spéciale qu'il garde toujours avec lui. Quand quelqu'un a besoin d'accéder au trésor, il doit demander la clé au roi. Si la clé n'existe pas encore, le roi la crée ; sinon, il la partage. Ainsi, il n'y a qu'une seule clé (instance) pour accéder au trésor (objet), assurant ainsi l'unicité et le contrôle d'accès, tout comme le design pattern Singleton.

## Structure
La classe Singleton déclare une méthode `getInstance` statique et renvoie la même instance de sa propre classe. Le code client ne peut pas référencer les constructeurs singleton. Seule la méthode `getInstance` doit autoriser l'accès aux objets Singleton.
![Image montrant la structure de singleton](https://refactoring.guru/images/patterns/diagrams/singleton/structure-fr.png?id=c61f45af3dee82ffdbe7a737fa33efa3)

## Exemple de Code Singleton en Java

Dans cet exemple, la classe de la connexion à la base de données est le Singleton. Cette classe n'a pas de constructeur public, vous ne pouvez y accéder que grâce à la méthode `getInstance`. Cette méthode met en cache le premier objet créé puis retourne ce même objet lors des appels ultérieurs.

```java
public class DatabaseConnection {
    private static DatabaseConnection instance;

    // Constructeur privé pour empêcher l'instanciation directe depuis l'extérieur
    private DatabaseConnection() {
        // Initialisation de la connexion à la base de données
    }

    // Méthode statique de création d'instance
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }

    // Autres méthodes de gestion de la connexion à la base de données
    // ...

    public void executeQuery(String query) {
        // Implémentation de l'exécution de la requête
    }
}
