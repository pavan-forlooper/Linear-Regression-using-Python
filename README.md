# Linear-Regression-using-Python
Linear Regression using Python


{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Task 1: Introduction"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "%matplotlib inline\n",
    "\n",
    "import numpy as np\n",
    "import matplotlib.pyplot as plt"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Task 2: Dataset\n",
    "\n",
    "Real estate agent table:"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "|Area|Distance|Price|\n",
    "|---|---|---|\n",
    "|70|3|21200|\n",
    "|50|1|22010|\n",
    "|120|9|24305|\n",
    "|100|2|31500|\n",
    "\n",
    "You can write the relationship with a 2-variable linear equation:\n",
    "\n",
    "$\n",
    "\\begin{equation}\n",
    "y = b + w_1.x_1 + w_2.x_2\n",
    "\\end{equation}\n",
    "$\n",
    "\n",
    "In a vector form:\n",
    "\n",
    "$\n",
    "\\begin{equation}\n",
    "y = b + (w_1 w_2).\\binom{x_1}{x_2}\n",
    "\\end{equation}\n",
    "$\n",
    "\n",
    "Where\n",
    "$\n",
    "\\begin{equation}\n",
    "W = (w_1 w_2)\n",
    "\\end{equation}\n",
    "$\n",
    "and\n",
    "$\n",
    "\\begin{equation}\n",
    "X = \\binom{x_1}{x_2}\n",
    "\\end{equation}\n",
    "$"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def generate_examples(num=1000):\n",
    "    W = [1.0, -3.0]\n",
    "    b = 1.0\n",
    "    \n",
    "    W = np.reshape(W, (2, 1))\n",
    "    \n",
    "    X = np.random.randn(num, 2)\n",
    "    y = b + np.dot(X, W) + np.random.randn()\n",
    "    \n",
    "    y = np.reshape(y, (num, 1))\n",
    "    \n",
    "    return X, y"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "X, y = generate_examples()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "print(X.shape, y.shape)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "print('X=', X[0], ' & y=', y[0])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Task 3: Initialize Parameters"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The loss over **m** examples:\n",
    "\n",
    "$\n",
    "\\begin{equation}\n",
    "J = \\frac{1}{2m} \\sum_{i=1}^{m} (y - \\hat{y})^2\n",
    "\\end{equation}\n",
    "$\n",
    "\n",
    "The objective of the gradient descent algorithm is to minimize this loss value.\n",
    "\n",
    "Gradient Descent Objective is to \n",
    "$\n",
    "\\begin{equation}\n",
    "min(J)\n",
    "\\end{equation}\n",
    "$"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "class Model:\n",
    "    def __init__(self, num_features):\n",
    "        self.num_features = num_features\n",
    "        self.W = np.random.randn(num_features, 1)\n",
    "        self.b = np.random.randn()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "model = Model(2)\n",
    "print('W=', model.W)\n",
    "print('b=', model.b)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Task 4: Forward Pass"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The gradient descent algorithm can be simplified in 4 steps:\n",
    "\n",
    "1. Get predictions y_hat for X with current values of W and b.\n",
    "2. Compute the loss between y and y_hat\n",
    "3. Find gradients of the loss with respect to parameters W and b\n",
    "4. Update the values of W and b by subtracting the gradient values obtained in the previous step\n",
    "\n",
    "Let's simplify our linear equation a bit more for an example:\n",
    "$\n",
    "\\begin{equation}\n",
    "y = wx\n",
    "\\end{equation}\n",
    "$\n",
    "\n",
    "Let's plot J as a function of w\n",
    "\n",
    "![Loss vs Param](JvsW.png)\n",
    "\n",
    "The gradients of loss with respect to w:\n",
    "\n",
    "\\begin{equation}\n",
    "\\frac{dJ}{dw} = \\frac{\\delta{J}}{\\delta{w}} = \\lim_{\\epsilon \\to 0} \\frac{J(w + \\epsilon) - J(w)}{\\epsilon}\n",
    "\\end{equation}"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "class Model(Model):\n",
    "    def forward_pass(self, X):\n",
    "        y = self.b + np.dot(X, self.W)\n",
    "        return y"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "preds = Model(2).forward_pass(np.random.randn(4, 2))\n",
    "print(preds.shape)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Task 5: Compute Loss\n",
    "\n",
    "The loss over **m** examples:\n",
    "\n",
    "$\n",
    "\\begin{equation}\n",
    "J = \\frac{1}{2m} \\sum_{i=1}^{m} (y - \\hat{y})^2\n",
    "\\end{equation}\n",
    "$"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "class Model(Model):\n",
    "    def compute_loss(self, y, y_true):\n",
    "        loss = np.sum(np.square(y - y_true))\n",
    "        return loss/(2*y.shape[0])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "model = Model(2)\n",
    "\n",
    "preds = model.forward_pass(X)\n",
    "loss = model.compute_loss(y, preds)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "loss"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Task 6: Backward Pass\n",
    "\n",
    "The gradient of loss with respect to bias can be calculated with:\n",
    "\n",
    "$\n",
    "\\begin{equation}\n",
    "\\frac{dJ}{db} = \\frac{1}{m} \\sum_{i=1}^{m} (\\hat{y^{(i)}} - y^{(i)})\n",
    "\\end{equation}\n",
    "$\n",
    "\n",
    "$\n",
    "\\begin{equation}\n",
    "\\frac{dJ}{dW_j} = \\frac{1}{m} \\sum_{i=1}^{m} (\\hat{y^{(i)}} - y^{(i)}).x_j^{(i)}\n",
    "\\end{equation}\n",
    "$"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "class Model(Model):\n",
    "    def backward_pass(self, X, y_true, y_hat):\n",
    "        m = y_hat.shape[0]\n",
    "        db = np.sum(y_hat - y_true)/m\n",
    "        dW = np.sum(np.dot(np.transpose(y_hat - y_true), X), axis=0)/m\n",
    "        return dW, db"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "model = Model(2)\n",
    "X, y = generate_examples()\n",
    "y_hat = np.zeros(y.shape)\n",
    "\n",
    "dW, db = model.backward_pass(X, y, y_hat)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "print(dW.shape, db.shape)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Task 7: Update Parameters"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "class Model(Model):\n",
    "    def update_params(self, dW, db, lr):\n",
    "        self.W = self.W - lr * np.reshape(dW, (self.num_features, 1))\n",
    "        self.b = self.b - lr * db"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Task 8: Training Loop"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "class Model(Model):\n",
    "    def train(self, x_train, y_train, iterations, lr):\n",
    "        losses = []\n",
    "        for i in range(iterations):\n",
    "            y_hat = self.forward_pass(x_train)\n",
    "            dW, db = self.backward_pass(x_train, y_train, y_hat)\n",
    "            self.update_params(dW, db, lr)\n",
    "            loss = self.compute_loss(y_hat, y_train)\n",
    "            losses.append(loss)\n",
    "            if i % 100 == 0:\n",
    "                print('Iter: {}, Current loss: {:.4f}'.format(i, loss))\n",
    "        return losses"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "model = Model(2)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_train, y_train = generate_examples()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "losses = model.train(X_train, y_train, 1000, 3e-3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "plt.plot(range(1000), losses);"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Task 9: Predictions"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "model_untrained = Model(2)\n",
    "\n",
    "X_test, y_test = generate_examples(500)\n",
    "y_test = np.reshape(y_test, (y_test.shape[0], 1))\n",
    "\n",
    "preds_untrained = model_untrained.forward_pass(X_test)\n",
    "preds_trained = model.forward_pass(X_test)\n",
    "\n",
    "plt.figure(figsize=(6, 6))\n",
    "plt.plot(preds_untrained, y_test, 'rx')\n",
    "plt.plot(preds_trained, y_test, 'bo')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "model.W"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "model.b"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.8"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
