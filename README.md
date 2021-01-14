# CH8 Homework

* 執行程式
```
$ sh run_complete_detector.sh
```
## 程式解析
1. 由於 sh檔內 args.malware_paths 、args.benignware_paths 、args.evaluate都有輸入，所以會執行
 ```python
 X, y = get_training_data(args.benignware_paths,args.malware_paths,hasher)
 cv_evaluate(X,y,hasher)
 ```
 2. get_training_data
    這函式是將 malware_paths 和 benignware_paths 內的檔案取得 training data
    
    <font size=4> X : 從 get_string_features() 得到檔名的特徵值 </font>
    <font size=4> y : [ 1 x malware_paths 內檔案數量 ] [0 x benignware_paths 內檔案數量] </font>
    
3. cv_evaluate
   * RandowForestClassifier
    使用隨機森林決策樹model 進行訓練
    ```
    classifier = RandomForestClassifier(64)
    classifier.fit(training_X,training_y)
    scores = classifier.predict_proba(test_X)[:,-1]
    ```
    scores就是預測出來的結果
   
   * metrics.roc_curve
	```
	fpr, tpr, thresholds = metrics.roc_curve(test_y, scores)
	```
     以 test_y 為標準值 1為陽性 0為陰性
   
      test_y 的index 對應 scores 的index 
   
     接下來選取一個閾值計算TPR/FPR,閾值的選取規則是在scores值中從大到小的以此選取。
     scores中大於等於閥值的就設為1 小於閥值就是0
	  公式:
   
     ```
    FPR = FP / (FP+TN)
    TPR = TP/ (TP + FN)
     ```

    FPR : test_y 陰性對應 score中 大於等於閥值中的比例

    TPR : test_y 陽性對應 score中 大於等於閥值中的比例

   <font size=5>閥值會從大到小重複以上公式 </font>


4. 最終產生曲線圖

![](https://i.imgur.com/CIpwGK0.png)
