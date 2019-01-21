# Computer Graphics

Every desktop computer today has a powerful 3D graphics pipeline. The basic operations in the pipeline map the 3D vertex locations to 2D screen positions and shade the triangles so that they both look realistic and appear in proper back-to-front order. It turns out that the geometric manipulation used in the graphics pipeline can be accomplished almost entirely in a 4D coordinate space composed of three traditional geometric coordinates and a fourth homogeneous coordinate that helps with perspective viewing. These 4D coordinates are manipulated using 4 × 4 matrices and 4-vectors.

A key part of any graphics program is to have good classes or routines for geometric entities such as vectors and matrices, as well as graphics entities such as RGB colors and images. This implies that some basic classes to be written include:

* vector2: A 2D vector class that stores an x- and y-component. It should store these components in a length-2 array so that an indexing operator can be well supported.
* vector3: A 3D vector class analogous to vector2.
* hvector: A homogeneous vector with four components
* rgb: An RGB color that stores three components.
* transform: A 4 × 4 matrix for transformations.
* image: A 2D array of RGB pixels with an output operation.

Modern architecture suggests that keeping memory use down and maintaining coherent memory access are the keys to efficiency. For the foreseeable future, a good heuristic is that programmers should pay more attention to memory access patterns than to operation counts because the speed of memory has not kept pace with the speed of processors.

Much of graphics is just translating **math** directly into code. The cleaner the math, the cleaner the resulting code.

