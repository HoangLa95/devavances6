# Choose Your 2D Game

Vous pouvez choisir entre l'une des catégories suivantes pour votre jeu.

## Top-down Games

:::{important} Jeux en vue du dessus
:class: dropdown
Un jeu vidéo en vue du dessus est un jeu dans lequel le gameplay est perçu depuis une caméra en vue à vol d'oiseau.
Les personnages peuvent se déplacer dans toutes les directions.
:::

### Jeux de rôle

:::{figure} ./images/pokemon.png
:alt: Pokémon Émeraude
:align: center

Credits: [DeviantArt](https://www.deviantart.com/lazzxion-keyblade/art/Pokemon-Emerald-Shimmer-WIP-343764514)
:::

### Jeux de tir

:::{figure} ./images/vampire-survivor.jpg
:alt: Vampire Survivor
:align: center

Credits: [Steam](https://store.steampowered.com/app/1794680/Vampire_Survivors/)
:::

## Side Scroller

:::{important} Jeux à défilement horizontal 
:class: dropdown
Un jeu vidéo à défilement horizontal est un jeu dans lequel le gameplay est perçu depuis une caméra en vue de côté.

Les personnages se déplacent souvent de gauche à droite.
:::

### Jeux de tir

:::{figure} ./images/contra.png
:alt: Contra
:align: center

Credits: [DeviantArt](https://www.deviantart.com/luis-mortalkombat14/art/super-contra-st3-2D-434478041)
:::

### Jeux de course

:::{figure} ./images/racing.jpg
:alt: Racing Game
:align: center

Credits: [UnityAssetStore](https://assetstore.unity.com/packages/templates/packs/2d-racing-game-76989/reviews)
:::

### Jeux de plateforme

:::{figure} ./images/mario.jpg
:alt: Mario
:align: center

Credits: [DeviantArt](https://www.deviantart.com/ah-noniamkerd/favourites/77948447/new-super-mario-princess-peach-ds-and-3d-land?page=44)
:::

## Game Design

:::{important} TODO  
1. **Outil de développement** : Unity 2D
2. **Ressources** : Assets gratuits sur [UnityStore](assetstore.unity.com) ou que vous pouvez créer vous-même.
3. **Document de Design** : 
    - *Introduction* : Histoire, idée derrière le jeu.
    - *Genre* : Votre choix parmi les types de jeux proposés.
    - *Inspiration* : Donner un (ou plusieurs) exemples de jeux qui vous a inspiré.
    - *Mécanismes de bases du jeu* : Choix de design (héro, ennemis, items, niveaux, contrôles...)
    - *Checklist* : Le minimum nécessaire que vous devez développer pour avoir un produit viable puis les bonus si vous avez le temps.
:::

:::{seealso} Un court exemple
- **Introduction** : Un héro doit tuer des démons pour sauver le monde.
- **Genre** : Top-down Bullet Hell.
- **Inspiration** : Vampire Survivor.
- **Mécanismes de bases du jeu** :
    - *Héro* : Le héro tire dans toutes les directions et possède des caractéristiques comme la vitesse et la fréquence de tir.
    - *Ennemis* : Un ennemi de base, un plus rapide, un qui inflige des dégâts au fil du temps et un qui inflige des dégâts avec zone d'effet.
    - *Items* : Un item qui augmente la vitesse, un qui augmente la fréquence de tir.
    - *Niveaux* : Quatres vagues d'ennemis différents.
    - *Contrôles* : WASD
- **Checklist** : Minimum de 1 à 7, bonus à partir de 8 
    1. Créer le menu principal.
    2. Héro (art, mouvement).
    3. Tir dans les huits directions (art, son).
    4. Vague 1 : ennemi de base (art, mouvement).
    5. Interaction héro-ennemi : prise de dégât héro/ennemi (effet spécial, animation, son).
    6. Menu de pause.
    7. Musique.
    8. Vague 2 : ennemi rapide.
    9. Item qui augmente la vitesse.
    10. Item qui augmente la fréquence de tir.
    11. Vague 3 : ennemi avec DoT (Damage over Time).
    12. Vague 4 : Ennemi avec AoE (Area of Effect).
:::