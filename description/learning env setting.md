# learning env setting

left comment on code about explanation

- import

  ```python
  import os
  import shutil
  import numpy as np
  import tensorflow as tf
  
  from tensorflow.keras.metrics import Mean, CategoricalAccuracy
  ```



#### dir_setting

This method create directory for save model and learning data 

```python
'''
CONTINUE_LEARNING : whether about restart or not of training
	CONTINUE_LEARNING == True : when restart the model training because training stop unintentionally before
	CONTINUE_LEARNING == False : start training from first epoch
''' 

def dir_setting(dir_name, CONTINUE_LEARNING):
    cp_path = os.path.join(os.getcwd() , dir_name)
    # create new directory for save model and learning data when every each epoch
    
    model_path = os.path.join(cp_path, 'model')
    # new directory for save model
    
    if CONTINUE_LEARNING == False and os.path.isdir(cp_path):
        # if left some file or information when the training start at first
        shutil.rmtree(cp_path)
        # delete all file on cp_path 

    if not os.path.isdir(cp_path):
        os.makedirs(cp_path, exist_ok=True)
        os.makedirs(model_path, exist_ok=True)
        # pass if the path exist. or not, create directory on path 
    
    path_dict = {'cp_path' :cp_path, 
                'model_path': model_path}
    return path_dict
```



#### continue_setting

The setting method for continue learning

```python
'''
CONTINUE_LEARNING : whether about restart or not of training
	CONTINUE_LEARNING == True : when restart the model training because training stop unintentionally before
	CONTINUE_LEARNING == False : start training from first epoch
model : saved model before
''' 

def continue_setting(CONTINUE_LEARNING, path_dict, model):
    if CONTINUE_LEARNING == True and len(os.listdir(path_dict['model_path'])) == 0:
        # if CONTINUE_LEARNING is True but nothing in model directory, 'CONTINUE_LEARNING' is 'False' 
        CONTINUE_LEARNING = False
        print("CONTINUE_LEARNING flag has been converted to FALSE") 

    if CONTINUE_LEARNING == True:
        
        epoch_list = os.listdir(path_dict['model_path']) 
        # get epoch list from 'model_path' 
        
        # the directory names of each epoch are 'eapoch_number'
        epoch_list = [int(epoch.split('_')[1]) for epoch in epoch_list]
        epoch_list.sort()

        last_epoch = epoch_list[-1]
        model_path = path_dict['model_path'] + '/epoch_' + str(last_epoch)
        # save the path of last epochs directory
        model = tf.keras.models.load_model(model_path)
        # load the model in the path
        
        losses_accs_path = model_path
        losses_accs_np = np.load(losses_accs_path +'/losses_accs.npz')
        # load the data about learning saved when training before

        losses_accs = dict()
        for k, v in losses_accs_np.items():
            losses_accs[k] = list(v)
            # recreate losses_accs dictionary

        start_epoch = last_epoch + 1 
        # setting the epoch number that new start training
    else:
        model = model
        start_epoch = 0
        losses_accs = {'train_losses': [], 'train_accs': [],
                        'validation_losses': [], 'validation_accs': []}
        # create empty losses_accs dictionary

    return model, losses_accs, start_epoch

```



#### get_classification_metrics

create dictionary named 'metric_objects' for carrying keras.metrics method

```python
def get_classification_metrics():
    train_loss = Mean()                     
    train_acc = CategoricalAccuracy() 

    validation_loss = Mean()                
    validation_acc = CategoricalAccuracy()

    test_loss = Mean()                
    test_acc = CategoricalAccuracy()

    metric_objects = dict()
    metric_objects['train_loss'] = train_loss
    metric_objects['train_acc'] = train_acc
    metric_objects['validation_loss'] = validation_loss
    metric_objects['validation_acc'] = validation_acc
    metric_objects['test_loss'] = test_loss
    metric_objects['test_acc'] = test_acc

    return metric_objects
```





## Full code

