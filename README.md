# Comment détecter des images générées ou modifiées par IA dans les annonces immobilières ?

## Introduction

Ce projet explore des méthodologies innovantes pour détecter et classifier les images générées ou modifiées par IA, crucial pour maintenir l'intégrité des analyses visuelles. 
Nous présentons ici une vue d'ensemble de nos approches, des données utilisées, ainsi que les performances des solutions expérimentées.

## Analyse des différences entre images réelles et artificielles

Pour améliorer la classification des textures par rapport aux objets, les réseaux de neurones convolutifs traditionnels sous-performent en raison de leur focalisation sur les caractéristiques globales et complexes des images, inadaptées aux motifs locaux répétitifs des textures. 
L'algorithme ELA (Error Level Analysis) est utilisé pour détecter les altérations numériques dans les images en analysant les variations de compression.

Différents indicateurs ont été extraits des images pour caractériser les différences entre images réelles et artificielles.

Voici un tableau récapitulatif des indicateurs extraits :

|           Indicateur            | Moyenne pour les images réelles | Moyenne pour les images artificielles |
|:-------------------------------:|:-------------------------------:|:-------------------------------------:|
| Somme de pixels des images ELA  |             0.0967              |                0.5450                 |
| Indice de vivacité des couleurs |              23.71              |                 30.43                 |
|      Indice de régularité       |             0.0027              |                0.0024                 |
|       Indice de rugosité        |             0.0227              |                0.0059                 |
|         Niveau de bruit         |              50.12              |                 56.14                 |
| Artefacts de filtre de couleur  |             0.0154              |                0.0026                 |
|        Indice d'édition         |              12.90              |                 30.43                 |


Pour une analyse approfondie des différences entre les classes d’images, les indicateurs d’Haralick ont été utilisés pour caractériser divers aspects de la texture des images. 
Ces mesures ont mis en évidence des différences notables entre les deux classes pour des indicateurs spécifiques tels que haralick_10, haralick_11, haralick_1 et haralick_8.

## Modèles de classification

- Première solution : **Classification basée sur la somme des pixels des images ELA**. 
Utilisation de l'indicateur de somme des pixels des images ELA avec d'un modèle Support Vector Machine (SVM).
Voir le notebook `model_1.ipynb` pour plus de détails.

- Deuxième solution : **Utilisation d’un CNN préentraîné sur les images ELA**.
Exploitation d'un réseau de neurones convolutifs (CNN) préentraîné (ResNet) pour extraire des features des images ELA.
Utilisation des features dans un modèle Random Forest pour prédire la classe des images.
Voir le notebook `model_2.ipynb` pour plus de détails.

- Troisième solution : **Classification avec d’autres indicateurs sur les images**
Utilisation d'indicateurs tels que la somme des pixels des images ELA, l’indice de vivacité des couleurs, les artefacts de filtre de couleur, l’indice de rugosité, ainsi que les indices haralick_11, haralick_8, haralick_1, haralick_5, haralick_7, haralick_3.
Ces indicateurs sont utilisés dans un modèle Random Forest pour prédire la classe des images.
Voir le notebook `model_3.ipynb` pour plus de détails.

## Conclusion

Voici les performances des modèles de classification sur les images artificielles et réelles :

|             Modèle             | Accuracy | F1-score |
|:------------------------------:|:--------:|:--------:|
|   SVM (Somme des pixels ELA)   |   0.84   |   0.82   |
|   CNN + Random Forest          |   0.89   |   0.89   |
| Random Forest (multi-features) |   0.93   |   0.92   |

