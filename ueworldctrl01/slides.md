---
theme: seriph
title: Title
# canvasWidth: 1280
aspectRatio: 16/9
defaults:
    layout: center-bg
    background: /background-1.png
    class: text-white
routerMode: hash
fit: false
---

# Contrôles virtuels (en World-Space)
___

---

## Préparations du projet
___

<br />

- Pour commencer, vérifier que vous avez la structure de dossier que voici:

<br />

- /Content/

- /Content/Project/

- /Content/Project/Blueprints

- /Content/Project/Maps

- /Content/Project/Materials

- /Content/Project/Meshes

- /Content/Project/Textures

<br />

Créez les dossiers exactement comme décrit ici si nécessaire.

<br />


---

## Exemple pilote: Création d'un bouton 
___

---

### Création des meshes nécessaire
___

#### Préparation du support bouton
___

<br />

Pour commencer, créez une nouvelle map, et sauvegardez la dans :

/Content/Project/Maps

<br />

Aussi, vous pouvez déjà créer les dossiers suivants:

- /Content/Project/Blueprints/Button01WS/

- /Content/Project/Meshes/Button01WS/

<br />


---

##### Création du mesh
___

<br />

Maintenant, nous allons créer une box de 10cm x 2.5cm x 10cm à l'origin (0,0,0)

<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/buttonholder_create01.png" width="75%" />

</div>

<br /> 


---

##### Positionnement du point de pivot
___

<br />

Ensuite, nous allons régler son pivot au centre arrière de la box.

=> Appliquez un offset de -1.25,0,5 sur le pivot comme ceci:

<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/buttonholder_transform01.png" width="75%" />

</div>

<br /> 


---

##### Récupération du chemin du mesh généré
___

<br />

Pour terminer, on va trouver, renommer et déplacer le mesh qu'on vient de créer afin de pouvoir le réutiliser facilement.

Cliquez sur l'actor créé dans la scène si celui-ci n'est pas déjà sélectionné, ensuite dans les details:

On va sur la variable StaticMesh voir le path de notre mesh :


<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/generatedmesh_path.png" width="35%" />

</div>

<br /> 


---

##### Déplacement et renommage
___

<br />

Une fois le chemin trouvé, on ouvre le content browser et on navigue vers le path.

<br />

Dans le dossier, on localise le fichier, puis bouton droit -> rename

Et on donne un nom de mesh explicite; 

Pour cela, basez-vous sur la convention officielle:

<br />

https://dev.epicgames.com/documentation/unreal-engine/recommended-asset-naming-conventions-in-unreal-engine-projects?lang=en-US

<br />

Pour exemple, j'ai nommé le mien SM_ButtonHolder_WS_01.

<br />

Ensuite, on drag & drop (move) ce fichier dans: /Content/Project/Meshes/ButtonWS01/


---

#### Préparation du Button
___

<br />

Maintenant que le support du button est créé, recommençons l'operation.

<br />

Mais cette fois nous allons créé le button.

<br />

- Dimensions: 8 cm x 1cm x 8cm
- Pivot positionné de la même manière, au centre arrière du bouton.
- Mesh nommé: SM_Button_WS_01 dans le même dossier.


---

#### Vérification dans la scène
___

<br />

Maintenant que vos deux mesh sont créé, nous pouvons vérifier dans la scène si tout se positionne correctement:

- Cliquez sur l'actor contenant le ButtonHolder, et repositionnez le à l'origine si ce n'est pas déjà le cas
- Cliquez sur l'actor contenant le Button, et positionnez le à 2.5,0,0 (l'origine + l'épaisseur du ButtonHolder)
- Cachez temporairement le Floor de la scène si il y en a un.

Cela doit maintenant ressembler à ceci:

<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/buttonandholder_test01.png" width="80%" />

</div>

<br /> 

---

#### Vérification du dossier.
___

<br />

Et votre dossier Button01WS contenant les meshes devrait ressembler à ceci:

<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/sm_buttonws01_folder01.png" width="50%" />

</div>

<br /> 

Uniquement quand c'est ok, vous pouvez supprimer ces 2 actors que l'on a créés temporairement dans votre scène. Nous n'en aurons plus besoin.


---

### Création de l'Actor principal
___

<br />

