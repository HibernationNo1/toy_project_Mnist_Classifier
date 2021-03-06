# READ ME

I Implement Classifier Model with Convolution Neural Network.

This project shows the changing of loss and accuracy value for every epoch, because the code constructed training, validation, test be implemented in same iteration `for` loop.  

And the training, validation, test data saved in every epoch as a file. I intend  'confusion matrix' and  'loss, accuracy plot'  saved in 'epoch_number' directory as `.png` file, After end of the each epoch, we can check 'confusion matrix' and  'loss, accuracy plot'. 

I also implement it can continue learning in case after the training stop unintentionally during learning .



**Contents**

- [Functions](#Functions)
  - [main.py](#main.py)
  - [learning_env_setting.py](#learning_env_setting.py)
  - [dataset_utils.py](#dataset_utils.py)
  - [train_validation_test.py](#train_validation_test.py)
  - [basic_utils.py](#basic_utils.py)
  - [cp_utils.py](#cp_utils.py)
  - [model.py](#model.py)
- [Conclusion](#Conclusion)
- [Getting Started](#'Getting Started')
- [Version](#Version)



## Functions

I describe about method and model in markdown file.  Open the file or click the rink for details about method.



#### 0. [main.py](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/description/main.md)

Hyperparameter setting, call setting method, model training, validation, and test by `for` loop



#### 1. [learning_env_setting.py](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/description/learning%20env%20setting.md)

- **dir_setting**

  create directory for save learning data and save the path.

- **get_classification_metrics**

  save objects that compute loss, accuracy in the 'metric_objects' dictionary.

- **continue_setting**

  if the model's learning is unintentionally interrupted, help it can learning continue .

  

#### 2. [dataset_utils.py](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/description/dataset%20utils.md)

- **load_processing_mnist**

  Load Mnist data of tensorflow and do pre-processing.



#### 3. [train_validation_test.py](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/description/train_validation_test.md)

- **go_train**

  learning with data for train.

- **go_validation**

  validation with data for validation, and return value for confusion_matrix .

- **go_test**

  test with data for test and save the loss and accuracy value as .txt file.

  

#### 4. [basic_utils.py](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/description/basic_utils.md)

- **resetter**

  reset the object in `metric_objects` to initial state in every epoch.

- **training_reporter**

  print the loss and accuracy about validation, train data in every epoch.

  

#### 5. [cp_utils.py](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/description/cp_utils.md)

- **save_losses_model**

  save the model learned so far.

- **loss_acc_visualizer**

  draw the plot about loss, accuracy of validation by twinx line plot during full epoch and save as .png file.

- **confusion_matrix_visualizer**

  draw the confusion_matrix plot and save as .png file.




#### 6. [model.py](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/description/Model.md)

- **MnistClassifier**

  I make classifier by keras.model sub-classing, Sequential API.

  in description file, I Implemented forward propagation of Conv2D and Danse layers.



**Diagram**

![](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/image/active%20diagram.png?raw=true)

> dark line : express the steps of source code running.
>
> blue line : express the parameter that give to function



## Conclusion

I Implement Classifier Model with Mnist data of tensorflow_dataset. And train this network to 50 epoch for half an hour and achieve accuracy 98.80% on the Mnist validation data set.

I make this project for experience that Implement model and utility code like construction of common thesis code. There were many difficulties in the process, I solve those problems by using Stack overflow, Naver Knowledge IN and open kakao community.



- Confusion Matrix on validation dataset when 50 epoch 



![](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/image/Confustion_matrix_visualization.png?raw=true)

- loss and accuracy line plot on train and validation dataset for 0 to 50 epoch

![](https://github.com/HibernationNo1/project_Mnist_Classifier/blob/master/image/Losses_accs_visualization.png?raw=true)



## Getting Started

```
$ code\main.py
```



## Version

| name             | version |
| ---------------- | ------- |
| python           | 3.8.5   |
|                  |         |
| **package name** |         |
| numpy            | 1.19.2  |
| tensorflow       | 2.4.1   |
| matplotlib       | 3.3.2   |

