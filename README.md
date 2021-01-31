# indianpolitics
NLP Project for detecting nationalism in Indian political articles

-TAKEN DIRECTLY FROM MEDIUM ARTICLE-

Over the course of the last few years, political tension in India has deepened rapidly. Indeed, the world has seen a growing movement of nationalism within India, fraught with rhetoric perpetuated by both leaders of the BJP (the current party in power, who have increasingly utilized a more nationalistic platform) controlled government as well as citizen-led institutions (the Rashtriya Swayamsevak Sangh organization for instance). It coincided with Hindu nationalists’ growing desire to reclaim the Indian identity, and as a result nationalism has found a place in every sector of Indian society. The motivation behind this project is to investigate one of those sectors: political written media. We aimed to build an algorithm that, when given the text of an Indian political article, could find the article’s attitudes towards both the national government as well as towards Muslims to identify whether or not the article was right-wing/nationalist.

Explanation of Our Data
Given that our objective was to run an algorithm on an entire news article, we required a dataset that had multiple articles in full length. We found our dataset on Kaggle that contained the information of over 15,000 Indian political articles from 2018. It included the author, the article content, and the website source among other characteristics. Initially we believed that far right leaning news websites would produce more nationalist articles. However, after reading through many articles we found that other than the article content itself, none of the other information from the dataset was useful to our goal. Thus, all of the later algorithms would rely almost entirely on the article content alone.


Methods
It’s important to note that determining whether or not a source nationalist and/or right-wing varies from source to source. As a result, our approach simplifies the criteria, and our training set was labeled with the interpretations of our authors. We decided that a simultaneous positive attitude towards the BJP and Modi and a negative attitude towards Islam, Muslims, and Pakistan would constitute a nationalist article.

Thus, we decided that we would take data from our original set of Indian political articles and label them as either nationalist or nonnationlist by first loading them into a dataframe called “politics” and then creating a column of nan values and then modifying each with “0” for nonnationalist and a “1” for nationalist. After doing so, we could filter out the table by only including articles that had been identified with a 1 or 0.

Because there weren’t enough nationalist articles in our dataset, we made a separate table and imported nationalist articles that we found online, with the same format except this time the ‘nationalist’ column with nan, 0, or 1 were all 1’s.

After combining those tables, we then had our cumulative table that we could split into our testing and training set. We used scikitlearn’s library to do a 79/21 split of our labelled articles between our testing and training set respectively.

After we had our sets ready, we created a function that would count the occurrences of our words of interest in each article. The words we wanted to measure the positivity around were “BJP,” “modi,” “rss,” and “Hindutva.” The words we wanted to measure the negativity around were “islam” and “Pakistan.” The counts of these words would later become our features.

The next step would involve using VADER sentiment analysis. In short, as part of the Natural Language Toolkit, it is a model that can measure the positivity and negativity of text. For each respective score, the closer the number is to 1 the more intense the emotion is within that text. Thus, we defined a function that would iterate through the text of an article, find each occurrence of the particular word in question, use VADER sentiment analysis to gauge the positivity/negativity scores of ±2 sentences from each occurrence, and then average their scores together to assign a positivity for each word. Each words’ positivity/negativity scores would then be averaged together to create a comprehensive emotion score for each article in the set.

Finally, we would run this process on each article in both our testing and our training set such that each article had attached features of positivity, negativity, the count of each of the aforementioned words, and whether or not the article was nationalist. All values except for the last one were standardized. Then, the training and test with all of the standardized features were split into an “x” and “y” table table with the latter containing only each article’s nationalist binary and the “x” table containing every feature except for that. We used XGBoost’s classifier to fit the data using the training set, and then ran the algorithm on the test set and compared it to our labels.

Conclusions
Initially we believed that a logistic regression model would be best for a binary classification problem. When we trained the model it had a 71% accuracy and 89% precision, however only a 62% recall on the test set.

We then researched and applied various other models and achieved varying results. The next model we tested was an SVC model. This model performed worse however, with an accuracy of 67%, precision of 67% and recall of 68%.

We then tried an XGBoost model. This model performed much better with an accuracy of 81%, precision of 81%, recall of 80%, and F1 Score of 82% on the test set.
Because the XGBoost classification model had the greatest accuracy, precision, and recall, we decided to choose that as our final model.

Further Improvements
The largest limiting factors were the time as well as resources we had at our disposal. A more extensive study could rely on experts to classify the articles’ nationalist status, as well as increase the volume of articles that the algorithm trains off of. Given a larger set to work from, we would be able to construct a more comprehensive list of features around which we could measure the attitudes towards. Additionally, in further studies we would study the proportion of the target words in the article as opposed to the count as to make the standardization of data more accurate.
Along with improving the algorithm, in the future we would like to implement a program that makes the algorithm usable on websites. Ideally we would like to create an extension that would be able to extract a news article from a website and run our algorithm on it.

Primary Packages Used:
Pandas
Numpy
scikit-learn
NLTK (VADER sentiment analysis)
Matplotlib
Seaborn
XGBoost
