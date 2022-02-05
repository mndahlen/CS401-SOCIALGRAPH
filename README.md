# ADA-2021-Project-Team arcadie
ada-2021-project-arcadie created by GitHub Classroom

## Mapping of Social Communities Using NLP Analysis of Quotes

### Abstract
Quotebank contains millions of quotes, of which a large fraction is one person mentioning another in a semantic way. This project aims to analyse and visualise the social relationships of the most popular people in Quotebank. After going through our data pipeline utilizing diverse strategies, Quotebank turns into a neat and useful dataset. The analysis shows that there are plenty of meaningful quotes available for visualizing what we would like to see. The most frequent speakers in Quotebank are American politicians and this is not surprising as Quotebank comes from English articles and in 2020 there was the U.S. presidential election. We also confirm that the social clusters of Quotebank heavily rely on their political parties. Our conclusion is we are able to map social communities in a reasonable and realistic way.

#### Here is [our data story](https://julienadda.wixsite.com/arcadieteam)

### Research Questions
1. Can the interpersonal relationships between celebrities be realistically mapped in a visible manner using quotes between them?
2. Will the relationships in Qoutebank be more effectively visualized by adding more features?
3. Can the clusters of relationally close people, such as political parties, be detected?

### Proposed additional datasets
Apart from the quotebank dataset, an additional dataset extracted from wikidata containing all speakers in the given dataset is used. This will assist in properly mapping entity aliases to full names of entities as well as finding relevant information about the entity such as political parties. Note that this means only active speakers will be included in the graph.


### Methods
Below, we briefly explain the outline of the methods used to obtain our results. In the last part, we finally show an overview of our notebook as a schematic.
#### Load and clean Quotebank and the additional data containing QIDs and the corresponding names.
We load and clean the Quotebank dataset for years 2015-2020. In this process we select quotes that are not usable for the project such as quotes without a speaker. Furthermore we load an additional dataset containing the persons present in the Quotebank dataset as a speaker. For the persons in the Quotebank dataset the second dataset contains the following for each person: Wikidata QID, full name, list of aliases. These aliases will turn very usefull in later stages of the project to detect persons being mentioned in quotes.

#### Obtain the aliases of the persons in Quotebank
The second dataset mentioned above needs to be cleaned further: Persons such as "Joe Biden" and "Hillary Clinton" are usually called by their surnames "Joe" and "Hillary". However, many other people have those aliases aswell! To fix this problem we aggregate the number of times every person was a speaker, for each person with alias "Joe", only the top speaker gets to keep the alias. This luckily turned out to be "Joe Biden" and likewise for many other mainstream persons.

#### Create graph with persons as nodes and sentiment scores as weights
1.15 million people were selected from Quotebank for each year between 2015-2020. For each quote, the quote was scanned using Spacy Named Entity Recognition (NER) for mentioned aliases such as "Joe", "Hillary", "Trump" or "Ben". Each quote was assigned a speaker, a mentioned person and the quote itself. Only quotes with only one mentioned person was kept. For each kept quote, the sentiment of the quote was analysed using NLTK giving a compound sentiment score between -1 and 1. The sentiment scores were used together with a distance function to compute weights for the graph. In cases were persons had several mentions of eachother each mention had to be averaged into one weight only. Thus between two persons there are only one edge. Note that the main graph we used for community detection only contained the 500 most popular persons with quotes only from year 2020. 

#### Analyse graph to detect communities
The graph was analysed using the Louvain community detection method. An important step was to fine tune our weight function slightly to obtain realistic results. For example we set a requirement for the Biden community to be mainly democrat and for the trump community to be mainly republicans. No other respect was paid to the persons in the communities. This way of tuning the weight function turned out to work well and gave several realistic communities: Sports, cultures and two political parties of Democrats and Republicans respectively. 

The Louvain method was performed using an external program called Gephi, which also allowed us to make beautiful plots of the graph.

#### Plot the graph using gephi (not included on the jupyter notebook)
To construct our graphs, we utilized a useful graphical tool 'gephi'. This tool provides various functions for customizing graphs. An example is shown below. 

![WhatsApp Image 2021-12-17 at 7 53 46 PM](https://user-images.githubusercontent.com/77029774/146601447-9f63f867-4f9d-45e1-a071-fa53ff54dfd5.jpeg)

This allows us to see if there are distinguishable clusters of the entitis. We verified if there is any correspondence between the clusters found in the data and the political parties, or another fertured groups.

#### This is a schematic of our pipeline in the notebook.
Below we show an overview of the notebook structure which in more detail explains the step above.


![Readmepng_FINAL](https://user-images.githubusercontent.com/73229139/146611005-39b5d21d-882f-4d95-a3e0-59816e030b6e.png)


### Proposed timeline
- 19 November : Take into account Milestone 2 feedback.
- 26 November : Find a way to visually represent our nodes and weights (using the quotes of 2020).
- 3 December : Add in our dataset the quotes from the years prior 2020, and generate their graphs. 
- 10 December : Analysis of our graphs (e.g: clustering) to prepare visualized materials.
- 17 December : Finalize the git repository, create a cool storyboard with meaningful figures and submit.

### Contributions
Julien: Problem formulation : big picture, coding the technical part of the algorithm, data analysis, read-me picture, figures/animation on the website, data analysis and finalizing the submission;
Martin: Problem formulation, coding the technical part of the algorithm, coming up with the algorithm and finalizing the submission;
Stefan: Problem formulation, coming up with the algorithm, graph creation and communities detection and analysis, website development;
Donggyun: Coding up the algorithm, plotting graphs during data analysis, writing up the readme and finalizing the submission.
