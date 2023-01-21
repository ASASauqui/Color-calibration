<h1><b>Circle images</b></h1>

Circle images with differential evolutionary algorithm from scratch.<br>

<h2><b>Introduction</b></h2>

Color calibration is for practical use in some problems, such as algorithms for meat spoilage detection based on color, chemical measurements based on reagents, etc.; color is a physical aspect that provides information, therefore, its correct calibration is relevant. In this project, the use of the differential evolution algorithm and various image processing techniques, such as inverse binary thresholding and perspective-dependent image warping, for calibrating colors in an image based on a ColorChecker. Transformation and perspective deformation were used for the robustness of the algorithm for the detection of the ColorChecker matrix, as well as the search for polygons according to the contours obtained based on an external search and their polygonal approximation through the algorithm of Douglas-Peucker.<br><br>
The objective of this project is to develop an algorithm that allows the correct calibration of color images. The colors within the image contain information about the image that can be used for various analyses, either for the collection of physical or chemical characteristics of the image. In other related works, different methodologies are used to achieve this calibration, such as the use of Bayesian neural network models, where a neural network is trained with white balance data; the application of Thin-Plate Spline interpolation algorithms, where the sRGB coordinates of the ColorChecker were deformed through TPS interpolation, modified for three-dimensional space; the use of polynomial fit based on pieces, where functions are selected for different intervals; the use of statistical-based methods in an unsupervised learning model where color constancy will be evaluated; or there are even some more focused on merely detecting the ColorChecker in an optimal way, either with the use of deep convolutional neural networks, where a neural network is trained with synthetic images of the ColorChecker 3D models and different background images.<br><br>
The proposal within this project suggests the use of image processing to search for important elements that share characteristics with the matrix being searched for; deformation of the ColorChecker matrix image based on perspective to have a better manipulation of the information; and the use of the differential evolution algorithm for the optimization of an objective function that seeks to reduce the sum of the difference between the control ColorChecker matrix and the calibrated ColorChecker matrix, this by using a 3x3 matrix that will be responsible to be able to calibrate the image.<br><br>

<h2><b>Used technologies</b></h2>

It is an optimization project made with:

<table align="center">
    <tr>
        <th align="center" style="text-align: center" width="50%">
            Programming Language
        </th>
        <th align="center" style="text-align: center" width="50%">
            Library
        </th>
    </tr>
    <tr>
        <td align="center" width="50%">
            <img src="https://img.shields.io/badge/python-blue.svg?style=for-the-badge&logo=python&logoColor=white">
        </td>
        <td align="center" width="50%">
            <img src="https://img.shields.io/badge/cv2-ab33c6.svg?style=for-the-badge">
        </td>
    </tr>
</table>
<br>

<h2><b>Installation and running (local)</b></h2>

### 1. Clone the repository
```
git clone <url>
```

### 2. Running and installations
Using Jupyter Notebook, you will be able to open and experiment with the .ipynb extension file, and you will be able to use code with the .py extension using regular Python.

Make sure you have CV2 installed in order to use the codes.
<br><br>

<h2><b>Codes division</b></h2>

You will find a total of 2 codes made in Python, of which 1 has the extension .ipynb, and the rest have the extension .py. The .ipynb file depends on the .py file to work, since the .py contains the implementation of the differential evolution algorithm made from scratch.

<ol>
    <li>
        <b>differential_evolution.py:</b>
        In this code is the implementation of the differential evolution algorithm made from scratch by us. It is very useful for the process of this project.<br><br>
    </li>
    <li>
        <b>color_calibration.ipynb</b> In this code is all the image processing necessary to prepare the ColorChecker and obtain the necessary data to start the differential evolution algorithm, to obtain the multiplier matrix that will calibrate the image properly.<br><br>
        <div align="center">
            <img width="70%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/codes/1.png?raw=true" />
            <p>Images comparison.</p>
        </div>
    </li>
</ol>
<br>

<h2><b>Folders division</b></h2>

In total there are 2 folders, the one with the name "datasets" and the one with the name "manuscript".

