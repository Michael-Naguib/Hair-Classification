# Hair-Classification
For nearly all of the Fall 2020 Semester, Several of my friends have been arguing over the color my friend's hair! So I decided to settle the matter or at least provide machine intelligence input on the subject! 

## Task Description
- Produce a (semi-explainable) model to classify the color of an individuals hair based on a sample image containing only the hair and nothing else
- The algorithm should also be able to consider human input and weighting as a factor when classifying a target

## Model & Approach
- For the model I decided to use a K-Nearest Neighbors model to sample a set of known colors in HLS Color Space
- HLS color space is is a representation of Color space considering a three component vector of **Hue**, **Lightness**, & **Saturation**
![HLS Color Space](https://raw.githubusercontent.com/Michael-Naguib/Hair-Classification/main/HLSspace.PNG "HLS Color Space")
- Using this space **Euclidian Distance** metric seamed like it would provide a decent fit; (an enhanced metric might weight components differently but that is discussed later... )
- The model made an important assumption: *the effects of lighting on hue could be mitigated via large sampling* meaning slight variations in color due to lighting could still be classified so long as the predefined color classification space was large enough! 
- The main idea was that by sampling what people consider Brown or Blond etc this would form a sort of overlaping grouping in color space where both density of volume per color and occupied volume matter and that a KNN would be excelent at this sort of classification

## Training Data
Training data consisted of several components
- 1) Manually Entered Data for colors: For this part of the data colors were manually input into arrays corresponding to their classification. Colors were taken from various 3rd party online sources which stated the classification of various RGB colors.
![Manual Data Blond](https://raw.githubusercontent.com/Michael-Naguib/Hair-Classification/main/MBlond.PNG "Manual Data Blond")
![Manual Data Brown](https://raw.githubusercontent.com/Michael-Naguib/Hair-Classification/main/MBrown.PNG "Manual Data Brown")
etc... for the other colors

- 2) The two parties with stake in the outcome were asked to blindly (with respect to the algorithm) submit several examples of what they each consider brown and blond hair etc... some examples are provided below
![example](https://raw.githubusercontent.com/Michael-Naguib/Hair-Classification/main/brown/brown234643.PNG "example")
![example](https://raw.githubusercontent.com/Michael-Naguib/Hair-Classification/main/blond/blond1.PNG "example")
Pixel colors were then extracted from these images and put in their respective categories
- All colors were converted to HLS and shuffled
- Colors were considered without regard to count in the training data: i.e the data under-went a preprocessing step where duplicates were removed **HOWEVER** if a color was in both classification categories it was considered valid (both classifications claim that color = null effect in Target classification as described below)
- Due to the fact that the algorithm is density sensitive and that it was noticed that the generated training dataset was skewed in the theoretical sense that simply there exist more blond colors than brown and more black colors than the both of those, the sets contining the HLS values for each of the classifications was randomly sampled according to the lowest unique color count. 
- The final Dataset can be visualized in HLS Space
![Dataset HLS](https://raw.githubusercontent.com/Michael-Naguib/Hair-Classification/main/DatasetInHLSspace.PNG "Dataset HLS")


## Classification of a Target
- Classification would be performed by classifying each individual pixel in an image then considering the classification for each pixel as a vote towards the overall classification of the image. 
- This final vote decomposition was provided as the image's classification
- The model was extended to classify more than just the original target colors of in the dispute (brown and blond) due to the fact that this might form a false dichotemy; The [Voting Spoiler effect](https://en.wikipedia.org/wiki/Vote_splitting#:~:text=The%20spoiler%20effect%20is%20the,both%20or%20several%20to%20win.) is actually desireable here!


## Training 
- For the KNN K-Folding was implemented and the final classification model was done by selecting the model which achieved the best performance on K neighbors

![KValueSelection](https://raw.githubusercontent.com/Michael-Naguib/Hair-Classification/main/KValueSelection.PNG "KValueSelection")


## Further Considerations
- It might be useful to construct a differnet metric or even take a Genetic Algorithm approach to constructing a metric for the space which boosts the accuracy of the classification
- The Model is very tending to overfit! As such for training, excluding images of the targets proved beneficial and still classification yeilded reasonable results
- A side effect of the model is that the shape of the hair can mean it has some pixels that are more do to the layout of the hair and less due to the color of the hair ... 


## Technical Notes
- ```Cair-Classification.ipynb``` contains the classification code and can be extended to classify any hair color by adding the appropiate data & folders
- The folders ```black```, ```blond```, etc contain images that are used to generate the training data for the classification code. The names of these folders and which color they represent are specified in the code. 
- (hence you could add a color to the classification by creating a new folder that is the color name then specifying the approtiate data in the classification code)

# So Was he Blond?
- The short answer is **NO** 
- But the model did show that some of the samples of his haircolr had a non insignifigant amount of blond. And in light of a new metric which would weight the Hue dimension in HLS space more might have differnt results