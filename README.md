# PrometheeOptimization

An implementation of Promethee 2 based on the "PROMETHEE is not quadratic: An O(qnlog(n)) algorithm" [paper](https://www.sciencedirect.com/science/article/pii/S0305048317303729) by Toon Calders and Dimitri Van Assch.

# Building/Running

## Build
To build our project just run ```make``` command in the root directory.

## Run
You can use main.py to manager all your Promethee runs. First, must be specified Promethee version, Fast Promethee is our more stable and fast version, -fp is his flag. To use Umbu version -um must be used and to Vanilla version -van.

Fast Promethee and Umbu use tiff input format, while Vanilla uses txt input format.  

## Input format

### Tiff Format
You must process each criterion separated and finally merge everyone.
Umbu and Fast Promethee share some flags to must be used, like as:
- Raster name
- Weight of criteria
- Preference functions and preference functions parameter
- Flag to define if criteria are directly proportional to values, otherwise not use this flag.
Sample as:
```
python main.py -fp AEMMF.tif 0.5 -type=linear 1 -ismax
```
But Fast Promethee has two optionals flags:
- The number of the process which can be used to calculate promethee (Default: 1).
- Define the max sum MB of RAM used in the all process which will run the algorithm(Default: 1024).

```
python main.py -fp AEMMF.tif 0.5 -type=linear 1 -size=2048 -proc=4
```

Umbu has one optional flag:
- The number of alternatives which will be saved in one chunk.
```
python main.py -um AEMMF.tif 0.5 -chunk=100000000 -type=linearWithIndifference 1 0.5
```

Umbu and Fast Promethee only support linear and linearWithIndifference functions and pixels Not-a-Number are treated as out of the zone of study.

### Txt Format
The input data should be specified in two directories; one describes the evaluation of the alternatives to each considered criterion, the other describes the preference function to compare alternatives. 

In the alternatives directory, the user should add a file to each criterion, following the `NAME_OF_CRITERION.input` convention. These files indicate the evaluation of each alternative according to the criterion specified, in a matrix format, separated by an empty space and new lines separator. 

The second directory, which specifies the preference functions, should have a file to each criterion, using the `NAME_OF_CRITERION.meta` convention. Each file should follow the pattern:
```
weight
function_name
parameters
is_max
```
In which `weight` is the value of weight of the criterion specified, `function_name` is the name of the function, `parameters` are the parameters, separated by an empty space, for the specific function, and `is_max indicates` if in the comparison greater values are desired over the smaller ones (0 if smaller values are desired, 1 for greater values).

Example:
```
0.2
level
0.5 1
1
```

We support all the prefence functions described in the literature. See below how each one of them should be declared in the `function_name` parameter:

```
Usual => usual
Quasi (U-shape) => quasi
Linear (V-shape) => linear
Level => level
Linear with Indifference => linearWithIndifference
Gaussian => gaussian
```

For the optimized version, we support the Linear shape (as the proposed method is restricted to Linear functions). Note that, however, our implementation only supports linear comparison, one can use the parameter `p = 0` and linear comparison will behave usual comparison.

#### Out of Zone of Study

In cases where not all of the alternatives must be counted to calculate the Flow, it is possible to ignore them by using "nan" (case sensitive) in the `NAME_OF_CRITERION.input` files.

Example:
```
nan 2.0 nan
3.5 4.8 6.5
nan nan 5.7
```
The alternative is determined by row and column in the matrix, so "nan" must be in the same positions to all criterion matrix.

## References

[PROMETHEE is not quadratic: An O(qnlog(n)) algorithm](https://www.sciencedirect.com/science/article/pii/S0305048317303729)<br>
[A Preference Ranking Organisation Method: The PROMETHEE Method for Multiple Criteria Decision-Making](https://www.jstor.org/stable/2631441)