<ol>
    <li>
        <b>datasets:</b> In the "datasets" folder we have images, either of the control matrix and the images that we want to calibrate. The "control_color_checker.jpg" and "c.jpg" images are our control matrices that have an adequate calibration in a good lighting environment, and the other images are to test the algorithm.<br><br>
    </li>
    <li>
        <b>manuscript:</b> In the "manuscript" folder there is an IEEE format manuscript that explains in detail the methodology of this project. If you want to know more, read this article.
    </li>
</ol>
<br>

<h2><b>Methodology</b></h2>

Below is a full explanation of the methodology used.

<h3><b>1. Color matrix extraction</b></h3>
<ul>
    <li>
        <b>A. Load images</b><br>
        Firstly, you need to load the two images to be used, the image that contains the control matrix and another image which will contain the same matrix, but in a different context (different lighting and position). Once these images are loaded, you can proceed to extract the color matrix.<br><br>
        <div align="center">
            <img width="50%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/1.png?raw=true" />
            <p>Control matrix and the image to calibrate.</p>
        </div><br>
    </li>
    <li>
        <b>B. Cut matrix</b><br>
        This is a method that receives the image where the color matrix is located, which you want to cut in order to extract the information. This method must be applied for both images.<br><br>
        Firstly, the area of the image (height x width) is obtained, this for later steps. In order to do a good image processing, the image must be transformed to a gray scale, and after that apply an inverse binary thresholding, it has to be inverse so that the matrix is taken with values of 1, otherwise it will take parts that are not wanted.<br><br>
        <div align="center">
            <img width="50%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/2.png?raw=true" />
            <p>Inversely thresholded images.</p>
        </div><br>
        Once this thresholding has been applied, an algorithm must be applied to search for the contours present in the image, but, since it is only desired to find the matrix in its entirety, this search for contours has to be for those external contours that do not belong to another contour, that is, look for contours that do not depend on others. With this, the matrix can be isolated for later location.<br><br>
        Once all the possible contours have been obtained, the following must be applied for each of them:<br><br>
        <ol>
            <li>
                Get the area of the contour.
            </li>
            <li>
                Calculate the perimeter of the contour by calculating the length of the curve:
                <p align="center">
                    Ɛ = 0.018 * Arc Length of the contour
                </p>
                Where Ɛ is the precision to compute a figure approximation.<br>
            </li>
            <li>
                Approximate a curve polygon or polygon with another curve/polygon with fewer vertices, so that the distance between them is less than or equal to the specified precision. This is done by using the Douglas-Peucker algorithm. With this you can find the number of vertices of the polygon.
            </li>
            <li>
                Once the number of vertices of the contour is obtained, those contours that are not suitable to be a matrix can be cleaned, those that are must have 4 vertices to comply with the shape of a rectangle. In addition, it must be verified that the area of said contour is greater than 1% of the total area of the image, this to eliminate tiny garbage contours that are usually generated in the search step for external contours. Now, the normal thing is that the contour of the largest area of the image that meets these conditions must be the color matrix, since the other contours do not have that shape or considerable size.
            </li>
        </ol><br>
        Having found the best candidate (which should be the color matrix), we can proceed to the image transformation part of the matrix.<br><br>
        <div align="center">
            <img width="50%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/3.png?raw=true" />
            <p>ColorChecker detected.</p>
        </div><br>
    </li>
</ul>

<h3><b>2. Transformation of the matrix</b></h3>

