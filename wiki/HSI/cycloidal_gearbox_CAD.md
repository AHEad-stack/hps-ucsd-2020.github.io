# Cycloidal Gearbox

Use [Desmos](https://www.desmos.com/calculator/s5uort2vxz) in order to create and refine the parameters for your gears.
C is the radius of the pins inside of which the cycloidal gears will rotate.
R is the radius of the pins.
N is the number of pins. The gear reduction of the cycloidal drive will be equal to 1/(N-1).
E is the eccentricity of the driveshaft, basically how many units off-center the cycloidal gears are. This setting is useful for fine-tuning the cycloidal equation.

If the settings are correct, the line should be smooth with no visual sharp edges and especially no discontinuities.

Once you have the four input variables, use the following matlab code to procure the parametric equations required for SolidWorks input.

```
function cycloidal(C,R,N,E)
% Function to generate the outer cycloidal pattern with C= outer radius of
% static pins, R= radius of pins, N= number of pins (gear ratio), E
% eccentricity of shaft
    yfn = strcat('X=(%.2f*cos(t))-(%.2f*cos(t+arctan(sin((1-%.2f)*t)',...
        '/((%.2f/(%.2f*%.2f))-cos((1-%.2f)*t)))))-(%.2f*cos(%.2f*t)) \n');
    xfn = strcat('Y=(-%.2f*sin(t))+(%.2f*sin(t+arctan(sin((1-%.2f)*t)',...
        '/((%.2f/(%.2f*%.2f))-cos((1-%.2f)*t)))))+(%.2f*sin(%.2f*t)) \n');
    yeqn = fprintf(yfn,C,R,N,C,E,N,N,E,N);
    xeqn = fprintf(xfn,C,R,N,C,E,N,N,E,N);
```

In matlab, assign the variables C, R, N, and E to their corresponding values from Desmos. Then, run `cycloidal(C,R,N,E);`  in matlab.
This will print two equations, which you will use later in the equation driven curve in SolidWorks.

In SolidWorks, create a new sketch on the top plane. Insert an equation driven curve,

![Visual showing where to insert an equation driven curve](https://github.com/hps-ucsd-2020/hps-ucsd-2020.github.io/blob/ab586fb4d7ce3adb1b3fe5957fe1b686b88f9be4/assets/img/wiki-img/insert_eq_curve.png)

In the pop-up box, select parametric curve

![Equation driven curve pop-up](https://github.com/hps-ucsd-2020/hps-ucsd-2020.github.io/blob/1d744bead89d08bc74b5d667f373cb764cedb1d1/assets/img/wiki-img/eq_curve_parameters.png)

Make sure to leave Matlab open, as you will need to copy-paste each equation twice.
In the pop-up box for equation driven curve, input your x and y equations and select a t of 0 to pi.
Repeat this process selecting a t of 0 to pi. No, you can not select a t of pi to 2\*pi. No, I do not know why.

Once you have the drawing of the cycloidal gear, you want to create the part. The first step is to create holes in the part for the output shaft.
In the sketch, draw a centerline from the origin (which will be at the center of mass of the cycloid) outward in any direction,
ending where the center of your output hole will be. Then, click the arrow next to "Linear Sketch Pattern" and select "Circular Sketch Pattern."
The circle should then automatically loop around the origin, and anywhere between 4 and 6 output holes should be sufficient.
<!-- Input hole size here, figure out later -->
After this, create a single hole in the center of the cycloid, which will function as an input hole.
This will need to be the same size as the exterior diameter of the bearing used on the input shaft. With that, all holes in the gear are done.
It should be at least 5mm thick.

The next part to make is the gearbox housing, inside of which the cycloids will rotate.
This will first require the base, which needs to be large enough to house the pins as well as a wall or support structure for the gearbox.
In the example piece I'm creating, the base will need to be at least 35mm in radius, and should be about 5mm thick.
In the center of the base, draw a circle which will be the size of the exterior diameter of the bearing which you use to hold the driveshaft in place.
Extrude the base, which should be at least 5mm thick.

Next will be the pin circle. From the drawing of your base, create a centerline from the center of the circle outward.
The length of this line must be the radius of your pin circle, which is the C value from Desmos. On the end of this line, draw a circle of radius R, which is also from Desmos.
Similarly to the output holes, create a circular sketch pattern from this, surrounding the origin.
The number of occurances in this pattern should be equal to N, the variable from Desmos, which is one more than the number of teeth in the cycloidal gear.
Since the gearbox should have two gears to minimize vibration, make the pins twice the length of the gear thickness.
