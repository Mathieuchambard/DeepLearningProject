[![License CC BY-NC-SA 4.0](https://img.shields.io/badge/license-CC4.0-blue.svg)](https://raw.githubusercontent.com/nvlabs/SPADE/master/LICENSE.md)
![Python 3.6](https://img.shields.io/badge/python-3.6-green.svg)

# Semantic Image Synthesis with SPADE
![GauGAN demo](https://nvlabs.github.io/SPADE//images/ocean.gif)


## Tester nos modèles

Avant toute chose, cloner ce repository dans votre environnement.

Pour tester il vous faut les librairies python suivantes :
- torch>=1.0.0
- torchvision
- dominate>=2.3.1
- dill
- scikit-image


De plus, il vous faut télécharger un des modèles suivant :
- LandscapeModel (https://mega.nz/folder/IbsABS5T#_GJWxdOgUic6wPBvehFFZQ) : Modèle pour générer des images de paysages classiques.
- AnimeModel (https://mega.nz/folder/ZWt1jDyY#DdF0152WTyaBJtqPnztN6A) : Modèle pour générer des images de paysages animés.

Créer, si il n'existe pas, un dossier checkpoints à la racine du projet. Puis créer un dossier "nomModel" (Exemple : LandscapeModel) à l'interieur.
Déposer dans ce dossier le modèle que vous avez téléchargé (Dans le fichier checkpoints/LandscapeModel/).

Vous êtes prêt à générer des images !

Créer un dossier en déposant à l'intérieur vos fragmentations. 

Des fragmentation sont disponibles dans imageTest ou dataLanscape/test_label.

Pour faire vos propres dessins, les images doivent être des nuances de gris avec comme couleur de gris : 1 + le label que l'on veut. Les labels sont dans labelInformation/label.csv.
Exemple : Si vous voulez mettre du ciel, il faut regarder le label dans label.csv : le label est 3 donc il faut mettre la couleur 4.


Ensuite, lancer dans un terminal :
```
python test.py --name nomModel --dataset_mode custom --no_instance --label_nc 150  --label_dir dossierLabel --image_dir dossierLabel
```

Exemple : 
```
python test.py --name LandscapeModel --dataset_mode custom --no_instance --label_nc 150  --label_dir imageTest/label --image_dir imageTest/label
```

### Entrainer et tester votre propre modèle

#### Fragmenter vos images 

Tout d'abord, il vous faut un dataset d'entrainement. Il vous faut des images ainsi que leur fragmentation.
Si vous avez deja les fragmentations, passez à l'étape d'après.

Sinon, il faut telécharger le dossier (https://mega.nz/folder/YfNhjA6C#iQ8nlSj9hAGIATU2nnNAzA). 
Dans le dossier segmentation, créer un dossier ckpt et mettre le dossier téléchargé.

Pour fragmenter vos images, exécuter dans segmentation :
```
python -u test.py --img dossierImage
```

#### Entrainement du modèle

Entrainer votre modèle peut être trés long. Avec une carte graphique RTX 3060, pour 3455 images pour 50 epochs, l'éxécution a duré 24h.

Pour entrainer votre modèle avec vos propres données, créer un environnement et télécharger  : 
- tensorflow==1.15.0
- scipy==1.2.0
- pillow==6.1.0

Lancer la commande suivante :
```
python train.py --name nomModel --dataset_mode custom --label_dir dossierLabel --image_dir dossierImage --label_nc NombreLabel --no_instance
```

Remplacer nomModel par le nom de votre model, dossierLabel et dossierImage par le nom de dossier de vos labels et de vos images.
NombreLabel est le nombre de label fragmenté. Si vous utilisez les fragmentations de la partie Fragmenter vos images de ce tutoriel, remplacer NombreLabel par 150.

Si l'entrainement s'interrompt, relancez avec la commande : 
```
python train.py --name nomModel --dataset_mode custom --label_dir dossierLabel --image_dir dossierImage --label_nc NombreLabel --no_instance --continue_train
```


Si vous voulez entrainer le modele avec nos données :

Paysages classiques : 
```
python train.py --name LandscapeModel --dataset_mode custom --label_dir dataLandscape/train_label --image_dir dataLandscape/train_img --label_nc 150 --no_instance
```

Paysages animés : 
```
python train.py --name animeModel --dataset_mode custom --label_dir dataAnime/label --image_dir dataAnime/img --label_nc 150 --no_instance
```

#### Test du modèle 

Voir la partie Tester nos modèles en changeant le nom du model.


## Source
Ce code est emprunté des méthodes SPADE et pix2pixHD.
Lien Github : https://github.com/NVlabs/SPADE, https://github.com/NVIDIA/pix2pixHD



### [Project page](https://nvlabs.github.io/SPADE/) |   [Paper](https://arxiv.org/abs/1903.07291) | [Online Interactive Demo of GauGAN](https://www.nvidia.com/en-us/research/ai-playground/) | [GTC 2019 demo](https://youtu.be/p5U4NgVGAwg) | [Youtube Demo of GauGAN](https://youtu.be/MXWm6w4E5q0)

Semantic Image Synthesis with Spatially-Adaptive Normalization.<br>
[Taesung Park](http://taesung.me/),  [Ming-Yu Liu](http://mingyuliu.net/), [Ting-Chun Wang](https://tcwang0509.github.io/),  and [Jun-Yan Zhu](http://people.csail.mit.edu/junyanz/).<br>
In CVPR 2019 (Oral).

