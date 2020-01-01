# Training Data Augmentation (Img and VOC Label) for Object Detection including Class Balancing 

## What is This Tool?

While working on a machine learning project, I found myself needing to be able to easily augment object recognition training images and their corresponding bounding box labels in bulk. With the images labeled with multiple objects and/or classes via the Pascal VOC XML convention (e.g. https://gist.github.com/Prasad9/30900b0ef1375cc7385f4d85135fdb44), I needed any augmentation acting on some image to also act on the bounding boxes of the objects labeled in the image, such that the augmented image still had the objects labeled correctly. As such, I needed the augmented image to be accompanied by it's corresponding augmented XML file.

TL;DR: I wanted to be able to seamlessly feed a bunch of multi-class, multi-object XML-labeled training images into an image augmentation pipeline which returns augmented and correctly labeled images in the same format with minimal effort. That is what this script does.

The script also attempts to prioritize augmenting images that have underrepresented class instances, such that the final training set (including augments) has a better balance between the number of objects labeled for each class.

## Usage

The main script, `augmentAndBalanceData()` is used to run the pipeline, and has the following arguments, in order:
 
 **1.** `train_image_folder`, the folder containing the training images (path/string)
 
 **2.** `train_annot_folder`, the folder containing the images' augmentations (see the below note) (path/string)
 
 **3.** `allLabels`, a list of all possible class labels (list of strings)
 
 **4.** _Optional:_ `minObjCount`, used to balance the dataset classes, this is the minimum number of instances of each class that you want to have after creating augmented images (int). Default = 100.

That's it! Just run `augmentAndBalanceData()` with those arguments, and the augmented images and labels will be deposited within the two corresponding folders that you provided. The augmented files will be named with a suffix of `_augN`, where `N` is some integer, so they are easy to tell apart from the originals.

### Note: training images are linked to their XML annotation files by giving them the same name, e.g. img1.jpeg would have annotation file img1.xml.

## Thanks to:

The actual image augmenting code is thanks to lele12 from https://github.com/lele12/object-detection-data-augmentation. I also use a modified version of a VOC parsing script from experiencor's keras YOLOv3 implementation: https://github.com/experiencor/keras-yolo3. All that I did was make these scripts play nice with each other, introduce easy bulk image augmentation, and automatic class balancing.
