# Toxicity and Anti-Fraud Toolbox (Submission for Code Camp 2.0)
### **Team members**:
* Satyaki Mukherjee, Dept. of Electronics and Communication Engineering, SRM KTR Campus (*Team Leader*)
* Souparno Bhattacharyya, Dept. of Computer Science and Technology, IIEST Shibpur
### **About this repository**
While the internet has many good uses, it also has a dark side! Toxicity, bullying, threats and frauds are rampant in this internet era. This repository contains our approach to detect and prevent toxicity and financial frauds. This project has been created for Code Camp 2.0, a Virtual Hackathon from Indian Society for Technical Education Students' Chapter SRM NCR Campus.

### **Video Submission Links**:
* **Google Drive**: https://drive.google.com/file/d/16On8CSqIET8cIPaZ_LlvvR2kOisMDify/view?usp=sharing
* **YouTube**: https://www.youtube.com/watch?v=fQQJpjp7-vE

---
### **Features**
1. Detect toxic text and identify sub-types
2. Detect toxic images
3. Detect toxic videos
4. Detect toxic audio and identify sub-types
5. Detect potentially fraud messages (Both emails and SMS)
6. Credit-card fraud detection

Now, we will be doing a deep dive into each of this. But before that, what are the ingredients we need to cook this awesome dish?

### Pre-requisites
* Cuss-inspect
* Detoxify
* NudeNet
* SpeechRecognizer

The other common libraries such as numpy, pandas etc. are not being mentioned as they are installed in most systems by default nowadays.

---
### **Feature #1: Detect toxic text and identify sub-types**
Here, our *is_toxic_text()* function takes a text as input from users. After getting the input, it first detects whether the text is toxic or not. If it is toxic, then it is further classified into more sub-types such as obscene, insult, indentity-attack, threat etc. A help link is also displayed in such cases.
Let's look at a couple of examples now - 
* **Example 1**: I will kill you idiot
As you can see, this is obviously an insult and a threat. When we enter it, our function detects both of them successfully and displays the link where you can get help.
* **Example 2**: I like playing cricket
This is a just a fact stated by someone. This is obviously non-toxic, and the algorithm detects that as well properly.

**How is it useful in the real-world?**: Many digital marketing businesses, as well as PR teams of celebrities, need to filter out toxic comments from the pages which they handle. It can be a tedious task if done manually. Now, thanks to us, all they have to do is implement a scraper and connect it with this function, along with a comment removal function. Hence, this feature of ours actually has a huge business potential.

---
### **Feature #2: Detect toxic images**
Before we go into this, please note that the definition of toxic image is very much dependent on the region. What’s offensive here in India might not be offensive in the UK. However, a sub-category of this thing is offensive universally - NSFW images. This is what we have targeted here.
Our *is_toxic_image()* function takes an image as an input from the user and then classifies it as toxic or non-toxic.

**How is it useful in the real-world?**: Many digital marketing businesses, as well as PR teams of celebrities, find toxic images posted to their pages. Removing them can be a tedious task if done manually. Thanks to us, all they have to do now is implement a scraper and connect it with this function, along with a post removal script. 

---
### **Feature #3: Detect toxic videos**
A video is basically a collection of many still images. What our *is_toxic_video()* function does is check each frame and assign a probability score to each frame. By multiplying all the frame probabilities, we obtain the total unsafe probability for the entire video. We set the threshold here at 0.45. If any video exceeds this threshold, we classify it as toxic and display help. 

**How is it useful in the real-world?**: If you have reached here, then the digital marketing and PR teams example might have bored you! Let's take up a fresh example, shall we?
The best example here is the censor board. Many of them have to watch the entire video to decide which part to censor and which part to not! Further, many videos simply do not need any scissor-work!
Our algorithm can relieve them of some boredom -
* For videos which do not exceed the threshold, the censor board can pass it as Unrestricted.
* For videos which exceeds the threshold:
    * They can check the probability score and assign the rating
     * They can also check the individual frame unsafe rating and check exactly which frames might potentially need some cuts. They can check only those frames manually.

This can save the censor board a lot of time and effort!

---
### **Feature #4: Detect toxic audio and identify sub-types**
A lot of bullying, absuing and threats can happen over audio messages and calls. Our *is_toxic_audio()* function takes an audio file in .wav format. Then, it converts the audio to text using Speech Recognition technology and then does the same thing that our toxic text detection function does.

