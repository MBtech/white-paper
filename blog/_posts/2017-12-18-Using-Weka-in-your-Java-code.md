---
layout: post
title: "Using Weka Programmatically"
categories:
  - tutorial
  - csblog
tags:
  - tutorial
---

## Using Weka in your Java code
I had a recent project where I had to work on building up a decision tree using sequences mined from data. The decision tree will then be evaluated using Cross validation. I decided to use Weka for the task since I had used Weka a couple of years back and it is fairly simple to use in Java code.

- First of all, add Weka dependency to your maven project's pom (and I hope you are using maven or some build system. STOP doing manual builds!!)


- Secondly, you can store the data in your Java data structures into the ARFF file format that Weka can easily read from (writing to the file is not necessary, you can also format data within your Java code). A code snipped to write data into ARFF format is shown below. In this case, I have a List of `patterns` that are essentially the features for this machine learning problem. `featureMatrix` is a binary feature matrix that holds either 1 or 0 for each feature in a data point. `classification` is the class label assigned to each of the data point.

``` Java
//Write to ARFF file
List<List<String>> patterns = .....
List<String> classification = .....
List<List<Integer>> featureMatrix = .....
BufferedWriter bw;
try {
    bw = new BufferedWriter(new FileWriter("data.arff"));
    bw.write("@RELATION data\n");
    i = 0;
    for (List<String> p : patterns) {
        bw.write("@ATTRIBUTE " + p + " {0,1}\n");
        i++;
    }
    bw.write("@ATTRIBUTE class {positive,negative}\n");
    bw.write("@DATA\n");
    int index = 0;
    for (List<Integer> features : featureMatrix) {
        bw.write(features.toString().replace("[", "").replace("]", "").replaceAll(" ", "") + "," + classification.get(index) + "\n");
        index++;
    }
    bw.close();
} catch (IOException ex) {
    Logger.getLogger(TopK.class.getName()).log(Level.SEVERE, null, ex);
}
```

- Now you can read the `data.arff` file and load the data instances to use for your decision tree classifier. The following code snippet shows you how to do that:
``` Java
DataSource source;
try {
    source = new DataSource("data.arff");
    Instances instances = source.getDataSet();
    if (instances.classIndex() == -1) {
      System.out.println("reset index...");
      instances.setClassIndex(instances.numAttributes() - 1);
    }

    J48 classifier = new J48();
    Evaluation eval = new Evaluation(instances);
    eval.crossValidateModel(classifier, instances, 10, new Random(1));
    System.out.println("Accuracy: "+eval.pctCorrect());
} catch (Exception ex) {
    Logger.getLogger(TopK.class.getName()).log(Level.SEVERE, null, ex);
}
```

    Here we do a 10 fold Cross-validation of a J48 tree (open source implementation of C4.5 decision tree)

- Before using the instances read from the ARFF file, you can also perform feature selection to select only the important features. Following code snipper shows how to use Information gain for attribute evaluation and Ranker as a search method:

``` Java
AttributeSelection filter = new AttributeSelection();  
InfoGainAttributeEval eval = new InfoGainAttributeEval();
Ranker search = new Ranker();
//search.setSearchBackwards(true);
filter.setEvaluator(eval);
filter.setSearch(search);
filter.setInputFormat(instances);
// generate new data
Instances newData = Filter.useFilter(instances, filter);
```

   Then you can use this `newData` for evaluation and cross validation.

**References**
- ARFF File format: http://weka.wikispaces.com/ARFF+%28stable+version%29
- Evaluation Weka docs: http://weka.sourceforge.net/doc.stable/weka/classifiers/Evaluation.html
- Weka in your Java code: https://weka.wikispaces.com/Use+WEKA+in+your+Java+code
- Creating Weka instances out of Java data structures:
https://stackoverflow.com/questions/12118132/adding-a-new-instance-in-weka
- Instances Weka docs: http://weka.sourceforge.net/doc.dev/weka/core/Instances.html
- Attributed Selection: https://weka.wikispaces.com/Use+Weka+in+your+Java+code#Attribute%20selection
- RankSearch docs: http://weka.sourceforge.net/doc.stable/weka/attributeSelection/RankSearch.html