Créons maintenant un nouvel actor dans le dossier /Content/Project/Blueprints/Button01WS/

<br />

Nommez le fichier par exemple: BP_Button_WS_01.

Ouvrez ensuite le fichier, et le Blueprint Editor s'affiche.

<br />

Dans l'éditeur, avant de commencer à programmer notre Blueprint, nous allons ajouter les 2 StaticMeshs que nous venons de créer, afin qu'ils fassent partie de l'actor.

<br />

Localisez le Tab Component (en haut à gauche), positionnez la selection sur le DefaultSceneRoot, cliquez sur le bouton +ADD et sélectionnez StaticMesh.


<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/add_sm01.png" width="20%" />

</div>

<br /> 

Ceci ajoute le component choisi (StaticMesh) dans le component sélectionné (DefaultSceneRoot).


---

#### Ajout du SM Component pour le ButtonHolder
___

<br />

Renommez le component StaticMesh en HolderStaticMesh.

Sélectionnez ensuite ce component HolderStaticMesh, et dans les détails, allez ensuite assigner le mesh SM_ButtonHolder_WS_01 que nous avons créé:

<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/add_buttonholder01.png" width="75%" />

</div>

<br /> 

---

#### Ajout du SM Component pour le Button
___

<br />

Recommencez maintenant l'opération pour le Button:

- Placez un comp StaticMesh dans le DefaultSceneRoot.
- Nommez le component ButtonStaticMesh;
- Assignez le mesh SM_Button_WS_01.
- Positionnez le à 2.5,0,0 (l'origine + l'épaisseur du ButtonHolder)

<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/add_buttoncomp01.png" width="75%" />

</div>

<br /> 

---

#### Réglages du Component
___

<br />

Ici, on a le choix entre réagir aux collisions, ou à ce qu'on appelle aux Overlaps (Triggers).

- Typiquement, les collisions sont bloquantes, ont un "Hit" d'impact (le point de contact) et génèrent des rebonds.

- Les overlaps eux ne sont pas bloquants, ne génèrent pas de rebonds, et réagissent dès que les surfaces se chevauchent.

<br />

Par simplicité nous allons partir sur la config overlaps:

- Sur le SM Comp du button de notre Actor, dans les détails, "Collision Preset", on choisi "OverlapAllDynamic".

- On coche aussi la case "Generate Overlap events" si celle-ci n'est pas déjà cochée.

---

#### Collision settings
___

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/smcomp_colsettings.png" width="50%" />

</div>

<br /> 


---

#### Ajout d'un collider
___

Afin que notre Button puisse recevoir des collisions, on a besoin de lui associer un mesh similaire mais simplifié, spécifique prévu à cet effet.

On termine donc par ajouter un mesh de collision a notre mesh SM_Button_WS_01:

- Ouvrez le Content browser, et dans votre dossier meshes, ouvre le fichier (asset) SM_Button_WS_01.

- Une fois dans l'éditeur de SM_Button_WS_01, dans la barre de menu en haut, tab "Collision", cliquez sur "Add Box Simplified Collision".

<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/add_button_col.png" width="50%" />

</div>

<br /> 

---

#### Réagir aux overlaps via la partie Blueprint de notre actor
___

<br />

Maintenant que notre button à le setup pour recevoir des collisions, on va essayer de les intercepter.

- (Ré)ouvrez le blueprint de notre actor, le BP_Button_WS_01.

<br />

Pour commencer, on va avoir besoin de préciser qu'**on veut réagir quand un overlap se produit**, et pour cela on va créer un "événement".

<br />

- On sélectionne le ButtonStaticMesh comp., et dans les détails, tout en bas dans Events, on clique sur le le + à droite de "On Component Begin Overlap"

Ceci ajoute un node "OnComponentBeginOverlap(ButtonStaticMesh)" dans votre EventGraph, c'est votre point d'entrée:

=> D'un qu'un overlap se produit dans notre ButtonStaticMesh, cet événement va déclencher dans votre EventGraph

=> Il devient dès lors possible de réagir à cela quand cela arrive, en ajoutant du code après cet Event.


---

#### OnComponentBeginOverlap
___

<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/onbegin_overlap.png" width="95%" />

</div>

<br /> 

---

#### Ajout d'une réaction simple à l'événement overlap
___

