# Taxi Driver

*Credits: [GameDev.tv](https://www.gamedev.tv/)*

Nous allons créer un jeu Top Down simple où un taxi doit prendre et déposer des clients à leurs destinations. 

Créer un projet Unity2D Taxi Driver pour commencer.

## Objectifs

- Nous avons un taxi qui doit prendre des clients.
- Il y a des obstacles sur le chemin qui le ralentit en cas de collision.
- Il y a des items qu'il peut récupérer pour gagner en vitesse.
- Le taxi doit déposer un client à sa destination.
- Il doit déposer tous les clients pour gagner le jeu.

## Mécanismes du jeu

- Le taxi doit pouvoir avancer et reculer.
- Il doit pouvoir tourner à gauche et à droite.
- Sa vitesse augmente quand il récupère des items.
- Sa vitesse diminue quand il se cogne contre des obstacles.
- Quand il prend un client, le client "disparaît".
- Quand il arrive à la destination, il dépose le client avant de pouvoir prendre un autre client.
- Il change de couleur pour montrer son statut (occupé par un client ou pas).

## Un premier script

Suivre les tutoriels [Getting started with scripts](https://learn.unity.com/tutorial/get-started-with-scripts?uv=2021.3) et [Code in the default script](https://learn.unity.com/tutorial/code-in-the-default-script?uv=2021.3).

:::{important} Savez-vous faire les choses suivantes ?
- Créer un script.
- Le rajouter en tant que **Component** à des **GameObject**.
- Utiliser **Debug.Log()**.
- Reconnaître la différence entre les méthodes **Start** et **Update**.
:::

## transform.Rotate()

1. Créer un capsule qui va représenter le taxi.
2. Ajouter un script `Taxi` au capsule.
3. Faire tourner le taxi à chaque frame de 0.1 selon l'axe z  avec `transform.Rotate(0,0,0.1f)`.

:::{warning} `float` en Unity
Il faut rajouter `f` à la fin d'un `float` en Unity pour qu'il puisse le distinguer d'un `double`.
:::

## transform.Translate()

4. Faire avancer le capsule (selon l'axe y) de 0.01 avec `transform.Translate(0,0.01f,0)` à chaque frame.

## SerializeField

5. Créer des variables `float steerSpeed = 0.1f` et `float moveSpeed = 0.01f` et les remplacer les valeurs dans `transform.Rotate` et `transform.Translate` respectivement.

6. Ajouter `[SerializeField]` devant les variables de la façon suivante:
```{code} csharp
[SerializeField] float steerSpeed = 0.1f;
[SerializeField] float moveSpeed = 0.01f;
```

Maintenant, vous devez voir `steerSpeed` et `moveSpeed` directement dans `Inspector` dans l'éditeur de Unity. 

:::{warning} Modification des valeurs avec `SerializeField`
Ces valeurs peuvent être modifiées directement dans l'éditeur et seront sauvegardées dans votre scène mais ne vont pas être changées dans votre script.

Les valeurs adoptées par les GameObject sont les valeurs dans l'éditeur Unity.
:::

:::{danger} Modification de la scène en mode Play
Les modifications apportées à la scène quand vous êtes en mode Play disparaissent quand vous sortez du mode Play !

Une façon simple de se souvenir que nous sommes en mode Play est de changer la couleur de l'éditeur en mode Play avec **Edit** > **Preferences** > **Colors** > **Playmode tint**.
:::

## Input.GetAxis()

Nous voulons ajouter des inputs de l'utilisateur : le taxi doit pouvoir avancer, reculer et tourner avec des touches du clavier.

Vous pouvez vérifier (et changer) les inputs en allant dans **Edit** > **Project Settings** > **Input Manager**. Nous sommes interessés par la catégorie **Axes**.

7. Dans `Update()`, rajouter `float steerAmount = Input.GetAxis("Horizontal")` et remplacer `steerSpeed` dans `transform.Rotate` avec `steerAmount`. 

Essayer de faire tourner la voiture en mode Play.

8. Vous pouvez changer la direction dans laquelle la voiture tourne en rajoutant `-` devant `steerAmount`. 

9. Vous pouvez aussi changer la vitesse à laquelle elle tourne en multipliant par `steerSpeed`.

:::{hint} Votre code devrait ressembler à ...
:class: dropdown
```{code} csharp
float steerAmount = Input.GetAxis("Horizontal") * steerSpeed;
transform.Rotate(0,0,-steerAmount);
```
:::

10. Faire la même chose pour permettre au joueur d'avancer et de reculer avec les touches du clavier.

## Time.deltaTime

Les ordinateurs avec des puissances différentes ne prennent pas le même temps pour exécuter un frame. 

11. Multiplier les changements de vitesse avec `Time.deltaTime` pour que notre jeu soit cohérent sur n'importe quel ordinateur.

Changer `steerSpeed` et `moveSpeed` dans `Inspector` pour avoir des vitesses raisonnables.

## Collider et Rigidbody

12. Ajouter une composante **Capsule Collider 2D** à votre capsule. 

Vous pouvez observer une petite ligne verte qui délimite le capsule (décocher Sprite Renderer pour l'observer plus clairement).

13. Ajouter une composante **Rigidbody 2D** à votre capsule. 

Maintenant, il doit se comporter avec la physique par défaut de Unity.

14. Mettre **Gravity Scale** à 0 sinon le capsule va tomber quand vous faites Play.

15. Créer un obstacle avec une composante Collider.

Observer l'interaction entre la voiture (le capsule) et l'obstacle.

16. Ajouter une composante Rigidbody à l'obstacle.

Re-observer l'interaction entre les deux objets.

## OnCollisionEnter2D()

Nous allons rajouter un comportement "On Collision" à notre taxi.

17. Créer un nouveau script `Collision` et ajouter-le comme composante au capsule.

18. Dans `Collision`, ajouter la méthode `private void OnCollisionEnter2D(Collision2D other)`.

`OnCollisionEnter2D(Collision2D other)` est appelé par Unity quand notre objet 2D (qui possède le script comme composante) entre en collision avec un autre objet 2D (`other.GameObject`).

19. Ajouter un simple message dans `OnCollisionEnter2D` pour que le capsule affiche un message dans la console quand il se cogne contre un obstacle.

Observer les interactions entre la voiture et l'obstacle lors d'une collision.

## OnTriggerEnter2D()

Nous allons maintenant ajouter un "trigger".

20. Ajouter un carré avec un collider et cocher **Is Trigger**.

Observer la différence entre un trigger et un collider normal.

21. Dans `Collision`, ajouter la méthode `OnTriggerEnter2D` avec un message.

Observer les interactions entre la voiture et le trigger.

## Ajouter des Assets au projet

Quand nous parlons d'"assets" d'un projet, nous référons souvent aux images utilisés pour les objets dans le jeu.

22. Suivre le tutoriel [Importing 2D Assets into Unity](https://learn.unity.com/tutorial/importing-2d-assets-into-unity-2019-3).

:::{danger} Avant d'importer des assets gratuits sur [assetstore.unity.com](https://assetstore.unity.com/)
Il y a des assets qui viennent avec une scène par défaut souvent nommé SampleScene (similairement au scène par défaut de Unity).

**Renommer votre scène** avant d'importer des assets. Sinon, vous risquez d'écraser votre scène courante par une scène démo des assets importés.
:::

23. Vous pouvez maintenant importer des assets gratuits depuis [assetstore.unity.com](https://assetstore.unity.com/) pour votre voiture, des obstacles, l'environnement,...

24. Vous pouvez aussi utiliser des assets gratuits que vous avez créé ou que vous pouvez trouver sur Internet.

:::{seealso} Gestion des assets importés de Unity Store
La gestion des assets depuis [assetstore.unity.com](https://assetstore.unity.com/) peut être faite à partir de **Window** > **Package Manager**.
:::

## Design d'un niveau

25. Créer un niveau avec des assets gratuits :
    - Il faut avoir une voiture avec les scripts que nous avons écrit.
    - Il faut des obstacles qui ne bougent pas lors d'une collision (comme des maisons par exemple).
    - Il faut des obstacles qui peuvent être déplacés par la voiture lors d'une collision (comme des pierres par exemple).

## FollowCamera

Nous voulons une caméra qui suit notre taxi quand il se déplace.

26. Créer un script `FollowCamera` que nous allons ajouter en tant que composante à la caméra.

27. Ajouter une variable `[SerializeField] GameObject thingToFollow` dans `FollowCamera`.

`[SerializeField]` ici nous permet de choisir l'objet que nous voulons dans `Inspector` au lieu d'avoir un objet fixé défini dans le script.

28. Changer la position de la caméra dans `Update` grâce à `transform.position = thingToFollow.transform.position`.

Quel est le problème de ce code ?

:::{hint} Indice
:class: dropdown
Le 2D est trompeur car il nous fait oublier que nous travaillons en fait en 3D. 

En effet, nous ne voulons pas que la caméra soit exactement au même endroit que la voiture mais plutôt en vue Top-Down, c'est-à-dire que la caméra doit être plus "haut" et regarde la voiture en vol d'oiseau.

La solution est d'avoir les mêmes coordonnées x et y que la voiture pour la caméra mais de "reculer" pour avoir l'effet vol d'oiseau en faisant `-10` par exemple sur l'axe z.
```{code} csharp
transform.position = thingToFollow.transform.position + new Vector3(0,0,-10);
```

Vous pouvez bien sûr refactoriser ce code pour le rendre plus propre !
:::

29. Améliorer le code. 

Observer les changements. Est-ce que vous voyez un problème ?

:::{hint} Indice
:class: dropdown
Vu que nous n'avons pas défini d'ordre sur l'exécution des `Update`, parfois la voiture se déplace avant la caméra, pardois elle peut se déplacer après.

La caméra n'est donc pas très stable car elle doit "se battre" avec les mouvements de la voiture.

Vous pouvez consulter cette documentation sur l'[ordre d'exécution des évènements dans Unity](https://docs.unity3d.com/Manual/ExecutionOrder.html).

Pour rendre le mouvement de la caméra stable et cohérent, nous allons renommer `Update` dans `FollowCamera` à `LateUpdate`.

Les mouvements de la voigure s'effectueront toujours d'abord.
:::

30. Renommer `Update` dans `FollowCamera`.

## Tags

31. Créer un cercle trigger que nous allons appeler `Client`.

32. Créer un carré trigger que nous allons appeler `Destination`.

Nous pouvons ajouter des "tags" à nos GameObject dans Inspector. Il faut créer un nouveau tag quand ce n'est pas un tag par défaut proposé par Unity puis ajouter ce tag aux objets de notre choix.

33. Ajouter un tag à l'objet `Client` que nous allons aussi nommer `Client`. Faire la même chose pour `Destination`.

Nous voulons deux comportements différents pour le taxi quand il passe par un trigger client ou quand il passe par un trigger destination.

34. Afficher des messages différents dans la console quand le taxi est "triggered" par `Client` ou par `Destination` grâce à `other.tag == "Client"` (ou `"Destination"`).

Maintenant, nous voulons que le taxi prenne un client avant de le déposer à la destination.

35. Modifier le code pour que le message affiché à la destination soit triggered seulement quand le taxi a un client.

36. Ajouter un autre client au scène. Vous pouvez dupliquer un objet avec **Ctrl+D**.

Nous voulons aussi que le taxi ne puisse pas prendre plusieurs clients en même temps.

37. Modifier le code pour que le message affiché par un client ne s'affiche que si le taxi n'a pas déjà un client à bord.

## Destroy

Nous voulons qu'un client "disparaît" quand il est récupéré par le taxi.

38. Détruire l'objet client quand le taxi va le prendre grâce à `Destroy(other.gameObject,0.5f)`.

`0.5f` correspond au "time delay" que l'objet qui sera détruit va prendre avant de disparaître. Dans ce contexte, il s'agit du temps que le client va mettre pour rentrer dans la voiture.

Nous pouvons renommer `0.5f` en `float destroyDelay` et le rendre `[SerializeField]` pour pouvoir modifier ce temps plus facilement dans `Inspector`.

## GetComponent

Nous voulons que le taxi change de couleur quand il est occupé par un client.

39. Créer deux variables `[SerializeField]` de type `Color32` que nous allons initialiser à `new Color32(1,1,1,1)`. La première va correspondre à la couleur du taxi avec un client et la deuxième, sans client.

C'est un système de couleur RGB où les trois premiers arguments correspondent à Red, Green et Blue respectivement et le dernier à Alpha pour l'opacité.

Pour changer la couleur du taxi, nous allons changer plus précisément la couleur du sprite.

40. Créer une variable `SpriteRenderer spriteRenderer`.

41. Initialiser la variable dans `Start` grâce à `spriteRenderer = GetComponent<SpriteRenderer>()`.

42. Nous pouvons maintenant modifier `spiteRenderer.color` au bon moment pour changer les couleurs de la voiture de la bonne façon.

## Exercice

43. Utiliser ce que vous avez appris pour implémenter des items qui permet au taxi d'aller plus vite quand il les récupère et de ralentir quand il se cogne contre un obstacle. 

Ce code devrait se trouver dans le script `Taxi` qui modifie la vitesse du taxi.

## Conclusion

Vous avez fini **Taxi Driver** et vous avez appris les fondamentaux de Unity. Vous pouvez maintenant passer au jeu suivant !