```python
import os
import shutil
import numpy as np
import tensorflow as tf

from tensorflow.keras.metrics import Mean, CategoricalAccuracy


# CONTINUE_LEARNING = True      training??? ???????????? ????????? ?????? ????????? ?????? Model??? training??? ????????? ????????? ??? 
# CONTINUE_LEARNING = False     training??? ???????????? ?????? ????????? ??? (?????? ?????? ????????? ???)

# ---------project??? ?????? directory??? ???????????? ??????---------
def dir_setting(dir_name, CONTINUE_LEARNING):
    cp_path = os.path.join(os.getcwd() , dir_name)
    # ?????? path?????? ??? directory path ??????

    model_path = os.path.join(cp_path, 'model')
    # cp_path ?????? ?????? ????????? ????????? directory
    
    if CONTINUE_LEARNING == False and os.path.isdir(cp_path):
        # training??? ???????????? ?????? ??????????????? ????????? ????????? ???????????? ???
        shutil.rmtree(cp_path)
        # cp_path ????????? ?????? file delete

    if not os.path.isdir(cp_path):
        os.makedirs(cp_path, exist_ok=True)
        os.makedirs(model_path, exist_ok=True)
    
    path_dict = {'cp_path' :cp_path, 
                'model_path': model_path}
    return path_dict

# ---------model learning??? ????????? ?????? ????????? ????????? ????????? stop???????????? ?????? learning ?????? ??????---------
def continue_setting(CONTINUE_LEARNING, path_dict, model):
    if CONTINUE_LEARNING == True and len(os.listdir(path_dict['model_path'])) == 0:
        # CONTINUE_LEARNING??? True??????, model directory??? ???????????? ?????????
        CONTINUE_LEARNING = False
        print("CONTINUE_LEARNING flag has been converted to FALSE") 

    if CONTINUE_LEARNING == True:
        # ----- model return -----
        epoch_list = os.listdir(path_dict['model_path']) 
        # 'model_path' ?????? object??? ????????? ????????? ???????????? ?????? training data??? ?????? ????????????.
        #  ????????? training data??? ??? directory ????????? epoch_n ?????? ??? ???
        epoch_list = [int(epoch.split('_')[1]) for epoch in epoch_list]
        # epoch.split('_') = [epoch, n] ?????? ????????? n = 1 ????????? ?????? ??? ????????? 
        # epoch_list = [1 for epoch in epoch_list] ??? ?????? ?????????.
        epoch_list.sort()

        last_epoch = epoch_list[-1]
        model_path = path_dict['model_path'] + '/epoch_' + str(last_epoch)
        # ????????? epoch??? ????????? model_path??? ??????
        model = tf.keras.models.load_model(model_path)
        # ????????? ?????? model??? load
        
        losses_accs_path = model_path
        losses_accs_np = np.load(losses_accs_path +'/losses_accs.npz')

        losses_accs = dict()
        for k, v in losses_accs_np.items():
            losses_accs[k] = list(v)
            # losses_acc_np??? ?????? ????????? dict??? ??????

        start_epoch = last_epoch + 1 
        # ?????? ????????? training ????????? ??????
    else:
        model = model
        start_epoch = 0
        losses_accs = {'train_losses': [], 'train_accs': [],
                        'validation_losses': [], 'validation_accs': []}
        # ??? dict??? ???????????????.

    return model, losses_accs, start_epoch


# ---------train, validation, test ????????? loss??? accuracy metrics ?????? dict?????? ??????---------
def get_classification_metrics():
    train_loss = Mean()                     # training??? ????????? loss function
    train_acc = CategoricalAccuracy() # accuracy??? ????????? function
    # function_name1 = function1()

    validation_loss = Mean()                # validation??? ????????? loss function
    validation_acc = CategoricalAccuracy()

    test_loss = Mean()                # test??? ????????? loss function
    test_acc = CategoricalAccuracy()


    metric_objects = dict()
    metric_objects['train_loss'] = train_loss
    metric_objects['train_acc'] = train_acc
    metric_objects['validation_loss'] = validation_loss
    metric_objects['validation_acc'] = validation_acc
    metric_objects['test_loss'] = test_loss
    metric_objects['test_acc'] = test_acc

    return metric_objects
```

