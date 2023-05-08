Download Link: https://assignmentchef.com/product/solved-syde372-lab-3-image-classification
<br>



<h1>1         Overview</h1>

The goal of this lab is to provide some context for classification in real-world situations. One particularly interesting application of classification is image classification, where content within an image is categorized into one of several classes. In this lab, image classification is performed on a set of texture images using both labelled classification (MICD) and unlabelled clustering (K-means).

Texture analysis is an important aspect of computer vision that depends on image classification. Examples include interpretation of remotely sensed images, determining depth cues from scenes, and identification of pertinent objects in medical imaging. In order to assist theoretical development and to be able to compare results from different studies, a standard set of texture images, the Brodatz images, is commonly used in research literature.

The necessary Matlab scripts, features, and image files for this lab can be found in a Zip file on the course homepage:

http://ocho.uwaterloo.ca/∼pfieguth/Teaching/372/sd372.html

For this lab, ten texture image files (*.im) are used. The texture images can be read and viewed in Matlab using the following commands:

image = readim(’cloth.im’) ; imagesc(image) ; colormap(gray) ;

<h1>2         Feature Analysis</h1>

Each texture image is eitherfrom each image. Let <em>d </em>(<em>α,</em>256<em>β</em>) be the gray level value of pixel×256 or 256×128 pixels in size. We will select sixteen(<em>α,β</em>) in the <em><sub>j</sub></em><em>th </em>block of the<em>n </em>×<em><sub>i</sub></em><em>th</em><em>n </em>image.blocks

<em>ij</em>

We propose two simple features which measure the variability of each image in the horizontal and vertical directions:

(1)

cloth         cotton        grass        pigskin        wood          cork          paper         stone         raiffa           face

Figure 1: Sample image blocks from which features are derived. The top row consists ofand the bottom rowdiscriminate by eye based on the smaller8 × 8 blocks for each texture respectively. Clearly the textures are much harder to8×8 feature blocks, so we would expect a classifier trained from32×32 blocks

these features to have a poorer performance.

Luckily, all of the features have been pre-calculated and provided in the ’feat.mat’ Matlab data file. The data file contains three feature matrices f2, f8, and f32 which contain the features calculated from each of the 16 blocks for all 10 images for <em>n</em>=2, 8, and 32 respectively. Separate matrices f2t, f8t, f32t are provided as test data. Each column in each matrix represents one image block and is arranged out as follows:

(2)

where <em><u>x</u></em><em><sub>ij </sub></em>is the feature vector in image <em>i </em>at block <em>j</em>.

To plot the features for a particular feature set, use the following commands:

load feat.mat aplot(f2);

You will notice that the feature points are plotted using letters. Each letter corresponds to a different texture in the following order: cloth, cotton, grass, pigskin, wood, cork, paper, stone, raiffa, face. Sample blocks from each texture image are shown in Figure 1.

Keep in mind how our two features are defined in (1):

<ol>

 <li>By looking at the texture images themselves, (e.g., on the SD372 home page), which do you think would be most likely to be confused with the other images? Why?</li>

 <li>Which images are likely to be most distinct? Why?</li>

</ol>

Be sure to answer the questions in the context of the features extracted from the texture images.

