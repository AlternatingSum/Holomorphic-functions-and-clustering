# Holomorphic functions and clustering
This program transforms one shape to another via a conformal mapping. It then measures the "distance" between two shapes by calculating the total stretching required to transform one shape into the other. Given a collection of shapes, it uses hierarchical clustering to group them based on these distances, placing similar shapes in clusters together. 

## Holomorphic functions and the Riemann mapping theorem

Given a pair of shapes in the plane, a differentiable function between them is said to be *holomorphic* if it preserves angles. Such a function may deform the original shapes, transforming straight lines into curves. But if two lines in the domain cross at a right angle, then they must still cross at a right angle after being transformed. 

If a holomorphic function is both one-to-one and onto, and its inverse is holomorphic as well, this function is called a *conformal mapping*. Such a function can transform the first shape into the second, and vice versa, while preserving all angles. 

If a planar shape is topologically equivalent to a disk (e.g. it doesn't have holes), then the Riemann mapping theorem states that there is a conformal mapping which transforms this shape into a disk. However, these mappings are difficult to find: They don't typically have closed representations and must be approximated numerically through an iterative process. 

The program holomorphic_functions_and_clustering.py contains code to transform an image into a disk, using an iterative process similar to the one described in the proof of the Riemann mapping theorem. Here is the result of transforming an image of a vase via this process, after various numbers of iterations: 

![Vase stretching](https://i.imgur.com/qRhAURn.png)

## Tranforming shapes via conformal mappings

If we have conformal mappings from two different shapes to a disk, we can combine these functions to create a conformal mapping from one shape to another The program holomorphic_functions_and_clustering implements this as well, and given two images it can create a "shapeshifted" version of one image with the shape of the other. Here are examples of this: The top row shows three original images of vases, and the bottom two rows show images obtained by shapeshifting the original vases into each other. (Note: These images were created by an earlier version of this program by the same author. Precise shapeshifting benefits from precise identification of foregrounds, and to create these images we used hand labeled foreground pixels. The current program uses automatic foreground detection, but is less precise.) 

![Transformed vases](https://i.imgur.com/s7FAmX5.png)

## Defining distance between shapes

Since we can now transform one shape into another in an angle-preserving way, we can now define a "distance" on the space of shapes. To find the distance between two shapes we first construct a conformal mapping between them (choosing one which preserves top, bottom, and vertical center of mass, to avoid ambiguity). We may then consider how much stretching is happening at each point to accomplish this. If the shapes are very different this will require a lot of stretching, and if they're very similar it may not require much stretching at all. By adding up the local stretching factors at each point in a mesh we arrive at a number representing the distance between the two shapes. 

## Clustering images by shape

Since we have a notion of distance on the space of shapes, given a collection of shapes we can now categorize them using hierarchical clustering. To test this, we gave the program ten images of vases which a human might organize into three different styles. The program correctly arrived at the same three categories: 

![Clustered vases](https://i.imgur.com/KIkO8Xa.jpg)

The results weren't perfect, however, and adding the following two vases (which a human might place in the third cluster) resulted in a different partition into clusters: 

![Troublesome vases](https://i.imgur.com/KPYBlGG.jpg)
