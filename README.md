# TIPE MP 2024-2025

Ce TIPE se concentre dans un premier temps sur l'implémentation à partir de numpy uniquement d'un environnement permettant la manipulation de réseaux de neurones et d'optimizers.

Dans un second temps, j'y ai développé un type d'optimizer dont l'un des paramètres, (similaire au learning rate), était calculé par un petit réseau de neurones annexe. J'ai développé mon optimizer avec l'idée que donner des entrées pertinentes sur les précédents gradients à un réseau lui permettrait d'interpoler la structure du paysage.

Sur un ensemble de fonctions de tests prédéfinies, sur lesquelles l'optimizer avait été développé ou non, on constate en moyenne une meilleure convergence de cet optimizer que d'Adam.

Malgré le caractère désordonné de ce repository, il n'en fût pas moins pédagogique pour moi en tant que premier pas dans le monde de l'optimisation.