A deformation process will be applied to the matrix cut or extracted from the image so that, regardless of the angle at which said matrix is, it remains in a flat context for better manipulation for data extraction.<br><br>
Firstly, the coordinates of the vertices of said matrix must be arranged to your liking, in this case the order must be clockwise, starting from the upper left vertex, that is: Top-Left, Top-Right, Bottom-Right, Bottom-Left.<br><br>
Once these vertices are arranged in the desired order, the dimensions where the image of the deformed matrix will reside must be created. In this case, the distances between two points must be obtained, obtaining the width A and B, and the height A and B, this is because a perfect rectangle cannot always result, normally it will have the shape of a trapezoid.<br><br>
<div align="center">
    <img width="50%" src="https://latex.codecogs.com/svg.latex?;width_A=\sqrt{(BottomRight_x-BottomLeft_x)^2+(BottomRight_y-BottomLeft_y)^2}"/>
    <img width="50%" src="https://latex.codecogs.com/svg.latex?;width_B=\sqrt{(TopRight_x-TopLeft_x)^2+(TopRight_y-TopLeft_y)^2}"/>
    <img width="50%" src="https://latex.codecogs.com/svg.latex?;height_A=\sqrt{(TopRight_x-BottomRight_x)^2+(TopRight_y-BottomRight_y)^2}"/>
    <img width="50%" src="https://latex.codecogs.com/svg.latex?;height_B=\sqrt{(TopLeft_x-BottomLeft_x)^2+(TopLeft_y-BottomLeft_y)^2}"/>
</div><br>
As the final image is intended to be a rectangle that maintains the proportions of the matrix, the maximum value must be taken for each width and each height.<br><br>

<p align="center">
    𝑚𝑎𝑥𝑊𝑖𝑑𝑡ℎ = 𝑚𝑎𝑥(𝑤𝑖𝑑𝑡ℎ<sub>A</sub>, 𝑤𝑖𝑑𝑡ℎ<sub>B</sub>)<br>
    𝑚𝑎𝑥𝐻𝑒𝑖𝑔ℎ𝑡 = 𝑚𝑎𝑥(ℎ𝑒𝑖𝑔ℎ𝑡<sub>A</sub>, ℎ𝑒𝑖𝑔ℎ𝑡<sub>B</sub>)
</p><br>

Once the maximum height and width values are obtained, an array can be made with the dimensions of the new image, following the order of clockwise and starting from the top left. Here you must put the values in which each vertex will be.<br><br>

<p align="center">
    𝑁𝑒𝑤𝐼𝑚𝑎𝑔𝑒 = < [0,0], [𝑚𝑎𝑥𝑊𝑖𝑑𝑡ℎ − 1,0], [𝑚𝑎𝑥𝑊𝑖𝑑𝑡ℎ − 1, 𝑚𝑎𝑥𝐻𝑒𝑖𝑔ℎ𝑡 − 1], [0, 𝑚𝑎𝑥𝐻𝑒𝑖𝑔ℎ𝑡 − 1] >
</p><br>

Once the maximum height and width values are obtained, a perspective transformation can be performed with the original points and the destination points. Having this, a perspective transformation algorithm is performed in order to provide you with more information about the required information of the given image.<br><br>
<div align="center">
    <img width="50%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/4.png?raw=true" />
    <p>Control matrix and matrix to be calibrated with the perspective transformation.
</p>
</div><br>
To finish, simply resize the image to 800x500 pixels, this to have a constant image size for data extraction, otherwise it may vary in size and affect the last process.<br><br>

<h3><b>3. Taking of colors</b></h3>

A matrix 'm' of the size of the color matrix is created, which has 4 rows and 6 columns, therefore, it will be 4x6 in size.<br><br>

<div align="center">
    <img src="https://latex.codecogs.com/svg.latex?m=\begin{bmatrix}R1&G1&B1\\R2&G2&B2\\...&...&...\\R6&G6&B6\end{bmatrix}"/>
</div><br>

You must determine in which position (x, y) you want to start to extract the colors in the image of the flat matrix, in this case it was taken: x = 50, y = 50. And each extraction square will have a height and width of 10 pixels.<br><br>
All you have to do is increase the 'x' and 'y' by 135 pixels and 124 pixels respectively. The color matrix is made up of 4 rows and 6 columns, from there they must be increased.<br><br>
The region of the square from x<sub>i</sub> to x<sub>i</sub> + width, and from y<sub>i</sub> to y<sub>i</sub> + height is extracted.<br><br>