**How is it useful in the real-world?**: This function can be a life saver for the cyber-crime department. Many complaints of bullying and threats are received by them where an audio message or call recording is submitted as a proof. Instead of manually listening to all the audios individually, they can simply write a script to check all audio files and detect the toxic ones. They can then go ahead with those complaints and take appropriate action. This can significantly speed up the complaints process!

---
### **Feature #5: Detect potentially frauds messages (Both emails and SMS)**
*This is where things get a bit interesting, and our algorithm gets a bit more intelligent as well!*
To train this ML model, we have used this publicly available [dataset](https://www.kaggle.com/uciml/sms-spam-collection-dataset/).
We used 75% of this dataset as the training data and the rest as test data. We have also visualized the words occuring in spam and normal messages in Wordclouds, because almost nothing needs to be said when you have eyes!
The messages were tokenized and pre-processed using the Porter Stemmer! Next up, 2 models were trained - Bag of words and TF-IDF.
In the classification report and consequent testing, we found the TF-IDF version to be performing slightly better than the bag of words model, which is why it is present in our *is_potentially_fraud_sms()* function.

Let's take a couple of examples for this one as well -
* **Example 1**: “Hey what’s up?”
As you can see, this is a normal text and has nothing wrong in it. Hence, the model classifies it as potentially not fraud.
* **Example 2**: “Congratulations you have just won 500000”\
We all receive these, don't we? This is a common financial scam. The attackers send out this text to trap gullible people. Then they ask for some token amount from these people as acceptance for the prize, and vanish. 
We put this as an input to our model and see that it correctly identifies it as potentially fraud. It also displays where they can report this.

A small point worth noting is this model might classify some authentic messages as potentially fraud too, but in this case, false positives are way less harmful than false negatives! After all, alertness is always better than reporting (Yes, I just modified prevention is better than cure!)

**How is it useful in the real-world?**: The impact of this model is huge because while many Indians have access to 4G nowadays, they still do not have much idea about cyber crimes. Before falling for traps, they can simply copy-paste their messages here and check its authenticity, thus serving as an extra barrier between gullible users and scammers. It can also help the cybercrime department to detect scam texts, as well as the telecom operators who can perform a basic hygiene check before actually delivering any SMS sent out in bulk from one single sender!

---
### **Feature #6: Credit-card fraud detection**
To train this ML model, we have used this publicly available [dataset](https://www.kaggle.com/mlg-ulb/creditcardfraud).
The dataset contains transactions made by credit cards in September 2013 by European cardholders.
This dataset presents transactions that occurred in two days, where we have 492 frauds out of 284,807 transactions. The dataset is highly unbalanced, the positive class (frauds) account for 0.172% of all transactions.
It contains only numerical input variables which are the result of a PCA transformation. Unfortunately, due to confidentiality issues, some of the original features are not provided directly. Features V1, V2, … V28 are the principal components obtained with PCA, the only features which have not been transformed with PCA are 'Time' and 'Amount'.

This also explains why we did not go ahead with user input for this data - Users don't have access to this confidential data. However, businesses do have access to them! Which is why, we went ahead and trained a model to classify these transactions using SVM. Before fitting the data to the SVM model, we normalized it using the MinMaxScaler.

*How accurate was it? Well, not our concern here!* By simply classifying all transactions as not fraud, we get more than 99% accuracy. But that is not correct, right? This is why we are using recall. Recall is the measure of our model correctly identifying True Positives. Thus, for all the transactions which are actually fraud, recall tells us how many we correctly identified as a fraud. The recall score is around 80%, which is not bad, as we are classifying 4 out of 5 fraud transactions correctly.


**How is it useful in the real-world?** All the businesses have to do is set up a pipeline in which they will send the public as well as the anonymous details to this model. The model can identify 4 out of 5 fraud transactions correctly. This can significantly reduce manual labour for the companies, and they can mobilise this labour in combating more advanced frauds. Also, as the companies collect more and more data, this model can be retrained and fine-tuned to make it more accurate in detecting frauds! This can save a lot of money for the credit card companies. What should be our share, I wonder!

---
That's all about this repository!
Before ending, thanks to the entire team of Code Camp 2.0 for providing us with a refreshing experience amidst this pandemic. It was challenging, yet a very enriching experience for all of us.

---
