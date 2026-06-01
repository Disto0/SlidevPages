---
theme: seriph
title: Intro à la programmation
# canvasWidth: 1280
aspectRatio: 16/9
defaults:
    layout: center-bg
    background: /background-1.png
    class: text-white
routerMode: hash
fit: false
---

# Intro à la programmation

---

## Notions élémentaires

---

### Le hardware

<br />

#### Principe de base
___

<br />

Avant de commencer, il peut-être utile de s'intéresser au fonctionnement interne du hardware que l'on utilise.

Il est en effet souvent dommage de ne pas utiliser l'outil adapté a une tache précise, ou de mal utiliser l'outil nécessaire...

<br />

<div style="display: flex; justify-content: center; ">

 <img src="/chantier01.png" width="300" />

</div>

---

Il y a bien longtemps...

<br />

...Nous avons conçu l'architecture interne d'un ordinateur de cette manière...

---

<br />

##### Le duo de base: CPU - RAM
___

<br />

Ou plutôt: Cache - CPU - RAM

<br />

```mermaid {theme: 'neutral', scale: 0.66}
flowchart RL
C@{ shape: das, label: "Front Side Bus (FSB)" }
D@{ shape: diamond, label: "Mainboard chipset" }
E@{ shape: lin-cyl, label: "[Internal Storage] - High Capacity (TB) - Low Speed (MB/s)" }
F@{ shape: rect, label: "[CPU Internal Cache] - Low Capacity (KB) - High Speed (TB/S)" }

id1[["[Random Access Memory] - Medium Capacity (GB) & Speed (GB/s)"]]

D <--> O["Other devices"] 
C <--> D
D <--> E
B <--> C <--> id1
B["[CPU]"] <--> F

```

---

Observations
___

<br />

Si on observe un peu, on remarque que:

<br />

**Plus nous somme proche du processeur et plus la vitesse est élevée.**

En contrepartie la capacité est moindre.

<br />

**Plus on s'en éloigne**, plus la capacité augmente...

... mais **la vitesse diminue**.

<br />

Note: *Aussi lors d'un transfert quelconque, la vitesse (bande passante) maximum théoriquement atteignable, ne peut être au mieux, que la vitesse de l'élément le plus lent de la chaÎne. (Canaux de transport compris)*

<div style="display: flex; justify-content: center; ">

<img src="/bottleneck_pipeline_diagram.svg" width="512" />

</div>


---

De ce fait: 

- Le disque ayant la plus grande capacité, est utilisé pour le stockage, même si il est plus lent.

- Quand les données ont besoin d'être utilisées, le processeur les charge du disque dans la RAM, afin que les données soient disponible.

- Quand le processeur doit effectuer des calculs, si la taille des données n'est pas trop grande, elles peuvent être chargée dans le cache du cpu, là ou elles seront traitées plus rapidement.


---

##### L'arrivée des GPUs
___

<br />

Avec les années et l'arrivée de la 3D, l'accélération matérielle et en particulier, l'accélération 3D est apparue avec la venue des premiers processeurs graphiques externe.

Le CPU n'étant pas adapté pour le calcul en parallèle massif, un nouveau type de processeur à émergé.

<br />

Sans trop rentrer dans le vif du sujet, on notera tout de même la sortie des 3DFX Voodoo 1 et 2 (en 96 et 98)

<br />

Et Nvidia Geforce 256 (en 99), qui fut le premier GPU à supporter l'accélération Transform & Lighting, encore utilisée aujourd'hui.

(Avant cela, ces calculs étaient pris en charge de manière "software" par le CPU.)

<br />

Voir: https://www.hardware.fr/articles/53-3/transformation-lighting-bases.html

---

##### L'ajout du GPU
___

<br />

```mermaid {theme: 'neutral', scale: 0.66}
flowchart RL
C@{ shape: das, label: "Front Side Bus (FSB)" }
D@{ shape: diamond, label: "Mainboard chipset" }
E@{ shape: lin-cyl, label: "[Internal Storage] - High Capacity (TB) - Low Speed (MB/s)" }
F@{ shape: rect, label: "[CPU Internal Cache] - Low Capacity (KB) - High Speed (TB/S)" }
G@{ shape: rect, label: "[GPU]" }

id1[["[Random Access Memory] - Medium Capacity (GB) & Speed (GB/s)"]]
id2[["[Video RAM] - Medium Capacity (GB) & Speed (GB/s)"]]

D <--> O["Other devices"] 
C <--> D
D <--> E
D <--> G
G <--> id2
B <--> C <--> id1
B["[CPU]"] <--> F

```

