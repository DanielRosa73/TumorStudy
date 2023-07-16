Auteurs: Marc Lagoin / Daniel Rosa

# 1 - Recalage

Type de transformation : La transformation choisie est la TranslationTransform. Il s'agit d'une transformation rigide qui ne modifie que la position de l'image sans changer son orientation ou son échelle. Ce choix est basé sur le fait que les deux images sont déjà dans une orientation similaire et à une échelle similaire.

Optimiseur : L'optimiseur choisi est le RegularStepGradientDescentOptimizerv4. C'est un optimiseur de descente de gradient qui ajuste les paramètres de la transformation pour minimiser la fonction de cout. 

Métrique : La métrique choisie est MeanSquaresImageToImageMetricv4, qui calcule la différence moyenne des carrés entre les intensités de l'image fixe et de l'image en mouvement. 

Enregistrement : L'objet d'enregistrement ImageRegistrationMethodv4 est utilisé pour exécuter l'enregistrement. Il prend en entrée l'image fixe, l'image en mouvement, la métrique et l'optimiseur, et il produit une transformation optimisée.

Rééchantillonnage de l'image : Après l'enregistrement, l'image en mouvement est rééchantillonnée à l'aide de la transformation composite pour aligner l'image en mouvement avec l'image fixe.

Soustraction d'images : Enfin, la différence entre l'image fixe et l'image rééchantillonnée est calculée pour évaluer la performance du recalage.


# 2 - Segmentation

Choix des seuils : 

Nous définissons les seuils inférieur et supérieur pour la binarisation. Ces seuils déterminent quels voxels seront considérés comme faisant partie de la tumeur. Nous avons choisi les valeurs de 50 et 150 après une analyse exploratoire de l'image. Il est important de noter que ces valeurs peuvent ne pas être idéales pour toutes les images et qu'un ajustement peut être nécessaire en fonction de l'image spécifique à traiter.

Application du filtre de seuil : 

Nous appliquons le filtre de seuil à l'aide de la fonction itk.BinaryThresholdImageFilter. Cette fonction prend notre image et nos seuils en entrée et produit une image binarisée en sortie.

Définition des valeurs à l'intérieur et à l'extérieur du seuil : 

Nous définissons la valeur des voxels à l'intérieur du seuil à 1 (ce qui représente la tumeur) et la valeur des voxels à l'extérieur du seuil à 0 (ce qui représente le reste de l'image).

Mise à jour du filtre : 

Nous mettons à jour le filtre de seuil avec la méthode Update(). Cette étape est nécessaire pour que le filtre de seuil exécute réellement la binarisation.

Enfin, nous récupérons l'image segmentée avec la méthode GetOutput() et nous l'écrivons dans un fichier NRRD.


# 3 - Analyse

Calcul de la différence d'intensité : 

Une fois que nous avons les deux images sous forme de tableaux NumPy, nous calculons la différence d'intensité absolue entre les deux en utilisant np.abs(scan1_array - segmented_image_array).

Visualisation de la différence d'intensité : 

Nous visualisons ensuite la différence d'intensité en utilisant Matplotlib. Pour cette visualisation, nous choisissons une tranche centrale de l'image 3D pour montrer les différences. La carte de couleurs 'hot' est utilisée pour représenter clairement les régions de haute différence d'intensité.

Seuil de différence d'intensité : 

Un seuil arbitraire est défini pour déterminer si un voxel est considéré comme faisant partie de la tumeur. Dans ce cas, le seuil est fixé à la moitié de la différence d'intensité maximale. Ce choix est arbitraire et peut être ajusté en fonction des besoins spécifiques de l'analyse.

Enfin, la différence de volume de la tumeur est calculée en comptant le nombre de voxels où la différence d'intensité dépasse le seuil. Cette mesure peut donner une indication de l'évolution de la tumeur.