# All In One

*Credits: [GameDev.tv](https://www.gamedev.tv/)*

Nous allons créer un Platformer générique avec un personnage qui collectionne des pièces et tire sur des ennemis pour arriver à la sortie d'un donjon.

## Objectifs

Nous allons apprendre à :
- Faire des tilemaps avec des layers.
- Créer des animations.
- Utiliser des Prefabs.
- Utiliser l'Input System.
- Implémenter les movements dans un Platformer.
- Utiliser la caméra dynamique de Cinemachine.
- Utiliser Physics Material (Friction et Bounciness).
- Instancier des objets (comme des bullet d'un gun).
- Gérer la persistance des données (points de vie, score).
- Faire un Start Menu et un Pause Menu.

## Mécanismes du jeu

Nous avons :
- un personnage qui peut se déplacer de gauche à droite, sauter et monter à une échelle.
- des ennemis avec un mouvement simple (aller-retour sur une plateforme).
- des pièges dans la map.
- des items qui font augmenter notre score.
- une sortie à chaque niveau.

:::{important} Avant de commencer à travailler
Penser à organiser votre dossier **Asset** en créant des dossiers différents pour les différents objets que nous allons utiliser. Par exemple, pour ce jeu, nous allons avoir besoin de : **Animations**, **Input System**, **Materials**, **Prefabs**, **Scenes**, **Scripts**, **Sprites** et **Tiles**.
:::

## Sprite Editor et Sprite Sheets

1. Trouver des Sprites pour un plateformer : animation pour un personnage 2D, des tiles pour les plateformes, des éléments pour l'arrière-plan, des animations pour un ennemi, une échelle, des pics, un item (une pièce par exemple), un balle (bullet) et une porte de sortie.

2. Suivre le tutoriel [Introduction to Sprite Editor and Sheets](https://learn.unity.com/tutorial/introduction-to-sprite-editor-and-sheets) et découper vos Sprites.

## Tilemaps

3. Suivre le tutoriel [Introduction to tilemaps](https://learn.unity.com/tutorial/introduction-to-tilemaps) et créer (**2D Object** > **Tilemap** > **Rectangular**) un **Platforms Tilemap** avec comme parent **Tilemap Grid**.

### Rule Tile

4. (Optionnel) Pour faciliter la création des niveaux, vous pouvez apprendre à utiliser un [Rule Tile](https://learn.unity.com/tutorial/using-rule-tiles).

## Layers

5. Dans l'Inspector de **Platforms Tilemap**, cliquer sur **Layer** puis ajouter une **Layer**. Dans **User Layer**, ajouter **Platforms** comme **User Layer** à partir de **User layer 6**. Ajouter aussi une **Sorting Layer** avec le même nom (**Platforms**). Il ne faut pas oublier d'ajouter ces layers à **Platforms Tilemap** dans **Layer** (**User Layer**) et **Sorting Layer**.

:::{important} Sorting Layer vs User Layer
**Sorting Layer** sert à l'affichages des sprites : plus le numéro d'un layer est grand, plus l'objet sur le layer serait "devant". Par exemple, **Sorting Layer 1** sera **Background** et **Sorting Layer 2** sera **Platforms**.

**User Layer** va être utilisé pour gérer la logique des interactions (collisions) des objets dans le jeu. Par exemple, pour ce jeu, nous allons créer les layers suivant: **Ground**, **Climbing**, **Player**, **Background**, **Bouncing**, **Enemies**, **Bullet**, **Interactables**. L'ordre ici n'a pas d'importance.

Une fois que les User Layers sont créées, nous pouvons aller dans **Edit** > **Project Settings** > **Physics 2D** > **Layer Collision Matrix** et décider les interactions entre les objets des différentes layers.

Par exemple :
- **Background** ne devrait pas interagir avec les autres éléments donc nous pouvons tout décocher dans sa ligne et colonne. 
- **Interactables** correspondent à des objets qui vont juste interagir avec **Player** donc cela sera la seule case cochée dans sa ligne (et colonne).
- **Enemies** doit pouvoir détecter **Ground** et **Player**.
Vous pouvez décider les interactions entre les layers qui correspondent à ce qui est attendu dans la suite du jeu.
:::

6. Créer **Background Tilemap** avec **Background** comme **User Layer** et **Sorting Layer**. Si vous voulez faire un effet parallaxe alors au lieu de créer une tilemap pour le background, vous pouvez suivre [ce tutoriel](https://www.youtube.com/watch?v=zit45k6CUMk) (*Credits : Dani*) plus tard (une fois que la [caméra dynamique cinemachine](#state-driven-cameras) soit établie).

## Animator Controller

7. Créer un objet **Player** avec les bonnes layers et ajouter un **Sprite Renderer** avec un sprite du personnage.

8. Suivre le tutoriel [Animator Controller](https://learn.unity.com/tutorial/animator-controllers-2019-3).

9. Créer un **Animator Controller** **Player** et attacher cette composante à l'objet **Player**.

10. Choisir les Sprites qui correspondent à l'animation **Idle** de votre personnage et créer un animation. Cocher **Loop Time** pour que l'animation se répète infiniment.

11. Glisser l'animation **Idle** dans l'**Animator Controller** **Player**. Vérifier que le personnage est bien animé.

12. Créer l'animation **Running** et ajouter-la à l'**Animator Controller**.

13. Dans l'onglet **Parameters** de **Animator**, créer un booléen `isRunning`.

14. Cliquer droit sur **Idle** et créer une transition vers **Running**. 

15. Dans la partie **Conditions** de la transition, ajouter la condition `isRunning` à `true` pour indiquer que nous devons aller de l'animation **Idle** à l'animation **Running** quand le personnage `isRunning`.

16. Créer la transition de **Running** vers **Idle** et tester les animations en mode Play en cochant et décochant `isRunning` dans Parameters.

:::{seealso} Transition settings
:class: dropdown
Les settings d'une transition permet de faire du *blending* de deux animations (rendre le passage d'une animation vers une autre plus naturel).

Vous pouvez jouer avec ces settings et observer les changements dans les animations. Des settings simples quand nous n'avons pas besoin de faire de blending est de tout mettre à 0 et décocher **Has Exit Time**.
:::

17. De la même façon, créer une animation **Climbing** et ajouter-la au controller avec les bonnes transitions et un paramètre `isClimbing`.

## Prefabs

18. Suivre les tutoriels [Prefabs](https://learn.unity.com/tutorial/prefabs-e) et [Introduction to Nested Prefabs](https://learn.unity.com/tutorial/introduction-to-nested-prefabs).

19. Sauvegarder notre objet **Player** comme **Prefab**.

## Composite Collider

20. Ajouter **Tilemap Collider 2D** et **Composite Collider 2D** à **Platforms Tilemap** puis cocher **Used by Composite** dans **Tilemap Collider 2D** pour avoir un seul collider pour les plateformes.

21. Changer **Body Type** dans **Rigidbody 2D** de **Platforms Tilemap** à **Static** pour que les plateformes restent figées dans notre monde.

22. Ajouter les composantes **Capsule Collider 2D** et **Rigidbody 2D** à **Player**.

23. Modifier **Capsule Collider 2D** pour que le collider colle bien au personnage.

24. Dans **Rigidbody 2D** > **Constraints**, cocher **Freeze Rotation Z** pour que le personnage ne puisse pas tomber vers l'avant (ou l'arrière).

## Input System

25. Dans **Window** > **Package Manager**, chercher **Input System** dans **Unity Registry** et installer le package **Input System**.

26. Ajouter la composante **Player Input** à **Player**.

27. Cliquer sur **Create Actions** et créer **InputActions** puis suivre le tutoriel [Customizing New Input Actions](https://learn.unity.com/tutorial/customizing-new-input-actions). Pour ce jeu, nous allons juste besoin de **Move**, **Fire**, et **Jump** et des contrôles avec des touches de clavier.

:::{seealso} Player Input Behavior Send Messages
Dans la composante **Player Input**, vous pouvez voir les fonctions qui serons appelées quand le joueur appuie sur les touches correspondantes comme `OnMove`, `OnJump` et `OnFire`.
:::

## Move

28. Créer un script **PlayerMovement** et ajouter-le à **Player**.

29. Dans **PlayerMovement**, nous allons commencer avec le code suivant.

```{code} csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerMovement : MonoBehaviour
{
    [SerializeField] float runSpeed = 10f;
    Vector2 moveInput;
    Rigidbody2D myRigidbody2D;

    void Start(){
        myRigidbody2D = GetComponent<Rigidbody2D>();
    }

    void Update(){
        Run();
    }

    void OnMove(InputValue value){
        moveInput = value.Get<Vector2>();
    }

    void Run(){
        Vector2 playerVelocity = new Vector2 (moveInput.x * runSpeed, myRigidbody2D.velocity.y);
        myRigidbody2D.velocity = playerVelocity;
    }
}
```
- `runSpeed` va nous permettre de modifier la vitesse du movement de **Player**.
- `moveInput` va récupérer la valeur de la touche sur laquelle le joueur a appuyé lors de `OnMove` : droite = (1,0), gauche = (-1,0), haut = (0,1), bas = (0,-1).
- À chaque `Update`, nous allons faire `Run` qui appliquer une vitesse horizontale (`moveInput.x * runSpeed`) et qui ne va pas changer la vitesse verticale (`myRigidbody2D.velocity.y`) de **Player**.

Tester les mouvements horizontaux de **Player**.

## Flip Player Sprite

Nous voulons que **Player** "se tourne" vers la gauche ou la droite selon la direction dans laquelle il bouge. Pour cela, il faut changer **Scale** (à -1) dans la composante **Transform** de **Player** (quand il tourne vers la gauche, 1 sinon).

30. Récupérer la composante **Transform** de **Player** dans **PlayerMovement** et nommer la `myTransform`.

31. Ajouter une fonction `FlipSprite` dans `Update`.

32. Utiliser le code suivant pour `FlipSprite`.

```{code} csharp
    void FlipSprite(){
        bool playerHasHorizontalSpeed = Mathf.Abs(myRigidbody2D.velocity.x) > Mathf.Epsilon;
        if (playerHasHorizontalSpeed){
            myTransform.localScale = new Vector3 (Mathf.Sign(myRigidbody2D.velocity.x),1f,1f);
        }
    }
```
- `playerHasHorizontalSpeed` vérifie si **Player** a une vitesse horizontale en vérifiant si la valeur absolue de la vitesse sur l'axe x est plus grand que `Epsilon` (au lieu de 0 car une valeur très petite comme 0.0000001 peut être considérée comme nulle).
- `Mathf.Sign(myRigidbody2D.velocity.x)` retourne -1 quand le joueur se déplace vers la gauche, ce qui permet de flipper le sprite de **Player**.

## Animer les movements

Nous voulons que les bonnes animations soit utilisées quand **Player** fait les movements correspondant.

33. Récupérer la composante **Animator** de **Player** dans **PlayerMovement** et nommer la `myAnimator`.

34. Dans `Run`, après avoir modifié la vitesse de **Player**, vérifier si `playerHasHorizontalSpeed` comme avant et mettre le booléen `isRunning` de l'animator à la même valeur grâce à `myAnimator.SetBool("isRunning", playerHasHorizontalSpeed)`.

## Jump

Pour pouvoir sauter, il faut que les pieds du personnage touchent le sol. 

35. Créer un **Box Collider 2D** au niveau des pieds du personnage. Faire attention à ce que la largeur du rectangle ne dépasse pas la largeur du Capsule, sinon le personnage pourrait sauter quand il est contre un mur.

36. Ajouter la **User Layer Ground** à **Platforms Tilemap**.

37. Récupérer la composante `BoxCollider2D` dans **Player Movement** et nommer-la `myFeetCollider`.

38. Définir la fonction `isTouchingTheGround` de la façon suivante.

```{code} csharp
bool isTouchingTheGround(){
    return myFeetCollider.IsTouchingLayers(LayerMask.GetMask("Ground"));
}
```

39. Similairement à `OnMove`, dans `On Jump`. nous pouvons définir la vitesse verticale (`[SerializeField] float jumpSpeed`) de **Player** quand la touche correspondante est appuyée.

```{code} csharp
void OnJump(InputValue value){
    if(isTouchingTheGround() && value.isPressed) {
        myRigidbody2D.velocity += new Vector2 (0f, jumpSpeed);
    }
}
```

40. Changer **Gravity Scale** dans **Rigidbody 2D** de **Player** et `jumpSpeed` pour que le joueur puisse sauter le bon nombre (à vous de choisir) de tiles.

Pour éviter que **Player** se colle contre les murs, nous allons lui donner une **Physics Material 2D** sans friction.

41. Dans le dossier **Materials**, créer **2D** > **Physics Material 2D** et nommer-la **Player Material**.

42. Changer **Friction** et **Bounciness** à 0 dans **Player Material** et mettre **Player Material** dans les colliders et rigidbody de **Player**.

## Climb

43. Créer **Climbing Tilemap** avec des échelles et y mettre les bonnes layers (User Layer et Sorting Layer).

:::{seealso} Custom Physics Shape
:class: dropdown
Vous pouvez changer la forme du Tilemap Collider de vos échelles en allant dans **Sprite Editor** et en choisissant **Custom Physics Shape** (dans le dropdown menu en haut à gauche). 
:::

44. Ajouter les **Tilemap Collider 2D** et **Composite Collider 2D** à **Climbing Tilemap**. Il ne faut pas oublier de cocher **Static** et **Is Trigger** dans **Rigidbody 2D** et **Used by Composite** dans **Tilemap Collider 2D**.

Ici, **Is Trigger** permet au joueur de passer devant l'échelle sans se cogner contre.

Pour pouvoir monter à une échelle, il faut vérifier si le corps de **Player** touche la User Layer **Climbing**.

45. Comme avant, récupérer `CapsuleCollider2D myBodyCollider` dans **PlayerMovement** et implémenter `bool isTouchingALadder()`.

46. Dans `Update`, ajouter la méthode `ClimbLadder` que nous allons implémenter de la façon suivante.

```{code} csharp
void ClimbLadder(){
    if(isTouchingALadder()) {
        myRigidbody2D.gravityScale = 0f;

        Vector2 climbVelocity = new Vector2 (myRigidbody2D.velocity.x, moveInput.y * climbSpeed);
        myRigidbody2D.velocity = climbVelocity;

        bool playerHasVerticalSpeed = Mathf.Abs(myRigidbody2D.velocity.y) > Mathf.Epsilon;
        myAnimator.SetBool("isClimbing", playerHasVerticalSpeed || !isTouchingTheGround());
    }
    else{
        myRigidbody2D.gravityScale = gravityScaleAtStart;
        myAnimator.SetBool("isClimbing", false);
    }
}
```
- D'abord, il ne faut pas que **Player** tombe quand il est sur une échelle. Donc, il faut mettre `myRigidbody2D.gravityScale` à 0.
- Comme pour les movements horizontaux, nous pouvons définir la vitesse verticale `climbVelocity` qui va dépendre de `moveInput.y` et `[SerializeField] float climbSpeed`.
- Pour déclencher la bonne animation, nous allons vérifier si `playerHasVerticalSpeed` de la même façon que `playerHasHorizontalSpeed`.
- `isClimbing` est à `true` quand `playerHasVerticalSpeed` ou quand il est sur une échelle et ses pieds ne sont plus au sol (`!isTouchingTheGround`).
- Quand **Player** ne touche plus l'échelle, il faut remettre `myRigidbody2D.gravityScale`  à la gravité de départ `float gravityScaleAtStart` dont la valeur est affecté dans `Start` avec `gravityScaleAtStart = myRigidbody2D.gravityScale`.

## State-Driven Cameras

47. Commencer par créer une **Follow Camera** avec **Cinemachine** [comme dans le jeu précédent](./Game2_MountainRacer.md/#cinemachine-followcamera).

48. Pour avoir des mouvements de caméra plus smooth, vous pouvez jouer avec les paramètres comme **Dead Zone Width**, **Dead Zone Height**, **X Damping** et **Y Damping**.

:::{seealso} Camera Settings
**Dead Zone Width** et **Dead Zone Height** définissent une zone rectangulaire dans laquelle le joueur peut bouger sans que la caméra bouge.

**X Damping** et **Y Damping** rendent le suivi de la caméra plus lente pour éviter des mouvements erratiques.
:::

Vu que la caméra est centrée sur **Player**, elle peut "sortir du monde" que l'on a défini et le joueur pourrait voir du vide autour. Pour éviter cela, nous allons définir un **Confiner**.

49. Dans **Background Tilemap** (ou **Background**), ajouter une composante **Polygon Collider 2D** et définir la zone que la caméra peut voir. Faire attention à ce que **Background Tilemap** soit sur les bonnes layers et que cette (user) layer n'interagit pas avec les autres.

50. Dans la **Follow Camera**, ajouter l'extension **Cinemachine Confiner 2D** et choisir comme **Bounding Shape 2D** **Background Tilemap** (il va choisir automatiquement le Polygon Collider de Background Tilemap).

Maintenant, nous allons créer plusieurs types de caméras et des transitions entre les différents caméras pour donner un aspect plus dynamique à notre jeu.

Ces caméras vont dépendre de l'état de **Player**. Par exemple, quand **Player** est en train de courir, nous avons envie de dézoomer la caméra pour voir ce qui est devant lui. Quand il ne fait rien, nous voulons peut-être zoomer sur lui pour donner un peu de personnalité. Quand il monte à une échelle, nous voulons peut-être aussi dézoomer car il "voit" plus quand il est en hauteur.

51. Dans **Hierarchy**, créer **Cinemachine** > **State-Driven Camera**.

52. Mettre toutes les caméras sous un même objet **Cameras**.

53. Renommer **Follow Camera** à **Run Camera** et rendre-la enfant de **State Driven Camera**.

54. Dupliquer **Run Camera** deux fois et les renommer **Idle Camera** et **Climb Camera**.

55. Jouer avec le zoom de ces caméras (**Lens Ortho Size**) et d'autres paramètres (si vous voulez) pour implémenter les effets mentionnés précédemment.

56. Dans **State Driven Camera**, choisir **Player** comme **Animated Target**.

57. Dans la liste de **State**, ajouter les états **Idling**, **Running**, **Climbing** et les caméras correspondantes.

58. Le temps de transition entre deux caméras est en secondes dans **Default Blend**. Vous pouvez créer des **Custom Blends** pour personnaliser les temps de transitions. Par exemple, **Run Camera** vers **Idle Camera** peut prendre plus de temps alors que **Idle Camera** vers **Run Camera** peut être plus rapide.

## Physics Material 2D

Précédemment, nous avons utilisé **Physics Material 2D** pour enlever la friction de **Player**. Maintenant, nous pouvons aussi créer des trampolines dans notre niveau.

59. Choisir un asset pour votre trampoline et créer une **Bouncing Tilemap** avec les bonnes layers et colliders.

60. Créer un **Physics Material 2D** dans votre dossier **Materials** et mettre `1.5` en **Bounciness** pour commencer.

61. Ajouter-le à **Rigidbody 2D** de **Bouncing Tilemap** et adjuster les valeurs jusqu'à avoir un rebond qui vous convient.

## Enemy

62. Utiliser ce que vous avez appris pour créer un ennemi avec une animation simple.

63. Dans **Scripts**, créer **EnemyMovement** dans lequel nous allons définir `[SerializeField] float moveSpeed` et récupérer la composante `Rigidbody2D myRigidbody2D`.

64. Dans `Update`, modifier la vitesse horizontale dans `myRigidbody2D` avec `moveSpeed`.

Il faut maintenant permettre à **Enemy** de pouvoir faire des aller-retours. Pour cela, nous allons ajouter un "champs de vision" à **Enemy** qui sera un Box Trigger qui, dès qu'il ne "voit" plus **Ground**, il va se retourner.

65. Créer un **Box Collider 2D** avec **Is Trigger** devant **Enemy** qui descend un peu dans le sol.

66. Ajouter `OnTriggerExit2D(Collider2D other)` dans **EnemyMovement** qui change `moveSpeed` en `-moveSpeed` pour que **Enemy** se déplace dans l'autre direction.

67. Ajouter aussi une fonction `FlipEnemyFacing` qui change `localScale` de **Enemy** pour que le sprite tourne aussi dans la bonne direction.


## Start Menu et Pause Menu

Faire un **Start Menu** et un **Pause Menu** grâce à ces tutoriels : [START MENU in Unity](https://www.youtube.com/watch?v=zc8ac_qUXQY), [PAUSE MENU in Unity](https://www.youtube.com/watch?v=JivuXdrIHK0) (*Credits : Brackeys*). 