Afin de ne pas trop complexifier l'exercice, dans un premier temps, on peut simplement ajouter un log en cas d'overlaps.

Pour cela:

- Dans l'EventGraph de notre actor, ajouter un node PrintText. (Bouton droit dans le graph, et tapper le nom dans la recherche)

- Une fois le node placé, il faut relier sa broche d'entrée "Exec" (flèche blanche à gauche), à la broche de sortie de notre Event OnComponentBeginOverlap.

- Ensuite, nous pouvons changer le text affiché par le PrintText par quelque chose de plus explicite et pratique pour nous débugger.

- Pour se faire on va ajouter un node FormatText, qu'on reliera à l'entrée Text de notre PrintText.

- Ensuite, dans le champ text, on va utiliser une formule spéciale que voici: "Comp: {0} is overlapped by Actor: {1}, Comp: {2}".

- Cette formule va créer une pin d'entrée différente pour chaque chiffre x entouré d'accolades: "{x}"

- Reliez ensuite les 3 premières pin de chaque nodes (l'event et le FormatText) ensemble.

<br /> 

<div style="display: flex; justify-content: center; ">

 <img src="/logonoverlap.png" width="60%" />

</div>

<br /> 

---

#### Félicitations! 
___

<br />

Votre Button est normalement prêt pour réagir aux overlaps!

<br />

Par contre comment tester ?

---

### VR Template (OpenXR)
___

<br />

Afin de tester notre Button, on va utiliser le VRTemplate de sorte à avoir un joueur (VRPawn) déjà prêt à l'emploi.

=> Ceci nous évitera de devoir coder le Pawn et le player controller, et nous concentrer sur notre objectif ici, les overlaps!

<br />

- Importez le VRTemplate dans votre projet (si celui ci n'est pas déjà présent), via le Content Drawer -> Add -> "Add Feature or Content Pack..."

- Une fois le template installé, ouvrez la map du template: "VRTemplateMap". (/Content/VRTemplate/Maps/)

- Et sauvegardez directement une copie de la map dans votre répertoire /Content/Project/Maps.

- Ensuite, dans la map, cliquez sur le bouton + pour ajouter un actor, et choisissez votre actor BP_Button_WS_01.

- Positionnez le bouton contre un mur, face avant du bouton visible.

<br />

---

#### Premier test in-game

<br />

Vous pouvez maintenant connecter le Quest Link et tester. :)

<div style="display: flex; justify-content: center; ">

 <img src="/bullet_overlap.jpg" width="60%" />

</div>

<br /> 

Le Button est bien déclenché quand on tire dedans, ou quand on utilise un objet...

... Mais pas avec nos mains.

---

#### Edition du VRPawn
___

<br />

##### Duplication du Pawn actuel
___

<br />

Nous allons donc modifier le VR Pawn afin de le rendre plus pratique pour nos test.

- Localiser l'actor (/Content/VRTemplate/Blueprints).

- Et faites-en une copie dans votre dossier Blueprints (/Content/Project/Blueprints).

- Renommez le un peu différemment de sorte à pouvoir l'identifier facilement: VRPawn_01

<br />

Maintenant, il faut assigner ce nouveau Pawn à la place de l'autre dans le GameMode du projet.

- Vous pouvez utiliser le raccourci dans WorldSettings -> SelectedGameMode -> DefaultPawnClass.

<div style="display: flex; justify-content: center; ">

 <img src="/worldsettings_pawn.png" width="25%" />

</div>

- Ou encore, directement dans le fichier (asset).

*Vous pouvez aussi décider de créer un nouveau GameMode, et de l'assigner dans les project settings.*


---

##### Ajout de collider(s)
___

<br />

Ouvrez maintenant l'éditeur de votre nouveau VRPawn_01.

- Ensuite, dans le menu Components, sélectionnez VROrigin (DefaultSceneRoot)

- Ajoutez un component SphereCollider, nommez le RightIndexCollider.

- Dans les details, faite un reset complet du transform et appliquez un radius de 1.0.

- Ensuite, dans la partie Collision, cochez "Generate Overlap Events" et le preset "OverlapAllDynamics"

(Comme précédemment pour notre Button)

- Répètez ensuite l'operation entièrement afin d'ajouter un deuxième collider, le LeftIndexCollider.

<div style="display: flex; justify-content: center; ">

 <img src="/addedcolliders_pawn.png" width="25%" />

</div>


---

##### Ajout d'un Slot dans le rig.
___

<br />

Afin de placer automatiquement notre collider au bon endroit, on va installer un slot prévu à cet effet.

- Ouvrez l'asset /Content/Characters/MannequinsXR/Meshes/SK_MannequinsXR

- Dans la liste des bones à gauche, localisez l'index droit: "index_03_r"

- Ensuite Click droit dessus, Add socket.

- Ajoutez un offset de -2.0 sur l'axe Z (Relative Location)

Recommencez l'operation pour l'index gauche, et sauvegardez.

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/sk_mannequinsxr_slots.png" width="66%" />

</div>

---

##### Attachement du component en Blueprint
___

<br />

Maintenant on peut ouvrir l'EventGraph de notre VRPawn_01.

- Dans le OnBegin, si on observe la sequence du début, on tombe très vite sur un node "Sequence".

- Dans ce node Sequence, on clique sur "Add Pin".

- Cela ajoute une nouvelle Pin de sortie, de là on tire une node et on choisit: "Attach Component to Component (RightIndexCollision)"

- On vérifie que RightIndexCollision est bien le target, et on assign "Right Hand" à la broche parent. (drag&drop)

- Comme Socket Name, on met le nom de socket créé dans notre rig: "index_03_rSocket".

- Optionnellement, on peut ajouter un branch qui renvoie un PrintString (de debug) quand return est true.

---

###### Le Blueprint.
___

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/attachcomps.png" width="90%" />

</div>


---

### Ajout d'un feedback visuel
___

<br />

Afin d'avoir un meilleur retour visuel, nous allons permettre au bouton de bouger.

<br />

=> Pour cela, on va quand même devoir faire quelques modifications dans notre actor:

*(La collision actuelle est directement placée sur le mesh du Button, cela va poser problème pour la détection.)*

<br />

- On supprime le mesh de collision qu'on a ajouté précédemment directement dans l'asset SM_Button_WS_01.

- Maintenant, on va rajouter un component BoxCollider dans notre actor, et configurer son transform pour qu'il remplace parfaitement le précédent, et qu'il épouse la forme du bouton. 

- Et pour terminer dans sa partie Event, et re-ajoute les events "OnComponentBeginOverlap" et "OnComponentEndOverlap".

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/button_boxcol.png" width="60%" />

</div>

---

#### Création du mécanisme de déclenchement.
___

<br />

Maintenant on retourne dans l'EventGraph.

<br />

L'idée ici maintenant est de pouvoir animer le bouton pendant l'overlap.

=> Les Events overlaps ne se déclenchant qu'une seule fois (au début et à la fin de l'overlap), nous allons donc faire cette animation en dehors des ces Events, pendant l'EventTick de l'actor.

