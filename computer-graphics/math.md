# Math

> [https://www.3blue1brown.com/essence-of-linear-algebra-page/](https://www.3blue1brown.com/essence-of-linear-algebra-page/)
>
> 1. 向量是什么
> 2. 线性组合、张成的空间、基
> 3. 矩阵与线性变换
> 4. 矩阵乘法
> 5. 行列式、逆矩阵、列空间
> 6. 点积
> 7. 叉积
> 8. 基变换
> 9. 特征向量与特征值
> 10. 抽象向量空间

线性代数中最基本、最根源的组成部分就是向量，矩阵是对向量的线性变换。

既有大小又有方向的量叫做向量，向量加法 addition 和向量数乘 scalar multiplication 贯穿线性代数始终，起着至关重要的作用。

任一向量可以看作是单位向量 $$(i,j)$$ 经过标量 scalar 缩放 scale 后的结果。

The set of all possible vectors that you can reach with a **linear combination** of a given pair of vectors is called the **span** of those two vectors.

空间上的两个不共线的三维向量的线性组合所张成的空间是一个平面；三个不共面的三维向量的线性组合所张成的空间是整个三维空间。

线性变换可以用变换后的基向量的坐标来描述。以变换后的基向量的坐标为列所构成的矩阵为我们描述了这一线性变换，而矩阵与向量的乘法就是将这种线性变换作用于向量。

两个矩阵相乘，可以看作是两个线性变换相继作用。

（二维空间的）矩阵的行列式描述了线性变换对（二维空间内）任意区域面积的缩放程度，因为我们用单位基向量组成的正方形面积来描述这个缩放，所以你也可以认为行列式就是缩放后的平行四边形的面积。

线性方程组 $$A\vec{x}=\vec{v}$$ 的求解过程，可以看作是找寻线性变换 $$A$$ 的逆变换 $$A^{-1}$$ 的过程，当且仅当 $$det(A)\neq 0$$ 时这个逆变换存在。

如果存在一个矩阵 B 使得 $$AB=BA=I$$ ，那么称 $$n\times n$$ 矩阵 A 为非奇异的 \(nonsingular\)。对于任一 $$n\times n$$ 矩阵 A ，当且仅当 $$det(A)\neq 0$$， A 是非奇异的。

Rank 秩代表着线性变换后的空间的维数。如果一个线性变换将空间压缩成了一条直线，那么 Rank = 1；如果一个线性变换将空间压缩成了一个平面，那么 Rank = 2。

The column space is the span of the columns of your matrix. 矩阵的列向量所能够张成的空间。秩的正式定义，是列空间的维数。

将上述的描述拓展到任意矩阵，而不仅仅是方阵，我们可以很容易察觉，矩阵的列代表的是基向量的坐标，矩阵的行数代表了基向量的维度。

通过深入探究点积的几何意义，我们发现了一个奇妙的对偶性（duality），那就是一个向量的对偶是由它定义的线性变换；而一个多维空间到一维空间的线性变换的对偶是多维空间中的某个特定向量。