On notera le "long parcourt" entre le disque, le cpu, la ram, et pour terminer la vram.

---

<br />

<br />

#### Evolution récente
___

<br />

```mermaid {theme: 'neutral', scale: 0.66}
flowchart RL
PCIe@{ shape: das, label: "PCIExpress" }
DS@{ shape: das, label: "Direct Storage" }
MBC@{ shape: diamond, label: "Mainboard chipset" }
S@{ shape: lin-cyl, label: "[Internal Storage] - High Capacity (TB) - Low Speed (MB/s)" }
L@{ shape: rect, label: "[CPU Internal Cache] - Low Capacity (KB) - High Speed (TB/S)" }
GPU@{ shape: rect, label: "[GPU]" }
RAM[["[RAM] - Medium Capacity (GB) & Speed (GB/s)"]]
VRAM[["[Video RAM] - Medium Capacity (GB) & Speed (GB/s)"]]

C["[CPU]"] <--> L
C <--> RAM
C <--> PCIe
PCIe <--> GPU
GPU <--> VRAM
PCIe <--> MBC
PCIe <--> S
MBC <--> O["Other devices"] 
GPU <--> DS

```

Certains raccourcis on été créé: 

- Le storage a un accès plus direct vers le CPU.
- Une passerelle à été créée entre le storage et le GPU: Microsoft Direct Storage API.

<br />

---

### Langages bas-niveau vs haut-niveau

---
 
<br />

#### Low-Level languages
___

<br />

Initialement, les premiers langages étaient des langages dits "de bas-niveau".

<br />

Leurs fonctionnement utilisent des instructions directes dans un langage proche du langage machine.

<br />

````md magic-move {lines: true}
```asm ts {0|*}
section .data
    ; Définition des données si nécessaire

section .text
global _start

_start:
    mov eax, 5      ; Charge la valeur 5 dans le registre EAX
    add eax, 3      ; Ajoute 3 à EAX (EAX = 8)
    mov ebx, eax    ; Copie le résultat de EAX vers EBX
    mov eax, 1      ; Code d'appel système pour 'exit' sur Linux
    xor ebx, ebx    ; Zéroise EBX (code de sortie normal)
    int 0x80        ; Exécute l'appel système exit`
```

````
*[^1]
[^1]: Un example de code en assembleur.

---

#### Low-Level languages
___

<br />

Ces instructions en général permettent de faire fonctionner le processeur, sa mémoire cache et la mémoire ram du système.

<br />

<div style="display: flex; justify-content: center;">

```mermaid {theme: 'neutral', scale: 0.75}
flowchart LR
id1[[Random Access Memory]]
Cache <--> B[CPU] <--> InternalStorage
id1 <--> B[CPU] 

```
</div>

- Leur écriture et compréhension est particulièrement difficile, de part leur manque d’abstraction, et du nombre d’instructions parfois nécessaires pour réaliser une simple tâche.

- Ils ont pour avantage d’être extrêmement rapide de part leur lien fort avec le hardware et leur utilité va de pair avec le hardware qui requiert leur utilisation.

---

#### High-Level Languages
___

Les langages de plus haut niveau arrivent ensuite (Ex: C, C++, Pascal, Kobol, Java, etc…).

<br />

```py
# Définition des nombres à additionner
nombre_a = 5                                # Équivalent au 'mov eax, 5' en assembleur
nombre_b = 3                                # Le deuxième opérande

# L'opération elle-même : l'abstraction du processeur
resultat = nombre_a + nombre_b              # Équivalent au 'add eax, 3'

