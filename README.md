# PartF
Ais2101 - Part F

## Part 1 code solution

clear;
clc;


data = readtable('penguins.csv');

cvp = cvpartition(height(data),'HoldOut',0.3);
idxTrain = training(cvp);
idxTest = test(cvp);

trainData = data(idxTrain,:);
testData = data(idxTest,:);

inputData = data(:, 1:end-1);
outputData = data(:, end); 

inputArray = table2array(inputData);
outputArray = table2array(outputData);

inputArray = double(categorical(inputArray));
outputArray = double(categorical(outputArray));


fis = genfis(inputArray,outputArray);


# This code opens the csv file then inputs it into a input and output array witch is then run through a genfis function.

##  After tuning with GA, PSO, ANFIS the solution is represented with this code:
%% Tune FIS

outputTuned1 = evalfis(fisout_PSO,inputArray);
outputTuned2 = evalfis(fisout_ANFIS, inputArray);
outputTuned3 = evalfis(fisout_GA, inputArray);

plot([outputArray,outputTuned1, outputTuned2, outputTuned3])
legend("Expected Output","Tuned PSO Output","Tuned ANFIS Output","Tuned GA Output","Location","southeast")
xlabel("Data Index")
ylabel("Output value")

Giving us the graph:

![image](https://github.com/AndreFug/PartF/assets/67748209/a5172e80-f47a-47c8-bfe9-a4eaa53406a1)


With the clc output:


## Single Objective Optimization

### Genetic Algorithm (GA)

- **Options:**
  - CreationFcn: @gacreationuniform
  - CrossoverFcn: @crossoverscattered
  - SelectionFcn: @selectionstochunif
  - MutationFcn: @mutationadaptfeasible

#### Generational Approach

| Generation | Func-count | Best f(x) | Mean f(x) | Stall Generations |
|------------|------------|-----------|-----------|------------------|
| 1          | 400        | 2.533     | 1114      | 0                |
| 2          | 590        | 2.533     | 933.1     | 1                |
| 3          | 780        | 1.928     | 706.1     | 0                |
| 4          | 970        | 1.928     | 509.7     | 1                |
| 5          | 1160       | 1.599     | 350.1     | 0                |
| 6          | 1350       | 1.593     | 232.8     | 0                |
| 7          | 1540       | 1.13      | 152.3     | 0                |
| 8          | 1730       | 1.109     | 95.76     | 0                |
| 9          | 1920       | 1.109     | 62.07     | 1                |
| 10         | 2110       | 1.109     | 35.94     | 2                |

#### Iteration Approach

| Iteration | f-count | Best f(x) | Mean f(x) | Stall Iterations |
|-----------|---------|-----------|-----------|------------------|
| 0         | 100     | 2.552     | 1216      | 0                |
| 1         | 200     | 2.552     | 1067      | 0                |
| 2         | 300     | 2.552     | 735.8     | 1                |
| 3         | 400     | 2.552     | 427.7     | 2                |
| 4         | 500     | 2.175     | 181.4     | 0                |
| 5         | 600     | 1.551     | 112       | 0                |
| 6         | 700     | 1.299     | 89.08     | 0                |
| 7         | 800     | 1.049     | 61.01     | 0                |
| 8         | 900     | 0.8756    | 74.27     | 0                |
| 9         | 1000    | 0.8756    | 47.5      | 1                |
| 10        | 1100    | 0.8756    | 32.38     | 2                |

### Adaptive Neuro-Fuzzy Inference System (ANFIS)

- **Number of nodes:** 55
- **Number of linear parameters:** 80
- **Number of nonlinear parameters:** 24
- **Total number of parameters:** 104
- **Number of training data pairs:** 335
- **Number of checking data pairs:** 0
- **Number of fuzzy rules:** 16

#### Training ANFIS

```
Start training ANFIS ...

1    0.252328
2    0.252296
3    0.252264
4    0.252232
Step size increases to 0.011000 after epoch 5.
5    0.2522
6    0.252168
7    0.252133
8    0.252099
Step size increases to 0.012100 after epoch 9.
9    0.252064
10   0.25203

Designated epoch number reached. ANFIS training completed at epoch 10.

Minimal training RMSE = 0.25203
```
