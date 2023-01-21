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
        <th align="center" width="50%">
            Programming Language
        </th>
        <th align="center" width="50%">
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
<br><br>

<!-- <h2><b>Demo (not available yet)</b></h2>

Currently there is no live visualization, since it is planned to migrate the database to MongoDB and make a modification to the Front-End to improve the view, which is currently very basic. As well as switching from JavaScript to TypeScript.
<br><br> -->

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
            <img width="80%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/codes/1.png?raw=true" />
            <p>Images comparation.</p>
        </div><br>
    </li>
</ol>
<br><br>

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
<br><br>

<h2><b>Methodology</b></h2>

Below is a full explanation of the methodology used.

<h3><b>1. Color matrix extraction</b></h3>
<ul>
    <li>
        <b>A. Load images</b><br>
        Firstly, you need to load the two images to be used, the image that contains the control matrix and another image which will contain the same matrix, but in a different context (different lighting and position). Once these images are loaded, you can proceed to extract the color matrix.<br><br>
        <div align="center">
            <img width="60%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/1.png?raw=true" />
            <p>Image of the letter 'A' after going through image processing.</p>
        </div><br>
    </li>
    <li>
        <b>B. Cut matrix</b><br>
        This is a method that receives the image where the color matrix is located, which you want to cut in order to extract the information. This method must be applied for both images.<br><br>
        Firstly, the area of the image (height x width) is obtained, this for later steps. In order to do a good image processing, the image must be transformed to a gray scale, and after that apply an inverse binary thresholding, it has to be inverse so that the matrix is taken with values of 1, otherwise it will take parts that are not wanted.<br><br>
        <div align="center">
            <img width="60%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/2.png?raw=true" />
            <p>Image of the letter 'A' after going through image processing.</p>
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
            <img width="60%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/3.png?raw=true" />
            <p>Image of the letter 'A' after going through image processing.</p>
        </div><br>
    </li>
</ul>

<h3><b>2. Transformation of the matrix</b></h3>

A deformation process will be applied to the matrix cut or extracted from the image so that, regardless of the angle at which said matrix is, it remains in a flat context for better manipulation for data extraction.<br><br>
Firstly, the coordinates of the vertices of said matrix must be arranged to your liking, in this case the order must be clockwise, starting from the upper left vertex, that is: Top-Left, Top-Right, Bottom-Right, Bottom-Left.<br><br>
Once these vertices are arranged in the desired order, the dimensions where the image of the deformed matrix will reside must be created. In this case, the distances between two points must be obtained, obtaining the width A and B, and the height A and B, this is because a perfect rectangle cannot always result, normally it will have the shape of a trapezoid.<br><br>

As the final image is intended to be a rectangle that maintains the proportions of the matrix, the maximum value must be taken for each width and each height.<br><br>

<p align="center">
    𝑚𝑎𝑥𝑊𝑖𝑑𝑡ℎ = 𝑚𝑎𝑥(𝑤𝑖𝑑𝑡ℎ𝐴, 𝑤𝑖𝑑𝑡ℎ𝐵)<br>
    𝑚𝑎𝑥𝐻𝑒𝑖𝑔ℎ𝑡 = 𝑚𝑎𝑥(ℎ𝑒𝑖𝑔ℎ𝑡𝐴, ℎ𝑒𝑖𝑔ℎ𝑡𝐵)
</p><br>

Once the maximum height and width values are obtained, an array can be made with the dimensions of the new image, following the order of clockwise and starting from the top left. Here you must put the values in which each vertex will be.<br><br>

<p align="center">
    𝑁𝑒𝑤𝐼𝑚𝑎𝑔𝑒 = < [0,0], [𝑚𝑎𝑥𝑊𝑖𝑑𝑡ℎ − 1,0], [𝑚𝑎𝑥𝑊𝑖𝑑𝑡ℎ − 1, 𝑚𝑎𝑥𝐻𝑒𝑖𝑔ℎ𝑡 − 1], [0, 𝑚𝑎𝑥𝐻𝑒𝑖𝑔ℎ𝑡 − 1] >
</p><br>

Once the maximum height and width values are obtained, a perspective transformation can be performed with the original points and the destination points. Having this, a perspective transformation algorithm is performed in order to provide you with more information about the required information of the given image.<br><br>
<div align="center">
    <img width="60%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/4.png?raw=true" />
    <p>Image of the letter 'A' after going through image processing.</p>
</div><br>
To finish, simply resize the image to 800x500 pixels, this to have a constant image size for data extraction, otherwise it may vary in size and affect the last process.<br><br>

<h3><b>3. Taking of colors</b></h3>

