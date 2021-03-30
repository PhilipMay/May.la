XGBoost
=======

Hyperparameter
--------------

Useful hyperopt search space:

.. code:: python

   space = {
       'max_depth' : hp.quniform('max_depth', 3, 20, 1),
       'gamma' : hp.uniform('gamma', 0, 10),
       'subsample' : hp.uniform('subsample', 0.1, 1),
       'colsample_bytree' : hp.uniform('colsample_bytree', 0.1, 1),
       'eta' : hp.uniform('eta', 0.05, 1),
   }

Links
-----

-  Root Documentation:
   https://xgboost.readthedocs.io/en/latest/index.html

   -  Python API Reference:
      https://xgboost.readthedocs.io/en/latest/python/python_api.html
   -  XGBoost Parameters:
      https://xgboost.readthedocs.io/en/latest/parameter.html
   -  Notes on Parameter Tuning:
      https://xgboost.readthedocs.io/en/latest/tutorials/param_tuning.html
   -  Data Interface:
      https://xgboost.readthedocs.io/en/latest/python/python_intro.html#data-interface
   -  Load and save the booster:
      https://xgboost.readthedocs.io/en/latest/python/python_intro.html#training
   -  Using XGBoost External Memory Version:
      https://xgboost.readthedocs.io/en/latest/tutorials/external_memory.html

-  What is XGBoost: https://www.kaggle.com/dansbecker/xgboost
-  Complete Guide to Parameter Tuning in XGBoost:
   https://www.analyticsvidhya.com/blog/2016/03/complete-guide-parameter-tuning-xgboost-with-codes-python/
