# Kalman_Filtering
This repository has the code for the kalman filtering and ensemble kalman filtering.

# Imports
To use these files, you will need to import numpy, and from numpy.linalg import inv. You will also need to import matplotlib.pyplot so you can easily graph these figures and see them. The imports are included in the code.

# Kalman Filter
The Kalman filter takes in the following arguments:
A: system dynamics matrix
B: control input matrix
C: observation matrix
u: control input
P0: initial process covariance matrix
x0: initial state
y: measurements of the state
Q: process noise covariance matrix
R: observation noise covariance matrix
dT: time steps

First, we will initialize our empty matrices. We will make an empty x array that will store all our updates, with the first column being the initial state. We will then make a tuple to store all our process covariance matrices, and make the first tuple our initial process covariance matrix. 

After initialization, we can enter into our for loop. We make our prediction on what the next x and P is. Predicting what x is is easy. This is just calculating the next step in the system dynamics, using Ax + Bu. Predicting P, we use the equation, Ppred = APA.T + Q. Following the predictions, we will then update it. 

We can find the Kalman gain, K, using the predicted P. We can use the equation, K = PpredC.T(CPpredC + R)^-1. From here, we can update our x and P and store them in their respective matrices. The updated x will be given by x = xpred + K(y - Cxpred), and P = (I - KC)Ppred.

# Ensemble Kalman Filtering
The ensemble kalman filter is a variation of the kalman filter. This is used when there are many variables, such as in modeling weather patterns. This is to avoid constantly updating the process covariance matrix, thus saving on computation time and memory. Thus, instead of relying on the process covariance matrix, it creates a sample around the state based on a distribution. This distribution is assumed to be Gaussian. 

The ensemble Kalman filter takes in the following arguments: 
A: system dynamics matrix
B: control input matrix
C: observation matrix
u: control input
Sigma0: the distribution of the state
x0: initial state
y: measurements of the state
Q: process noise covariance matrix
R: observation noise covariance matrix
dT: time steps
numOfSamples: the number of samples you want to create

First, we will initialize everything. We will find the len of x0, so we can create our meanX row dimensions. meanX is the array that will store the mean of each distribution at the end of each for loop. The first set in the meanX array will store our x0 array. Then, we make our first round of samples, which we will call X. We use the np.random.multivariate_normal since it is a multivariate problem most of the time. We will enter our initial state, the distribution, and the number of samples we want. Then, we enter our for loop.

**NOTE**: The reason for the if statements checking the length is due to the fact of how np.tile works. If, for example, you have a 2x1 array, and you want to tile them to make a 2x10 array, it will make a 4x10 array. However, if the length is 1, you need to put a 1xnumOfSamples, so it will be a 2D array.

In the for loop, we make our w and z. The w is the state noise and the z is the observation noise. We then make our U and Ymeasure matrices, which is just the tiled big array of the original u and y. We then calculate our xprime, which is our predicted x. We then calculate the mean of that, and tile it. We then calculate the predicted Y, and find the yhat for that. Once we have those two values, we can find the covariance of Y and the covariance of x and y. Using these, we can find the Kalman gain, K, using a new equation. We matrix multiply the covariance of x and y by the inverse of the covariance of y. We then update our xprime, and find the mean of it and store it in our meanX array. We use the updated xprime, and propagate that distribution forward to use in our next time step.