# Affichage du résultat (l'équivalent de notre sortie réussie)
print(f"Le résultat de {nombre_a} + {nombre_b} est : {resultat}")
```
*[^2]
[^2]: Un example de code en python.

<br />

---

#### High-Level Languages
___

<br />

- Ils ont une abstraction plus forte que les langages de bas-niveau et sont généralement plus proche du langage humain (Anglais).
    - Cela améliore drastiquement leur compréhension. 

- Ils disposent de bibliothèques de fonctionnalités, qui comportent beaucoup de fonctions de base et essentielles.

    - Ce qui permet d'éviter de devoir tout réécrire à chaque fois en langage bas niveau.

<br />

Ex: la fonction print() utilisée ici en fin d’exemple…

---

#### Résumé
___

<br />

##### Low-Level Languages

<br />

| Avantages | Inconvénients |
| --------- | ------------- |
| - Rapidité et précision | - Complexité |
| - Lien fort avec hardware | - Manque d'abstraction |

<br />

##### High-Level Languages

<br />

| Avantages | Inconvénients |
| --------- | ------------- |
| - Abstraction forte / Compréhensible | - Moins de contrôle |
| - Aspect fonctionnel et varié | - Performances dépendantes du language et de son utilisation |

<br />

---

### Les paradigmes
___

<br />

Il existe plusieurs manières de programmer,... plusieurs "paradigmes de programmation".

<br />

Voir [Paradigme (programmation) — Wikipédia ](https://fr.wikipedia.org/wiki/Paradigme_%28programmation%29)

<br />

---

<br />

Sans non plus tout lister, car ce n’est pas l’objectif de ce cours...

...l’idée est vous montrer simplement quelques notions avant d’aller plus loin…, on peut citer notamment:

<br />

- la programmation séquentielle :
=> Je fais ça, puis ça, puis…


```bat {0|2-3|4|5|*}
@echo off
SET /A number_a = 5
SET /A number_b = 3
SET /A result = %number_a% + %number_b%
echo %result%
```
*[^1]
[^1]: Un example de code en batch.

<br />

---


- la programmation événementielle :

<br />

=> je réagis précisément à une action

<br />

```js {5-7|11|*}

<script>
    // --- Définition des fonctions réactives (les "écouteurs") ---
    // Fonction appelée quand on clique sur le bouton
    function manageClic() {
        console.log("Action : Clicked!"); // Log dans la console pour les développeurs
    }

    // --- L'enregistrement des écouteurs d'événements (Le cœur du paradigme) ---
    // On dit au bouton: "Quand un clic arrive, exécute la fonction manageClic."
    document.getElementById('bouton').addEventListener('click', manageClic);

</script>

```

---

- la programmation orientée objet :

<br />

=> Je défini un “modèle” afin de représenter quelque chose (par ex une personne), qui contient une structure de données (pour définir son état), et des fonctions (pour définir son comportement). 

<br />

<div style="display: flex; justify-content: center; ">

```mermaid
classDiagram
    class Person
    Person: +String Name
    Person: +Int Age
    Person: SayHello()
```    

</div>

Et quand j’ai besoin d’un exemple de ce quelque chose, j’en crée un (une copie de ce modèle) et je l’utilise, avec le reste.

<br />

Aussi (ce ne sera pas illustré ici par simplicité), vu que tout est regroupé, je peux délimiter au préalable ce qui est accessible. (de l’extérieur)

<br />

---

``` py {1|2-6|7-10|1-10|12-24}
class Person:
    # Constructeur : définit l'état initial
    def __init__(self, name: str, age: int): # appelé à la création de l'object
        # En python, on définit ici les variables de l'objet 
        self.name = name  # Attribut (État)
        self.age = age  # Attribut (État)

    # Méthodes (Comportement)
    def sayHello(self): 
        return f"Hello, I'm {self.name}."

# --- Programme principal ---
person1 = Person("Alice", 30)   # Création/Instanciation du modèle
message = person1.sayHello()    # Appel du comportement sur l'objet
print(message)                  # Utilisation de l'objet dans la logique principale

person2 = Person("Martin", 26)   # Création/Instanciation du modèle
message = person2.sayHello()    # Appel du comportement sur l'objet
print(message)                  # Utilisation de l'objet dans la logique principale

