# Fichiers
Voilà le guide qui accompagne la lecture du fichier intitulé Synthèse_du_Code.
Le fichier Synthèse_du_Code se concentre sur les dernières avancées que j'ai fait, ie je n'ai pas pris la peine de remettre l'ensemble du travail de "découverte" du domaine fait cet été, quand je mettais en évidence les limites d'une simple descente de gradient.

¤ La première cellule sert est la brique fondatrice du travail de développement effectué plus tard.
Les fonctions qui y sont définies sont commentées dans le fichier.
On commence par y définir :

1) La classe gérant les couches de neurones, puis celle gérant les réseaux.
   On trouve dans cette classe des fonctions classiques pour pouvoir initialiser, modifier, et utiliser un réseau.
   On y trouve aussi les 2 seules fonctions du code que je n'ai pas codé entièrement tout seul. Je souhaitais pouvoir sauvegarder des réseaux de neurones via un fichier .txt et les reload plus tard.
   Etant donné que ce n'est pas une tâche qui représente le moindre intérêt scientifique, et que je ne savais pas ouvrir et modifier un .txt via python, ChatGPT s'en est chargé.
 
2) La classe Opti. Elle permet de créer des processus d'optimisation. Concrètement, créer et utiliser un Opti, c'est utiliser un optimizer(classe Algos) pour entraîner un réseau à minimiser une fonction donnée, sur un nombre fixé d'itérations. Globalement, tout est expliqué dans les commentaires du code vis à vis de cette classe

3) La classe Algos. Elle définit l'aspect purement algorithmique du processus, qui sert à modifier les poids (fonction mod). Elle sert à créer les optimizers, ensuite exploités dans le processus d'"Opti". De même, il n'y a pas grand chose à rajouter.

4) La classe Visu. Elle sert à tout gérer d'un seul coup lorsqu'on souhaite visualiser un processus d'optimisation. Elle gère alors l'optimisation, et l'affichage d'une visualisation, 2d ou 3d.
   J'ai mis l'appel du module bizarre qui sert à gérer la 3d au sein de la définition de la fonction .visu() de la classe. Ainsi, vous pouvez utiliser le module Visu avec la fonction .plt() pour les plots 2d matplotlib de la perte en fonction de l'epoch sans que tout ne se casse pour autant.

¤ La deuxième cellule est à part pour que vous ne l'éxecutiez pas, elle me sert juste à accéder à mon .txt

¤ La troisième cellule me sert à introduire plusieurs optimizers classiques(via la classe Algos). 

¤ La 4e cellule implémente 3 fonctions de tests que j'utiliserai pour créer un Optimizer.

¤ Il est temps de fournir quelques explications sur où on va maintenant. Je suppose qu'entre, ce que je viens d'introduire, et ce qui s'apprête à venir, il y aura d'autres explications sur le sujet le jour J, notamment mettre en évidence la nécessité d'un processus d'optimisation pour les réseaux, montrer les limites de la descente de gradient, en venir à la conclusion qu'il serait quand même cool d'essayer de trouver de meilleures solutions. 
Ainsi, ce qui suit est la partie "recherche" où j'ai cherché une manière de créer un optimizer cohérent et pouvant triompher d'Adam dans une proportion des situations qu'on essaiera de rendre aussi vaste que possible.

L'idée : reprendre la direction indiquée par une descente de gradient avec momentum, mais choisir à chaque étape à quel point on appuie sur l'accélerateur, en fonction de la convergence ou non des directions prises précédemment, un peu comme un humain le ferait. Concrètement, pour faire cela, on décide du learning rate à chaque itération. Qui décide ? Je pensais au début créer un algorithme moi même, mais ça se serait juste apparenté à une énième version légerement modifiée d'Adam, et Internet regorge déjà de papiers sur des optimizers magiques qui sont quasiment identiques à Adam.
Finalement, je me suis dit qu'en lui donnant des données pertinentes, un petit réseau de quelques neurones pourrait se charger de la décision du learning rate.
Je considère alors un softmax des dernières 5 valeurs absolues des gradients, auxquels je restitue leur signe initial après le calcul du softmax, afin que le réseau puisse être sensible aux changements de direction.
Je donne aussi la variance des 5 derniers gradients normalisée par la somme de leurs valeurs absolues.

¤ Cellule 5 : Les fonctions softmax_abs(), v1 et v2, sont les fonctions de softmax dont je parlais précédemment. J'utilise actuellement la v2 pour o1_3, l'optimizer que je développe.
La fonction o1_3, qui est la dernière version de l'algorithme derrière o1, prépare les données sur les gradients et les donne au réseau de neurone pris en input. L'output de ce dernier est mise dans une exponentielle afin de donner plus de range d'action au réseau, sur ce qui sera utilisé comme le learning rate.

¤ Cellule 6 : Afin d'obtenir un réseau performant, je pars sur un modèle d'entrainement générationnel : A chaque génération naît une population consitué de mutations du meilleur agent précédent.
Chaque agent représente un réseau de neurone, qu'il utilise avec la fonction o1_3 pour obtenir l'Algo o1, qu'il utilise ensuite sur nb_iter_par_gen différents points de départs pour effectuer des processus d'optimisation, et ce sur les 3 fonctions de test.
Ainsi, en augmentant nb_iter_par_gen, on espère obtenir une version de l'Algo o1 capable de généraliser correctement à l'ensemble du domaine de définition des 3 fonctions en question.
Puisque chacune de ces fonctions représente des aspects intéressants et uniques, volontairement fortement accentués puisqu'il s'agit de fonctions de test, j'espère ensuite que o1 sera capable de généraliser à la plupart des fonctions de perte.

D'autres évolutions sont toujours à venir, notamment sur la mise en place d'une manière d'établir une évaluation plus générale de o1 par rapport à un benchmark constitué d'optimizers réputés (SGD avec momentum, Adam).

¤ La cellule 7 me sert à enregistrer sous un certain identifiant le modèle venant d'être entrainé.
