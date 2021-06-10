# Content Based Image Retrieval 

## Abstract 
The process known as Radon transformation has been typically used to create barcodes which are used to tag medical images. This process works by taking a sampled image and transforming it into several projections (usually 4 or 8). These projections are then used to generate barcodes. [3] The generated barcode is then compared with the MNIST database of images using the Hamming distance between the input barcode and of each image on the MNIST database. Database images with a Hamming distance below a set threshold are outputted. Both of these processes were used in order to generate algorithms. In this paper, we propose two different algorithms, each with its own designated task. The first algorithm used to create a barcode for each 28-by-28-pixel image provided in the MNIST dataset. The second algorithm is used to then take that corresponding barcode and then utilize it to search for the most similar image in the given dataset. This is done by using the comparing the barcode of the query image with other barcodes in order to find the most similar image. Subsequently, experiments were conducted to report the retrieval accuracy of the algorithms, and the algorithm complexity was analyzed based on Big-O-Notation.
## 1. Introduction

Generally, searching for an image in a set database is typically done through something known as Content-Based Imaged Retrieval (CBIR). Content-Based Image Retrieval works with the image itself rather than assigning specific text or data to it to search for it. Earlier CBIR systems identified a certain image by its description of visual characteristics like color, shape, texture, and other significant physical details. In 2015, an idea was proposed to use Radon barcode for image retrieval system in the medical field [3]. This was inspired by the many other products that use barcodes in some shape or form. Radon barcode is binary code that is generated by Radon Transformation and uses projection angles to tag an image. Using Radon barcodes is much more efficient than previous Content-Based Image Retrieval methods since it is a lot easier to search for specific images using Hamming distance. Hamming distance between two-bit strings that are equal length is the number of positions where corresponding bits are different, thus the images that are most like one another have the lowest Hamming distance [4].

## 2. Algorithms
**A. MNIST Dataset Barcode Generation**

The radon barcode generation algorithm works by creating projections for the angles 0, 45, 90 and 180 degrees for the chosen image. These projections are created and for this array a threshold mean is found. Where after, it is binarized to 1’s and 0’s where if the number were larger than the mean it would be set to 1 and 0 otherwise. After, the binarization of this array it becomes appended to the RBC array. After, the addition of this array the while loop will create a new angle through θ ← θ + 180/nθ. This will iterate through depending on the number of projections chosen, for this specific algorithm the while loop will run 4 times. Finally ending with a binary array with the dimension size of 112 x 1. This array can be reduced in size, with approximately seven digits on either size to retain accuracy. Ultimately, our final barcode remained at 112 x 1 due to the small dataset size. But for a larger dataset, the algorithm run time would decrease with a smaller binary array comparison.
Radon projections is the staple of the algorithm 1, where it focuses on finding the sum of an array in specific angles. For 4 projections, as previously state it will occur at 0, 45, 90 and 180. The figure below showcases, a simple implementation of radon projections for a 3 x 3 array. In our specific case, we will be working with a 28 x 28 down sampling size. Which ultimately means, that our array will be far larger. Our radon projections per angle will have a 28 x 1. With our full RBC array having an array size that can be denoted by: 

𝑅𝐵𝐶 =𝐷𝑜𝑤𝑛𝑠𝑎𝑚𝑝𝑙𝑖𝑛𝑔 𝑥 𝑁𝑢𝑚𝑏𝑒𝑟 𝑜𝑓 𝑃𝑟𝑜𝑗𝑒𝑐𝑡𝑖𝑜𝑛𝑠

Which means that the RBC array will have a size of 112 x 1, after all four projections are appended.

<p align = "center">
<img src="https://user-images.githubusercontent.com/76674104/114340501-afc5c080-9b25-11eb-8d27-6641517698d5.png "/>
</p>

**Figure 1** *Radon Barcode, Projections (P1, P2, P3, P4) binarized after radon projections are created. The binarization is dependent on the threshold mean.*

**Algorithm 1** Radon Barcode Generation [9]

1: Import all libraries
2: Initialize Radon Barcode r ← θ
3: Set down sampling size R<sub>N</sub> ← 28
4: Set number of projection angles n<sub>θ</sub> ← 4
5 for i in range of 60,000 images:
6: Initialize angle θ ← 0
7: Get query image I
8: Downsample I: I = Normalize(I,RN)
9: while θ < 180 do
10: Get all projections p for θ
11: Find typical value Ttypical ← mediani(pi)|pi≠0
12: Binarize projections: b ← p > Ttypical
13: Append the new row r ← append(r, b)
14: θ ← θ + 180/nθ
15: end while
16: Create new data frame row
17: Send the full data frame to the excel file
18: Store RBC in Hamming array for comparison
19: Return r


**B. Radon Barcode Comparison Using Hamming Distance**

Similarity and error detection are generally found by calculating the frequency of dissimilarity between two data sets. These principles are applied when calculating Hamming Distance. It being a metric for comparing two binary datasets of equal length, Hamming Distance is the number of positions in which the two bits are different. In our application Hamming Distance is fundamentally calculated by comparing the least significant bit in each RBC, if the values differ; a value is added to the counter. Then the least significant bit is popped from each RBC and a Boolean false is appended to each RBC, shifting them to the right. These orders of operations are performed until a Boolean false value is present in each index of each RBC. 