<p align="center">
    𝐶𝑜𝑙𝑜𝑟 𝑟𝑒𝑔𝑖𝑜𝑛 = 𝑖𝑚𝑔[𝑦<sub>i</sub> : 𝑦<sub>i</sub> + ℎ𝑒𝑖𝑔ℎ𝑡, 𝑥<sub>i</sub> : 𝑥<sub>i</sub> + 𝑤𝑖𝑑𝑡ℎ]
</p>
<div align="center">
    <img width="50%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/5.png?raw=true" />
    <p>Extracted color regions.</p>
</div><br>

And, subsequently, the median of the values of said region of the image is taken for the RGB regions.<br><br>

<p align="center">
    𝑀𝑒𝑑𝑖𝑎𝑛 𝑐𝑜𝑙𝑜𝑟 = < 𝑚𝑒𝑑𝑖𝑎𝑛(𝑐𝑜𝑙𝑜𝑟 𝑟𝑒𝑔𝑖𝑜𝑛<sub>R</sub>), 𝑚𝑒𝑑𝑖𝑎𝑛(𝑐𝑜𝑙𝑜𝑟 𝑟𝑒𝑔𝑖𝑜𝑛<sub>G</sub>), 𝑚𝑒𝑑𝑖𝑎𝑛(𝑐𝑜𝑙𝑜𝑟 𝑟𝑒𝑔𝑖𝑜𝑛<sub>B</sub>) >
</p><br>

Said average value of the region must be the color that represents the color boxed in the matrix image, an average could also be made, but in this case the median was better chosen. With this average color obtained, it must be added to the matrix 'm' in position 'i'.<br><br>

<p align="center">
    𝑚<sub>i</sub> = 𝑀𝑒𝑑𝑖𝑎𝑛 𝑐𝑜𝑙𝑜r
</p><br>

In this way you can extract the information from the matrix of an image. Once the two matrices have been made (one for each given image), both the control matrix 'C' and the matrix to be calibrated 'Mo' can be used for the image calibration process.<br><br>

<h2><b>3. Differential evolution</b></h2>

The differential evolution algorithm is a stochastic search algorithm, which works through a population of solutions called vectors, of the type:

<p align="center">
    𝑥 = < 𝑥<sub>1</sub> , 𝑥<sub>2</sub> , ... , 𝑥<sub>n</sub> >
</p><br>

This algorithm is useful to explore different search areas and gradually move the population to better regions in the search space, it is based on 3 important steps: mutation, crossover and selection of survivors.<br><br>
In the mutation, for each individual x<sub>i</sub>, another called v<sub>i</sub> is generated.<br>

<p align="center">
    𝑣<sub>i</sub> = 𝑥<sup>r1</sup> + 𝐹(𝑥<sup>r2</sup> − 𝑥<sup>r3</sup> )
</p><br>

Where F is a random number between 0 and 2.

Later, in the crossover, what is sought is to combine the original vector x<sub>i</sub> with the previously created v<sub>i</sub> for the emergence of another vector called u<sub>i</sub>. This combination is made interleaved from the index 0 to k, and the decision to take for u<sub>ik</sub> the x<sub>ij</sub> or the v<sub>ij</sub>, is made by means of a probability 'Cr', which in this case is 0.5.<br><br>

<div align="center">
    <img width="30%" src="https://latex.codecogs.com/svg.latex?u_{k}^{i}=\begin{cases}v_{k}^{i},&if\;rand(0,1)\leq Cr\;\\x_{k}^{i},&otherwise\end{cases}"/>
</div><br>

And finally, in the selection of survivors, the best individual between xi and ui is selected for a tournament, and the one who survives will be part of the next generation.<br><br>

<p align="center">
    𝑥<sub>i</sub> = 𝑏𝑒𝑠𝑡(𝑥<sub>i</sub> , 𝑢<sub>i</sub>)
</p><br>