<table width="229">

 <tbody>

  <tr>

   <td width="42"> </td>

   <td colspan="3" width="80">Classified</td>

   <td width="28"> </td>

   <td width="27"> </td>

   <td width="27"> </td>

   <td width="27"> </td>

  </tr>

  <tr>

   <td width="42">Truth</td>

   <td width="27">1</td>

   <td width="27">2</td>

   <td width="27">3</td>

   <td width="28">…</td>

   <td width="27">8</td>

   <td width="27">9</td>

   <td width="27">10</td>

  </tr>

  <tr>

   <td width="42">1</td>

   <td width="27">14</td>

   <td width="27">1</td>

   <td width="27">0</td>

   <td width="28">…</td>

   <td width="27">1</td>

   <td width="27">0</td>

   <td width="27">0</td>

  </tr>

  <tr>

   <td width="42">2</td>

   <td width="27">1</td>

   <td width="27">15</td>

   <td width="27">0</td>

   <td width="28">…</td>

   <td width="27">0</td>

   <td width="27">0</td>

   <td width="27">0</td>

  </tr>

  <tr>

   <td width="42">3</td>

   <td width="27">0</td>

   <td width="27">0</td>

   <td width="27">13</td>

   <td width="28">…</td>

   <td width="27">0</td>

   <td width="27">1</td>

   <td width="27">0</td>

  </tr>

  <tr>

   <td width="42">…</td>

   <td width="27">…</td>

   <td width="27">…</td>

   <td width="27">…</td>

   <td width="28">…</td>

   <td width="27">…</td>

   <td width="27">…</td>

   <td width="27">…</td>

  </tr>

  <tr>

   <td width="42">8</td>

   <td width="27">1</td>

   <td width="27">0</td>

   <td width="27">0</td>

   <td width="28">…</td>

   <td width="27">11</td>

   <td width="27">1</td>

   <td width="27">0</td>

  </tr>

  <tr>

   <td width="42">9</td>

   <td width="27">0</td>

   <td width="27">0</td>

   <td width="27">1</td>

   <td width="28">…</td>

   <td width="27">1</td>

   <td width="27">12</td>

   <td width="27">0</td>

  </tr>

  <tr>

   <td width="42">10</td>

   <td width="27">0</td>

   <td width="27">0</td>

   <td width="27">0</td>

   <td width="28">…</td>

   <td width="27">0</td>

   <td width="27">0</td>

   <td width="27">16</td>

  </tr>

 </tbody>

</table>

Table 1: An example confusion matrix: In this example, Image 1 was classified correctly fourteen times, classified incorrectly as Image 2 once, etc.

<h1>3         Labelled Classification</h1>

<ol>

 <li>For each of the three feature matrices f2, f8 and f32, develop the MICD classifier. You are not required to plot the classification boundaries. Apply the classifier to the test data f2t, f8t and f32t.</li>

 <li>What was the misclassification rate for each image and for each <em>n</em>? Prepare three confusion matrices, each like the one shown in Table 1, one table for each value of <em>n</em>, to compare how the images are classified.</li>

 <li>Compare and explain the results for the different values of <em>n</em>.</li>

</ol>

<h1>4         Image Classification and Segmentation</h1>

In the ’feat.mat’ data file are two large matrices, multim and multf8. The matrix multim contains a image that is composed of multiple different textures. The matrix multf8 contains the corresponding feature vector for each pixel using <em>n </em>= 8. The matrix elements multf8(i,j,1), multf8(i,j,2) are the two features corresponding to row <em>i</em>, column <em>j </em>of the image.

Use your MICD classifier corresponding to <em>n </em>= 8 to classify each pixel in multf8. Create an array cimage such that cimage(i,j) contains the classified class number. Plot the result using imagesc(cimage)

State your observations. How do the classified regions in the resulting classified image cimage compare to original texture image multim?

<h1>5         Unlabelled Clustering</h1>

For this section, we will perform unlabelled clustering to perform unsupervised image classification on the texture images we’ve classified earlier using the MICD classifier. For this section we will treat the data points, <em><u>x</u></em><em><sub>ij</sub></em>, as <em>unlabelled </em>points in feature space. Implement the K-means algorithm using the features in the f32 feature matrix only.

<ol>

 <li>Assume <em>K </em>= 10. Pick ten points randomly from your data <em><u>x</u></em><em><sub>ij </sub></em>and use these ten points as your initial prototypes.</li>

 <li>Apply the K-means algorithm using the selected prototypes. Plot the position of the 10 converged prototypes superimposed on the data <em><u>x</u></em><em><sub>ij</sub></em>.</li>

 <li>Now repeat step 2 except using the fuzzy K-means approach with <em>b </em>= 2.</li>

 <li>Repeat steps 2 and 3 a few more times (with different, random initial prototypes each time). It is not necessary to submit any plots for these trials; however, compare the variability in the clustering from your trials. State your conclusions and observations. Furthermore, qualitatively compare your results from the unlabelled clustering with the results you obtained using the MICD classifier. How do they compare?</li>

</ol>

Take a look again at the data points in matrix multf8. These are unlabelled data – without looking at multim, we don’t actually know which pixel location belongs to which class. How well do you think K-means would work on the data in multf8? Could we use unlabelled clustering to classify the image multim?

<h1>6         Report</h1>

Include in your report:

<ul>

 <li>A brief introduction.</li>

 <li>Discussion of your implementations and results.</li>

 <li>Printouts of pertinent graphs (properly labelled).</li>

 <li>M-files for each section.</li>

 <li>Include responses to all questions.</li>

 <li>A brief summary of your results with conclusions.</li>

</ul>