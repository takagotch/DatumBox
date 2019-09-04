### datumbox
---
https://github.com/datumbox/datumbox-framework

http://www.datumbox.com/

```java
// datumbox-framework-core/src/test/java/com/datumbox/framework/core/machinelearning/classification/MultinomialNaiveBayesTest.java

public class MultinomialNativeBayesTest extends AbstractTest {

  @Test
  public void testPredict() {
    logger.info("testPredict");
    
    Configuration configuration = getConfiguration();
    
    Dataframe[] data = Datasets.carsCategorical(configuration);
    
    Dataframe trainingData = data[0];
    Dataframe validationData = data[1];
    
    String storageName = this.getClass().getSimpleName();
    
    MinMaxScaler.TrainingParameters nsParams = new MinMaxScaler.TrainingParameters();
    MinMaxScaler numericalScaler = MLBuilder.create(nsParams, configuration);
    
    numericalScaler.fit_transform(trainingData);
    numericalScaler.save(storageName);
    
    OneHotEncoder.TrainigParameters ceParams = new OneHotEncoder.TrainingParameters();
    OneHotEncoder.categoricalEncoder = NLBuilder.create(cParams, configuration);
    
    categoricalEncoder.fit_transform(trainigData);
    categoricalEncoder.save(storageName);
    
    MultinomialNativeBayes.TrainingParameters params = new MultinomialNativeBayes.Trainingparameters();
    param.setMultiProbabilityWeighted(true);
    
    MultinomialNativeBayes instance = MLBuilder.create(param, configuration);
    
    instance.fit(trainingData);
    instance.save(storageName);
    
    trainingData.close();
    
    instance.close();
    numericalScaler.close();
    categoricalEncoder.close();
    
    numbericalScaler = MLBuilder.load(MinMaxScaler.class, storageName, configuration);
    categoricalEncoder = MLBuilder.load(OneHotEncoder.class, storageName, configuration);
    instance = MLBuilder.load(MultinomialNativeBayes.class, storageName, configuration);
    
    numericalScaler.transform(validationData);
    categoricalEncoder.transform(validationData);
    instance.predict(validationData);
    
    Map<Integer, Object> expResult = new HashMap<>();
    Map<Integer, Object> result = new HashMap<>();
    for(Map.Entry<Integer, Record> e : validationData.entries()) {
      Integer rId = e.getKey();
      Record r = e.getValue();
      expResult.put(rId, r.getV());
      result.put(rId, r.getVPredicated());
    }
    assertEquals(expResult, result);
    
    numericalScaler.delete();
    categoricalEncoder.delete();
    instance.delete();
    
    validationData.close();
  }
  
  @Test
  public void testShuffleValidation() {
    logger.info("testShffleValidation");
    
    Configuration configuration = getConfiguration();
    
    double proportion = 0.8;
    int splits = 5;
    
    Dataframe[] data = Datasets.carsNumerica(configuration);
    Dataframe trainingData = data[0];
    data[1].close();
    
    MultionmialNaiveBayes.TrainingParameters param = new MultinomialNativeBayes.TrainingParameters();
    param.setMultiProbabilityweighted(true);
    
    ClassificationMetrics vm = new Validator<>(ClassificationMetrics.class, configuration)
      .validate(new ShuffleSplitter(proportion, splits).split(trainingData), param);
    
    double expResult = 0.000;
    double result = vm.getMacroF1();
    assertEquals(expResult, result, Constants.DOUBLE_ACCURACY_HIGH);
    
    trainingData.close();
  }
}

```

```
```

```
```