person3 = Person(16, "xxx")   # Création/Instanciation du modèle
message = person3.sayHello()    # Appel du comportement sur l'objet
print(message)                  # Utilisation de l'objet dans la logique principale
```

---

- mais il y en a plein d’autres, chacun ayant des avantages et inconvénients… (on peut même parler de domaines d’application)

<br />

Enfin, un langage peut parfois fonctionner avec plusieurs paradigmes. (ex C++, C#, python, etc…)

---

### Langage compilé vs interprétés
___
#### Langage Compilé
___

<br />

- Il y a des langages compilés :

    - On écrit les sources dans notre langage, mais celles-ci ne sont pas directement comprises par le processeur ; elles doivent être traduites. 
    - Ce processus de traduction, appelé compilation, convertit notre code source en une série d’instructions machine compréhensibles par la plateforme cible.
    - ceci afin de produire ce qu’on appelle, un fichier binaire exécutable. (dans windows, par exemple un fichier .exe)

<br />

<div style="display: flex; justify-content: center;">

```mermaid {theme: 'neutral', scale: 0.85}
flowchart LR
  subgraph Sources
    A[main.cpp]
    B[player.cpp]
    C[utils.cpp]
  end

  D[/Compilateur\nGCC \/ MSVC/]

  subgraph Output
    E([programme.exe])
  end

  Sources --> D --> Output
