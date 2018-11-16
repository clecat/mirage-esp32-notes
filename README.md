# mirage-esp32-notes
Notes I took about my uses of the esp32 portage of mirageOS by @well-typed-lightbulbs.

Important note: I'm using the Odroid-Go model, every informations noted here should be used carefully when using another model.

Notes may sometime be written in French, I will translate them as soon as I can.

## Graphic driver

Pour l'odroid-go, il faut changer le driver de lortex: en effet, les pins utilisées dans l'appareil ne sont pas les mêmes que dans celui qu'il utilisais.
Ce problème est d'ailleurs très certainement extensible à tout les autres appareils.
Afin de le résoudre, j'ai cherché dans les sources des drivers C++ données par le fabriquant ("Display.cpp").
De plus, il est nécessaire de changer les numéros des pins correspondantes, mais aussi la suite de bits envoyés à l'écran à l'initialisation. Je l'ai trouvé dans le même fichier.

__Problèmes à résoudre:__

Le code source C++ mentionnais une pin de reset avec une valeur de 0.
Je dois trouver un moyen de passer outre, sachant que cette valeur fait planter le code initial.
Le rétro éclairege de l'écran n'est pas géré par le driver que j'ai retouché.

## Flashing and launching the app on the odroid

Pour ce faire, suivez le tutoriel de well-typed-lightbulbs/mirage-samples-esp32.
Quelques remarques supplémentaires:
* La RAM:
    * Les esp32 sont dotés d'une RAM extrêmement faible, si votre appareil contient donc de la RAM en spi, vous pouvez l'utiliser. Néanmoins, souvenez vous que la RAM SPI ne permet pas d'allouer des ressources dont l'accés est nécessaire en DMA (ex: buffer pour le lcd etc).
* Dans menuconfig:
    * Pour activer la RAM SPI, aller dans component config -> esp32 specific -> 2nd choix
    * Augmentez la taille de la flash (16 mb sur l'Odroid) dans flasher config -> flash size
    * Vous aurez besoin de python2 pour continuer. Si votre python par défault est python3, vous pouvez créer un virtualenv ou bien modifier SDKtoolconfig -> python en spécifiant python2
* Pour le flash:
    * Mettez à jour votre environnement avec le PATH menant à votre instance de xtensia.
* Pour le monitor:
    * Le raccourci pour quitter l'application est ctrl + ]. Si votre clavier est un azerty, utilisez ctrl + altgr + ).
