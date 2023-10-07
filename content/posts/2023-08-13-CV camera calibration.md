



# camera Calibration

A robot(XY moving platform) is holding a camera. How can I get the coord of an obj in picture captured by camera.

### 1. Simplest situation

simplest situation is linear calibration. It output a linear scale for your coord tranformation.

```
  picture 1:                                     picture 2:                          
  +-----------------------+                     +-----------------------+           
  |                       |                     |                       |           
  |                       | camera moving left  |                       |           
  |  +----+               |                     |                +----+ |           
  |  |obj |               | ------------------> |                |obj | |           
  |  |    |               |                     |                |    | |           
  |  +----+               |                     |                +----+ |           
  |                       |                     |                       |           
  |                       |                     |                       |           
  +-----------------------+                     +-----------------------+           
                                                                          
```

for `picture 1 `: we have a robot_coord(2, 0), obj_px_coord(30, 60)

for `picture 2 `: we have a robot_coord(-2, 0), obj_px_coord(170, 60)

**The question is:** 

- what is the robot_coord when the obj_px_coord is (100, 60) ?

Analysis:   2,0 ---> linear mapping --> 30,60

​                  -2,0 ---> linear mapping --> 170,60

so we have:  $\Delta x = -2 - 2 = -4$,  $\Delta x_{pixle} = 170 - 30 = 140$, $\Delta y = 0$,  $\Delta y_{pixle} = 0$ 

linear scale on x: $\text{scale} = \frac{\Delta x}{\Delta x_{pixle}} = \frac{-4}{140} = \frac{-1}{35}$

now we need to know the new `x`value that makes the `x_pixcle == 100`

**The solution is:** 

for pixle_coord:  $x_{orig\_pixle} + \Delta x_{pixle2} = 100$, $x_{orig\_pixle} = 30$, so $\Delta x_{pixle2}$ = 70.

for robot_coord: $\Delta x_2 = x_{target} - x_{orig} = \text{scale} \times \Delta x_{pixle2} = \frac{-1}{35}  \times 70 = -2$

So, $x_{tar} = x_{orig} - 2 = 0$



### 2. 