<br />

- Pour cela on va créer une variable "CurrentOverlapComp" (de type PrimitiveComponent).

- Maintenant, on va "mettre en cache" OtherComponent dans cette variable dans le OnComponentBeginOverlap.

- Et dans le OnComponentEndOverlap, on vide la variable afin de supprimer le cache.

- Pour terminer, dans l'EventTick, on va déjà ajouter un if (branch), et on utilise comme condition CurrentOverlapComp -> IsValid. sans rien brancher derrière pour l'instant.

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/onoverlap_tick.png" width="25%" />

</div>


---

#### Sécurisation du mécanisme.
___

<br />

Avant d'aller plus loin, on va un peu sécuriser le mécanisme:

<br />

- Dans OnBeginComponentOverlap, tout au début, rajoutez un Branch (condition: CurrentOverlapComp -> IsValid -> NOT ! )

=> On block tout overlap éventuel lorsque le système est déjà enclenché.

<br />

- Dans OnEndComponentOverlap, tout au début, rajoutez un Branch (condition: CurrentOverlapComp -> Equal <- Other Comp)

=> On ne valide la fin du processus que lorsqu'on est sûr que c'est bien la fin de notre overlap.

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/onoverlap_tick_validation.png" width="90%" />

</div>


---

#### Calcul de l'enfoncement du Button
___

<br />

