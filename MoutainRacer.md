# Mountain Racer

*Credits: [GameDev.tv](https://www.gamedev.tv/)*

Nous allons créer un Side Scroller où une voiture fait la course dans les montagnes.

## Objectifs

- Nous avons une voiture qui avance de gauche à droite et qui peut tourner vers l'avant ou l'arrière.
- Nous allons implémenter des effets avec des systèmes de particules.
- Nous allons rajouter de la musique et des sons à nos effets.

## Mécanismes du jeu

- La voiture avance toute seule de gauche à droite.
- Le joueur peut augmenter ou diminuer la vitesse de la voiture.
- La voiture peut tourner vers l'avant ou l'arrière.
- La voiture laisse une traînée de poussière quand elle touche le sol.
- Le jeu recommence quand la voiture atteint le point d'arrivée ou quand elle se renverse.
- Il y a une musique de fond.
- Il y a des effets sonores quand la voiture atteint le point d'arrivée ou se renverse.

## Sprite Shape

1. Créer un **2D Object** > **Sprite Shape** > **Closed Shape** que vous pouvez renommer **Terrain**.

2. Ajouter un tag **Ground** à **Terrain**.

3. Créer un dossier **Sprites** dans **Assets** où vous allez mettre les sprites du projets.

4. Dans **Sprites**, créer un **2D** > **Sprite Shape Profile** que vous pouvez renommer **TerrainProfile**.

5. Changer le profile de **Terrain** dans **Sprite Shape Controller** en **TerrainProfile**.

6. Dans **TerrainProfile**, vous pouvez remplacer **Sprite Shape Edge** avec la texture que vous voulez sur les bords de votre terrain.

7. Remplacer **Sprite Shape Fill** pour que le terrain soit homogène.

8. Dans **Terrain** > **Sprite Shape Controller**, vous pouvez faire **Edit Spline** pour modifier le terrain et faire votre niveau.

## Edge Collider

9. Ajouter la composante **Edge Collider 2D** à **Terrain**.

10. Dans **Sprite Shape Controller** > **Collider**, modifier la valeur de **Offset** si nécessaire pour coller les bords du **Edge Collider 2** à la texture de votre terrain.

## Player

11. Ajouter des assets de voiture à votre scène avec des roues et le corps de la voiture séparés mais groupé sous un parent **RaceCar** avec le tag **Player**.

12. Ajouter Collider et Rigidbody aux roues et un Trigger au haut de la voiture.

## Cinemachine FollowCamera

13. Dans **Window** > **Package Manager** > **Packages:Unity Registry**, installer **Cinemachine**.

14. Dans **Hierarchy**, créer un **Cinemachine** > **Virtual Camera** et renommer le **Virtual Follow Camera**.

15. Dans la composante **CinemachineVirtualCamera**, changer **Body** à **Framing Transposer** et **Follow** à notre objet **RaceCar**.

16. Vous pouvez changer **Screen X** et **Screen Y** dans **Body** pour modifier la position de l'écran par rapport à la position de **RaceCar**.

## Surface Effector 2D

17. Ajouter une composante **Surface Effector 2D** à **Terrain** et cocher **Edge Collider 2D** > **Used By Effector**.

18. Dans **Surface Effector 2D** > **Force**, changer **Speed** et **Force Scale** et observer les interactions entre **RaceCar** et **Terrain**.

## AddTorque()

19. Créer un dossier **Scripts** dans **Assets**.

20. Créer un script **PlayerController** à ajouter-le à **RaceCar**.

21. Dans **PlayerController**, créer une variable `Rigidbody2D rigidbody2D`.

22. Dans `Start()`, affecter à rigidbody2D la composante **Rigidbody 2D** de **RaceCar** grâce à `GetComponent<Rigidbody2D>()`.

Nous voulons permettre au joueur de faire tourner la voiture vers l'avant ou l'arrière quand il appuie sur les flèches gauche et droite.

23. Dans `Update()`, ajouter la condition suivante.
```{code} csharp
if(Input.GetKey(KeyCode.LeftArrow)){
    rigidbody2D.AddTorque(torqueAmount);
}
```
où `[SerializeField] float torqueAmount = 1f` est défini auparavant.

Observer les interactions de **RaceCar** avec **Terrain** quand vous appuyez sur la flèche gauche.

24. Faire de même avec `RightArrow` pour permettre à la voiture de tourner vers l'avant.

25. Vous pouvez maintenant jouer avec les valeurs de **Linear Drag** et **Angular Drag** dans les Colliders de **RaceCar**, **Speed** et **Force Scale** dans **Surface Effector 2D** de **Terrain** ainsi que `torqueAmount` dans **PlayerController** pour avoir des interactions qui vous plaisent.

## SceneManager et Invoke

Nous voulons que le jeu recommence quand la voiture atteint la ligne d'arrivée.

26. Ajouter un rectangle Trigger qui représente la ligne d'arrivée et renommer le **Finish Line**. 

27. Créer un script **FinishLine** et rajouter-le comme composante à **Finish Line**.

28. Dans **File** > **Build Settings**, vérifier le numéro en face de votre scène. Par défaut, il doit être `0`.

29. Dans **FinishLine**, ajouter `using UnityEngine.SceneManagement` au début du script.

30. Avec `OnTriggerEnter2D(Collider2D other)`, nous pouvons ajouter `SceneManager.LoadScene(0)` quand l'objet taggé **Player** arrive en contact avec notre Trigger.

Pour améliorer l'expérience du joueur, nous allons ajouter un délai quand le jeu recommence au lieu de recommencer instantanément.

31. Factoriser `SceneManager.LoadScene(0)` dans une méthode `void ReloadScene()`.

32. Dans `OnTriggerEnter2D`, appeler `ReloadScene` avec un délai grâce à `Invoke("ReloadScene",reloadDelay)` où `[SerializeField] float reloadDelay = 1f` est déclaré auparavant.

33. Créer un script **CrashDetector** attaché au haut de la voiture avec le Trigger et implémenter la Game Loop de la même façon quand la voiture est renversée (le haut de la voiture touche le sol).

## Particle System

34. Dans **Hierarchy**, cliquer droit puis **Effects** > **Particle System**.

Nous allons créer un effet de feu d'artifice à la ligne d'arrivée.

35. Renommer le **Particle System** à **Firework** et ajouter-le en tant qu'enfant de **Finish Line**.

:::{seealso} Enfant vs Composante
:class: dropdown
Le but d'avoir **Particle System** en tant qu'enfant de **Finish Line** au lieu d'une composante de **Finish Line** nous permet de déplacer le centre de l'émetteur de particules là où on le veut.

Par exemple, des feux d'artifices peuvent être plus haut dans le ciel au lieu d'être au sol où se trouve la ligne d'arrivée.
:::

36. Jouer avec les valeurs du **Particle System** et les modules **Emission**, **Shape**, **Renderer** pour avoir l'effet que vous voulez.

37. Pour déclencher l'effet, ajouter `[SerializeField] ParticleSystem finishEffect` qui va prendre comme valeur (grâce à **Inspector**) le système de particule que vous avez défini. Puis, ajouter `finishEffect.Play()` juste avant de faire `Invoke("ReloadScene",reloadDelay)`.

:::{warning} Play On Awake
Il faut décocher **Play On Awake** dans **Particle System** de **FinishEffect** pour ne pas déclencher l'effet au début du jeu.
:::

38. Faire de même pour avoir des effets de particules quand la voiture se renverse.

39. Créer un **Particle System** qui va représenter la traînée de poussière laissé par la voiture. 

40. Créer un script **DustTrail** qui déclenche la traînée de poussière seulement quand la voiture touche le sol (pas quand elle est en l'air).

:::{hint} Indice
:class: dropdown
Vous pouvez récupérer le **ParticleSystem** avec `[SerializeField] ParticleSystem dust` par exemple.

Nous pouvons déclencher l'effet avec `dust.Play()` dans `OnCollisionEnter2D` et l'arrêter avec `dust.Stop()` dans `OnCollisionExit2D`.
:::

## public GameObject et FindObjectOfType

Nous voulons pouvoir modifier la vitesse de la voiture avec les flèches haut et bas. Notre problème est que la vitesse de la voiture devrait être gérée par **PlayerController** mais en réalité, cette implémentation est dans **Surface Effector 2D** de **Terrain**.

Nous voulons donc récupérer la composante **Surface Effector 2D** de **Terrain** pour pouvoir modifier ces valeurs depuis **PlayerController**.

Il existe deux façons de le faire:
- Avec `public GameObject terrain`, vous pouvez récupérer la référence vers **Terrain** dans **PlayerController** grâce à **Inspector**. Puis, il suffit de faire `GetComponent` pour accéder à `SurfaceEffector2D` avec une variable `surfaceEffector2D`.
- Vu qu'il y a une seule composante de type `SurfaceEffector2D`, vous pouvez aussi récupérer cette composante directement avec `FindObjectOfType<SurfaceEffector2D>()`.

41. Utiliser une des deux façons pour récupérer la composante **Surface Effector 2D** dans **PlayerController**.

:::{warning} public vs SerializeField
`public` permet aussi de modifier la valeur d'une variable directement dans **Inspector**. Pourtant, il est préférable d'utiliser `[SerializeField]` pour garder les variables utilisées `private`.
:::

42. Dans `Update` (de **PlayerController**), refactoriser le code de la rotation (vers l'avant et l'arrière) du joueur dans une méthode `void RotatePlayer()` et ajouter une méthode `void BoostPlayer()` où nous allons implémenter l'accélération (et la décélération) de vitesse de la voiture avec les touches `UpArrow` et `DownArrow`.

Nous allons avoir seulement deux vitesses (vous pouvez implémenter une vrai accélération ou décélération si vous voulez), soit `baseSpeed` et `boostSpeed`.

43. Dans `BoostPlayer()`, changer `surfaceEffector2D.speed` quand les touches correspondantes sont appuyées.

Nous voulons désactiver les inputs du joueur quand la voiture se renverse. 

44. Dans **PlayerController**, implémenter une méthode `public void DisableInputs()` qui va désactiver `RotatePlayer` et `BoostPlayer` dans `Update` (grâce à un `bool`).

45. Dans **CrashDetector**, appeler `DisableInputs` quand la voiture se renverse.

## Audio

46. Créer **Audio Source** dans **Hierarchy** et renommer le **Background Music**.

47. Vous pouvez ajouter une musique libre de droits dans **AudioClip** et cocher **Loop** pour l'avoir comme musique de fond.

Maintenant, nous voulons des effets sonores à l'arrivée et quand la voiture se renverse.

48. Ajouter une composante **Audio Source** à **Finish Line** avec l'effet sonore de votre choix.

49. Dans le script **FinishLine**, récupérer la composante `AudioSource` et fait `Play()` (ou `PlayOneShot()`) au bon moment pour déclencher l'effet.

:::{seealso} Plusieurs effets sonores sur le même objet
:class: dropdown
Pour avoir plusieurs effets sonores sur le même objet, vous pouvez aussi créer plusieurs variables `AudioClip` (correpondantes aux différents effets) et les rendre `[SerializeField]` pour récupérer les clips. Puis, avec une composante `AudioSource` sans `AudioClip`, vous pouvez appeler `Play` (ou `PlayOneShot`) avec comme argument le clip correspondant.
:::

50. Ajouter un effet sonore quand la voiture se renverse.

## Conclusion

Vous avez fini **Moutain Racer**. Vous pouvez maintenant passer au jeu suivant !