<ul>
    <li>
        <b>A. Objective function</b><br>
        What is sought to do to solve this problem is to minimize the sum of the difference between the image of the control ColorChecker matrix (I<sub>1</sub>), and the product between the image of the ColorChecker matrix that is being calibrated (I<sub>2</sub>) with the proposed matrix 'm', this will be taken as the objective function that will serve in the differential evolution algorithm, only the difference will be absolute.<br><br>
        <div align="center">
            <img width="30%" src="https://latex.codecogs.com/svg.latex?f(I_1,I_2,m)=\sum\left|I_1-(I_2\cdot m)\right|"/>
        </div><br>
    </li>
    <li>
        <b>B. Parameters</b><br>
        In this proposal, the representation of the population will be a vector of size 9, since what is sought is to optimize a 3x3 matrix 'm', which is what will be used to calibrate the image.<br><br>
        The bounds that will be used to limit the values proposed by the differential evolution algorithm range from -2 to 2, these values were taken since, when running this algorithm previously, it was possible to observe that they varied between these values, so they were selected. and they were put to improve the work of the algorithm.<br><br>
        The population size used was 30, with 1000 generations and a crossover probability of 0.5.<br><br>
    </li>
    <li>
        <b>C. Process</b><br>
        The differential evolution algorithm is called, and based on the array 'P' obtained, the function is called to calibrate the image based on the array 'P' transformed into the 3x3 matrix 'm'.<br><br>
        <p align="center">
            𝐶𝑎𝑙𝑖𝑏𝑟𝑎𝑡𝑒𝑑 𝐼𝑚𝑎𝑔𝑒 = 𝐼𝑚𝑎𝑔𝑒 𝑡𝑜 𝑐𝑎𝑙𝑖𝑏𝑟𝑎𝑡𝑒 ∙ m
        </p><br>
        Once this is done, the image is calibrated with respect to the matrix 'm' and the process is finished.<br><br>
    </li>
</ul>



<h2><b>4. Results</b></h2>

The proposed algorithm was run 100 times, where the following information could be collected.<br>

<div align="center">
    <img width="50%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/results/1.png?raw=true" />
    <p>Result of 100 runs of the algorithm.</p>
</div><br>

Of those 100 given runs, the best result was run number 48, which obtained a fitness of 1284.0, corresponding to the matrix:<br><br>

<div width="50%" align="center">
    <img src="https://latex.codecogs.com/svg.latex?;\begin{bmatrix}1.4785&-0.1431&-0.10365\\0.0565&1.4331&-1.1011\\-0.2031&-0.1527&2.0\end{bmatrix}"/>
</div><br>

While the worst result of those 100 runs was attempt number 45, which obtained a fitness of 1295.0 whose matrix was:<br><br>

<div width="50%" align="center">
    <img src="https://latex.codecogs.com/svg.latex?;\begin{bmatrix}1.5273&-0.1431&-0.10365\\0.0565&1.4331&-1.1011\\-0.2031&-0.1527&2.0\end{bmatrix}"/>
</div><br>

As can be seen, the results of the extremes are almost identical, both in the fitness and in the matrix, it only varies in the value [0,0] of the matrix; this small difference caused an 11-point variation in fitness, which is very small.<br><br>

The mean and median of these 100 results are equivalent, the mean was 1288.57 and the median was 1288.0, only 0.57 difference, so this was the most obtained result. While the standard deviation indicates that there was not much variation between results, giving a deviation of 2.467.<br><br>

The calibrated images based on these calibration matrices compared with the original image are the following:<br>

<div align="center">
    <img width="70%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/results/2.png?raw=true" />
    <p>Comparison of calibrated images.</p>
</div><br>

It can be seen that both the worst and the best solutions obtained very good results. The control matrix, being a digital image, seeks to make the colors more vivid by being 100% faithful to the original. Although a calibration that is identical to the control matrix was not achieved, very positive changes were made.<br><br>

The result matrices, in comparison with the control matrix and with the matrix that was sought to be calibrated at the beginning, are seen as follows:<br>

<div align="center">
    <img width="50%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/results/3.png?raw=true" />
    <p>Comparison of calibrated matrix.</p>
</div><br>

As previously mentioned, both the worst and the best solutions have very good results, so there is not much change.<br><br>