On va maintenant créer une nouvelle Function dont le rôle va être de trouver la position de l'objet (provoquant l'overlap) par rapport au bouton.

- Créez une nouvelle Function et nommez là "GetComponentButtonSpaceLocation".

- A l'intérieur de cette fonction, sur le node d'entrée, on va ajouter un input "OtherComp" (de type primitive component).

- Par commodité, on crée aussi une variable local OtherComponent du même type, et directement en début de function on met en cache OtherComp.

OtherComp pouvant être n'importe quoi, ne sachant pas si il y a un Socket dedans, on va devoir s'adapter.

- Directement après le Set OtherComponent, on ajoute un Branch (Condition: OtherComponent -> Get All Socket Names -> Is Not Empty).

- On va avoir maintenant besoin d'un Node Return, donc re-sélectionnez votre function, et ajoutez un Output de type Float.

- Si votre button a le même setup que l'exercice, sa face avant pointe vers X, donc nommez votre nouvel output "X".

- Branchez un node Return derrière le True, et un derrière le False de votre Branch, laissez X vide pour l'instant.

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/check4sockets.png" width="80%" />

</div>

---

#### World-Space, Object-Space ?
___

<br />

Effectivement, maintenant on va devoir prendre la position de OtherComponent, qui est donnée en World-Space (en comptant l'origin à 0,0,0 et sans rotation.), et convertir cette position en Object(Button)-Space; En tenant compte de la position et de l'orientation du Button, là ou il est placé.

<br />

Pour comprendre, regardez simplement autour de vous, vous vous situez dans un bâtiment, à l'intérieur d'une pièce.

<br />

Ce Bâtiment et cette pièce finalement on une localisation et orientation fixe (par rapport à la terre, dont l'origine est 0,0,0)

<br />

Vous, en contrepartie, même si vous savez que vous pouvez vous rendre à une localisation précise dans ce monde (via des coordonnées), de votre point de vue, votre espace, commence juste à partir de vous même.

<br />

De ce fait, si on veux connaître les coordonnées exacte d'un objet que vous décrivez comme se situant à 1 mètre de distance devant vous, il convient de déjà savoir ou vous vous trouvez dans le monde à cet instant précis, et quelle orientation vous avez.

<br />

Aussi, peut-être que votre perception des choses est altérée; et peut-être voyez-vous les choses en grand et que 1 Mètre devant vous, au fait ce n'est peut être que 75cm, ou encore inversement à 1.5M
=> La notion de scale. (qui est appliqué aussi dans le calcul)


---

#### Conversion de la position en Object(Button)-Space
___

<br />

Essayons cela:

- Ajoutez deux nodes "Get Actor Transform" dans la function, un sera pour les sockets, l'autre pour quand il n'y en a pas.

- De leurs broche de sortie, ajoutez un node "Inverse Transform Location"; et le Transform prend place dans son input "T".

<br />

Ensuite on va devoir fournir la position pour chacune des parties.

Ajoutez un node Get OtherComponent pour chaque, et ensuite de ce node:

- Pour la partie socket: OtherComponent -> "Get Attach Socket Name" -> "Get Socket Location" (Dans "In Socket Name", et OtherComponent dans Target) -> Location

- Pour la partie sans socket: OtherComponent -> "Get World Location" -> Location

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/buttonspace_conv.png" width="40%" />

</div>


---

#### Extraction de la valeur de distance
___

<br />

Maintenant qu'on a notre localisation en Object-space, on va récupérer uniquement la valeur qui nous intéresse:

- De chaque sorties (Vector3) des nodes "Invert Transform Location" -> Break Vector.

- Ensuite, on relie la broche d'output "X" de la partie Socket à la broche d'input "X" du "Return" qu'on a placé fin de Branch (coté True).

- Et on fait pareil avec la partie Component dans l'autre "Return" (coté False).

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/get_compobjspace_loc.png" width="50%" />

</div>

Une fois terminé, vous pouvez ajouter cette function dans l'EventTick de notre actor, juste après le Branch qu'on avait ajouté précédemment (partie True).


---

#### Animation/déplacement du mesh
___

<br />

Maintenant on va créer une autre function "MoveButton".

Son rôle va être de prendre la distance de notre objet comme point de référence pour adapter la position de notre bouton en temp-réel, et dans un range délimité. 

- Créez la function, et directement ajouter un input pin "XPosition" (de type float).

- Ajoutez un node "(Get)ButtonStaticMesh" et de là -> Set Relative Location; ButtonStaticMesh prend la place dans son input "Target".

- De l'input pin "Location", tirez vers la gauche et placez un node "MakeVector".

- Reliez le paramètre d'entrée "XPosition" à la broche "X" du make Vector.

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/setbuttonloc.png" width="50%" />

</div>

---

#### Ajout d'un offset et délimitation du range
___

<br />

On va maintenant ajouter un réglage d'offset (permettant de compenser la taille du collider), et délimiter un range pour notre button.

- Ajoutez un paramètre d'entrée "Offset" (type float), un "MinValue" (de type float), et un MaxValue (de type float).

- Du paramètre "XPosition", ajoutez un node "Subtract", ensuite tirer un lien du paramètre "Offset" au 2eme pin du node "Subtract".

- Ensuite de la sortie de la soustraction, ajoutez un Node "Clamp(float)"", et vérifiez bien que le résultat de la soustraction arrive dans "Value".

- Reliez le paramètre "MinValue" au node "Clamp", broche "Min", et "MaxValue" à la broche "Max".

- Remplacez "XPosition" par le résultat du "Clamp" à la broche "X" du "MakeVector".

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/movebutton.png" width="75%" />

</div>

---

#### Implémentation dans l'EventTick
___

<br />

Maintenant qu'on a nos deux functions prête, on peut ajouter la deuxième (MoveButton), à la suite de la première dans l'EventTick.

- On relie l'output "X", à l'input "XPosition" de "MoveButton".

- Pour les 3 autres inputs "Offset", "MinValue" et "MaxValue", on tire vers la gauche et on click sur PromoteToVariable.

- On peut renommer "Offset" en "CollisionOffset", "MinValue" en "ButtonMinPosition", "MaxValue" en "ButtonDefaultPosition".

<br />

Pour terminer on va s'asssurer que le mesh reprend toujours sa position intiale à la fin de l'opération:

- A la sortie du Branch dans OnEndComponentOverlap, on re-ajoute un "Set Relative Location" (Target: ButtonStaticMesh, Location -> MakeVector -> X -> ButtonDefaultPosition)


<br />

<div style="display: flex; justify-content: center; ">

 <img src="/movebutton_tick.png" width="50%" />

</div>

---

#### Ajout d'une réaction au bouton
___

<br />

Rendez-vous dans la méthode MoveButton:

- Ajoutez un paramètre d'input "PressValue" (type float) dans la function.

- Ajoutez un node "Less Equal", et reliez la sortie de la soustraction à la première pin.

- Reliez "PressValue" à la deuxième pin.

- Ajoutez un paramètre d'output "Pressed" (type bool) dans la function, cela va ajouter un node "Return".

- Reliez la sortie du "LessEqual", au node "Return".

- Reliez la sortie du "Set Relative Position" au node "Return".

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/pressvalue.png" width="75%" />

</div>


---

#### Modification de l'EventGraph
___

<br />

Maintenant que notre méthode MoveButton nous renvoi un état Pressed, on va adapter le graph.

- Créons une variable "bIsPressed" (type boolean).

On ne va pas directement assigner son état en sortie de function Move (il suffirait de lire son état).

A la place, on va uniquement assigner son état, quand le bouton est pressé, et que la valeur actuelle ne l'est pas.

- De la broche output "Pressed", ajoutez un node "AND".

- Ensuite placez un "(Get) bIsPressed", puis -> NOT -> et on relie au pin 2 du AND.

- On relie l'output du AND dans un nouveau Branch qu'on place après le MoveButton

- On termine par faire un reset de "bIsPressed" dans le "EndComponentOverlap", entre l'output de "Set Relative Position" et le "(set) CurrentOverlapComp ".

<div style="display: flex; justify-content: center; ">

 <img src="/bispressed.png" width="50%" />

</div>
---

#### Ajout d'Events
___

<br />


---

## Autres contrôles possibles
___

- Création d'un keypad
- Création d'un écran tactile.
- Création d'un slider
- Création d'un potentiomètre 
- Création d'un levier
- Création d'une manivelle

___

<br />


---