A matrix 'm' of the size of the color matrix is created, which has 4 rows and 6 columns, therefore, it will be 4x6 in size.<br><br>
You must determine in which position (x, y) you want to start to extract the colors in the image of the flat matrix, in this case it was taken: x = 50, y = 50. And each extraction square will have a height and width of 10 pixels.<br><br>
All you have to do is increase the 'x' and 'y' by 135 pixels and 124 pixels respectively. The color matrix is made up of 4 rows and 6 columns, from there they must be increased.<br><br>
The region of the square from xi to xi + width, and from yi to yi + height is extracted.<br><br>

<p align="center">
    𝐶𝑜𝑙𝑜𝑟 𝑟𝑒𝑔𝑖𝑜𝑛 = 𝑖𝑚𝑔[𝑦𝑖 : 𝑦𝑖 + ℎ𝑒𝑖𝑔ℎ𝑡, 𝑥𝑖 : 𝑥𝑖 + 𝑤𝑖𝑑𝑡ℎ]
</p>
<div align="center">
    <img width="60%" src="https://github.com/ASASauqui/Color-calibration/blob/main/readme_images/methodology/5.png?raw=true" />
    <p>Image of the letter 'A' after going through image processing.</p>
</div><br>

And, subsequently, the median of the values of said region of the image is taken for the RGB regions.<br><br>

<p align="center">
    𝑀𝑒𝑑𝑖𝑎𝑛 𝑐𝑜𝑙𝑜𝑟 = < 𝑚𝑒𝑑𝑖𝑎𝑛(𝑐𝑜𝑙𝑜𝑟 𝑟𝑒𝑔𝑖𝑜𝑛𝑅), 𝑚𝑒𝑑𝑖𝑎𝑛(𝑐𝑜𝑙𝑜𝑟 𝑟𝑒𝑔𝑖𝑜𝑛𝐺), 𝑚𝑒𝑑𝑖𝑎𝑛(𝑐𝑜𝑙𝑜𝑟 𝑟𝑒𝑔𝑖𝑜𝑛𝐵) >
</p><br>

Said average value of the region must be the color that represents the color boxed in the matrix image, an average could also be made, but in this case the median was better chosen. With this average color obtained, it must be added to the matrix 'm' in position 'i'.<br><br>

<p align="center">
    𝑚𝑖 = 𝑀𝑒𝑑𝑖𝑎𝑛 𝑐𝑜𝑙𝑜r
</p><br>

In this way you can extract the information from the matrix of an image. Once the two matrices have been made (one for each given image), both the control matrix 'C' and the matrix to be calibrated 'Mo' can be used for the image calibration process.<br><br>




<h2><b>4. Results</b></h2>

As previously mentioned, the results of the models used for the detection of letters and numbers were very good, having, in the case of the model for letters, an accuracy of 0.908 and an R2 of 0.844; and, in the case of the number model, an accuracy of 0.949 and an R2 of 0.872.<br>
The results delivered by the proposed methodology, in general, were very good, being able to read plates that have a weak contrast or that have different colors, it is even capable of doing its work on plates with a bit of unnecessary saturation.<br>
In general, from an exclusive dataset to verify the effectiveness of the algorithm, made up of 148 plates from different countries, types, colors, fonts and text locations, it managed to hit 80 plates perfectly (without any type of error), others 43 plates had only one error and the remaining ones had more than one error. To observe the number of errors, the Levenshtein distance algorithm was used to compare two strings. The standard deviation of 1.312 indicates that there is usually an error between predictions, and the average distance gives us 0.831, indicating the same as the standard deviation, that it is possible that there may be an error for each prediction between 4-9 characters. who usually owns a license plate.<br>
Obviously, the best results were given in boards of the European, Japanese, Chinese, Argentinian, Russian, etc. type, because these boards have little information saturation and, normally, the contrast of the components is very high. In the case of license plates in the United States, the results were mixed, since license plates in this country can be customized and tend to have an excessive saturation of components and colors, making it difficult to recognize the serial code. But, even if they are diverse, as long as the letters can be discerned, the information can be read correctly.<br>
Therefore, these success rates could be increased if only plates from China, Russia, Japan, etc. had been placed, and, conversely, decreased if only complex plates had been placed. For this reason, various plates were put on it, to avoid any type of "favoritism" towards a type of plate.<br>
However, the methodology presents some confusion between similar characters, hence the problem that arose in most of the 43 plates that presented a single error. The characters that are often confused are the following: between 'G' and '6'; between '0' and '0'; between 'B' and '8'; between 'D' and 'O'; between 'I' and '1'; between 'Z' and '2'; etc. This can reduce the accuracy of the model, but the confusion is understandable, since at the time of image processing some samples may have remained similar and hence the errors between these characters with similarities. In addition to that, in themselves, these pairs of characters tend to be very similar.<br><br>

<div align="center">
    <img width="40%" src="https://github.com/ASASauqui/Car-license-plate-detector-and-reader/blob/main/Readme%20Images/methodology/methodology_9.png?raw=true" />
    <p>Some predictions.</p>
</div>
