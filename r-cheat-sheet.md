# R Cheat Sheet
###### Verry much a work in progress

Get help
```
?somecommand
```

Set the width of the terminal.
```
options("width"=200)
```

List existing variables.
```
ls()
ls(all.names = TRUE)
```

Create a data vector.
```
c(1,2,3)  # Creates vector [ 1, 2, 3 ]
c(1:5)    # Creates vector [ 1, 2, 3, 4, 5 ]
```

Get the dimensions of a data set.
```
dim(data)
```

Exit the shell.
```
quit()
quit("no")  # Quit without saving (and avoid the prompt to save)
```

Plot some values.
```
x=c(0:100)
y=c(100:200)
plot(x,y)
```

Generate scatter plots for a set of data.
```
plot(dataset)
# Only include a subset of the data set
plot(Carseats[c(1,6,11)])
```

Run simple linear regression on some data.
```
lm.fit=lm(response~predictor,data=datasource)
```

Run mulitple linear regression.
```
lm.fit=lm(response~.,data=datasource) # the . indicates use all the remaining names in the data source as predictors
lm.fit=lm(response~pred1+pred2)       # use two predictors
lm.fit=lm(response~pred1+pred2+pred3+pred2:pred3) # the : is used to include the interaction term between two predictors
lm.fit=lm(response~pred1*pred2)       # use the individual predictors as well as the interaction term of the two predictors
```

Generate a contour plot for the coefficients generated from RSS (residual sum of squares).  
Stolen from [here](https://github.com/JWarmenhoven/ISLR-python/issues/1).  
TODO: generate a contour plot to displace collinearity, like the example on p.100.
```
library(ISLR)
par(mfrow=c(1,1),mar=c(5,5,2,2))
g=50
data=Auto
x=data$horsepower-mean(data$horsepower) # predictor
y=data$displacement # response
b=sum((x-mean(x))*(y-mean(y)))/sum((x-mean(x))^2)
a=mean(y)-b*mean(x)
RSS.min=sum((y-as.vector(cbind(1,x)%*%c(a,b)))^2)/100000
a.grid=seq(a-2,a+2,length=g)
b.grid=seq(b-.02,b+.02,length=g)
grid=as.matrix(expand.grid(a.grid,b.grid))

RSS=rep(0,g^2)
for (i in 1:(g^2)) {
yhat=as.vector(cbind(1,x)%*%grid[i,])
RSS[i]=sum((y-yhat)^2)/1000
}
RSS=matrix(RSS,g,g)
m=which.min(RSS)

contour(a.grid-b*mean(data$weight),b.grid,RSS,xlab=expression(beta[0]),ylab=expression(beta[1]),levels=c(833,834,835,836,837),axes=T,frame.plot=T,col=4,drawlabels=T)

points(a-b*mean(data$weight),b,col=2,pch=19,cex=1.5)
```

Compute the confidence interval of the coefficients for a linear regression.
```
confint(lm.fit)
```