```

</div>


<br />

Cela nécessite un environnement de développement, ainsi que les SDK et autres dépendances requise par le runtime que vous utilisez pour développer.

---

<div style="display: flex; justify-content: center; ">

 <img src="/ue_github02.png" width="90%" />

</div>

---

#### Langage Compilé
___

Ce principe est le plus courant.



<br />

- Une fois l’application buildée, il n’est plus possible de la modifier sans la re-builder.

- Cela rend la re-lecture du code du programme bien plus compliquée (si on ne dispose pas des sources) qu’un programme interprété car il a été converti en binaire; 

    - Il faut un désassembleur pour le reconvertir en code (low-level) et s’en suit une grosse étape reverse engineering profond, pour comprendre et réécrire l’ensemble dans un code compréhensible.

    => Ce n’est pas impossible, mais cela prend beaucoup de temps, et de savoir-faire.

---

#### Langage Interprété
___

<br />

Mais, en parallèle de cà, d’autres langages sont des langages interprétés (exécutés tel quel, sans compilation), c’est notre langage qui tourne au runtime. (ex python)  

=> Il n’y a donc pas de notion de fichiers sources, ni de binaire compilé, c’est livré tel quel.
Le code n’est plus vraiment protégé explicitement, mais c’est aussi moins contraignant.

<br />
---

### Langages non-managés vs managés.
___

<br />

Une autre différence à évoquer, est la différence entre les langages non-managés et les langages managés.

<br />

#### Langages non-managés
___

<br />

Exemple C++ :

=> j’ai besoin de mémoire, j’alloue de la mémoire, je l’utilise, et je pense TOUJOURS à libérer la mémoire manuellement quand je suis sur d’avoir fini:

<br />

Ceux-ci sont en général plus performants, mais comportent inévitablement plus de risque de fuite de mémoire, car la responsabilité de libérer la mémoire, incombe au développeur.

<br />

=> Si celui-ci alloue de la mémoire pour faire une opération dans son code, et qu’il oublie de la libérer, cela provoque ce qu’on appelle un LEAK.

<br />

Autrement dit, une fuite de mémoire; 

=> Si le code ne peut que allouer de la mémoire sans jamais pouvoir la libérer, la consommation de mémoire (ram) risque d’augmenter avec le temps d’utilisation du programme, jusqu’à éventuellement arriver à un crash du système quand la mémoire sera saturée.


---

#### Langages managés
___

<br />

Exemple C# :

=> j’ai besoin de mémoire, j’alloue de la mémoire, je l’utilise, et le système la libérera lui même automatiquement, plus tard… (quand?), quand cette mémoire ne sera plus reliée à rien…

<br />

Eux sont en général à l’opposé un peu moins optimisés, mais sont plus safe à utiliser.


=> Le problème ici est qu’au runtime, dans un game engine, le temps est précieux, chaques ms compte, et l’opération qui libère les ressources, le GarbageCollector prend lui aussi des ressources (précieuses) lorsqu’il s’exécute,... 

On préférera dès lors repenser (à 10x) notre code pour limiter un maximum son déclenchement.


---

### Les variables
___

<br />

Ok, maintenant on va parler un peu de variables.

<br />

Une variable c’est quoi? 

=> C’est une sorte de conteneur pour stocker une (ou plusieurs)  information(s). 

=> C’est une zone mémoire réservée, avec un nom pour y accéder…:

(… ou parfois plutôt une adresse, parce que c’est plus direct)

<br />


---

#### Les types de variables
___

<br />

Maintenant, c’est bien, mais il y a quoi dedans?

<br />

=> C’est la que le type vient en jeu.

<br />

Quand vous créez une variable, vous pouvez/devez spécifier de quel type elle est.

<br />

En précisant ce type, nous savons dès lors à tout moment, en plus du nom de la variable, quel type de donnée trouver dedans (en cas de lecture), mais aussi quel type de donnée nous pouvons aussi écrire dedans .

<br />

=> Par exemple, si nous avons besoin d’une variable pour stocker l'âge d’une personne, cette distinction de type va permettre de faire la différence entre “40” et “quarante”, entre des chiffres entier (INT) et un mot en lettres (“String”), ou encore un nombre à décimale inutile ici. (40.0)


---

##### Data-Types vs Reference-Types
___

<br />

Aussi, en fonction du langage, il a toujours une liste  de types de base dit types de valeur 

<br />

=> si on fait une copie de la variable dans une autre et qu’on modifie l’original par après, la valeur ne sera modifié que dans l’original et pas la copie

<br />

Et ainsi que d’autres types dit de référence 

<br />

=> si on modifie l’une, l’autre est modifiée aussi


---

###### Les Data-Types
___

<br />

Quelques exemples de Data types:
- bool, ou BOOLEAN => peut contenir 2 valeurs: true, ou false (1 ou 0)
- int ou INTEGER => peut stocker un nombre entier (en général de -4 milliard à + 4 milliard)
- float => peut stocker des “nombres à virgule flottante” (décimale)
- char => peut stocker un charactère.
- enum ou ENUMERATION => peut contenir une liste (énumération) de champs, uniques, indexable tel quel, ou avec un numéro.
- il y en a d’autres en fonction du langage, souvent plusieurs variante d’un même type mais avec une taille mémoire différente. (ex Int16/short), Int64/long, float64/double), ou encore version signée vs non signée (ex uint, sbytes, ufloat…)
- votre data type custom.

<br />

…Oui un type correspond a une taille mémoire, et certains langages permettent de faire la différence.

---

###### Les Reference-Types
___

<br />

Et voici aussi quelques exemples de types références:
- string => peut stocker une chaine de charactère. (mais attention se comporte comme un value type en général)
- Array, ou List => peut contenir plusieurs valeurs, indexable avec un numéro
- Dictionnary => peut contenir plusieurs valeurs, indexable avec un une clé (nom)
- tout autre “collection”
- tous les autres types propre au langage visé (il y en a énormément, et cela varie beaucoup d’un langage à un autre)
- tous vos types références custom.


---

### Les langages typés et les langages non-typés.
___

#### Langages typés
___

<br />

Partant de ce principe de types de variable, c’est la qu’interviennent les langages typés.

<br />

En forçant un typage “fort” cela permet de s’assurer de définir le type d’une variable à l’avance, ce qui nous assure une certaine sécurité.

=> ex: C#

<br />

Cela permet aussi au compilateur de mieux prédire au moment de la compilation, le comportement de votre code.

<br />

Ceci car il connaît à l’avance tous les types de variables utilisés, et peut provoquer une erreur de build pour vous prévenir que quelque part votre code va essayer de stocker un type de valeur qui ne correspond pas à la variable dans lequel stocker cette valeur. 

(Et vous empêche un crash, inutile…)


---

#### Langages non-typés
___

<br />

Mais, il existe aussi des langages dit non-typés!

<br />

Ces langages sont dès lors beaucoup plus permissif; 

la responsabilité de veiller à fournir le bon type de donnée n’incombe plus au compilateur mais au développeur…… :)

=> Si un moment la donnée ne correspond pas, ce sera au runtime que cela se passera (cassera) et non pas au moment de compiler le programme. 

<br />

=> Il faudra beaucoup compter sur le parsing, la gestion des erreurs et les logs, si à divers moment il est demandé à l’utilisateur d’entrer des données manuellement, ainsi que pendant le développement.

<br />

A contrario, certaines choses sont plus rapides et faciles à faire.
