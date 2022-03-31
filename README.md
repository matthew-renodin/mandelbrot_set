# mandelbrot set introduction

This project is a few years old but is certainly still interesting too me. I wrote it orginally in 2016 and just updated it. I based the functions on Borland Graphics. When I started programming I worked on a project that used that library for creating a UI. I actually ported it to Linux and integrated with the X11 graphics library to build native Linux application orginally wrtten for DOS and Windows apps. 

This implementation uses JavaScript and an HTML canvas which is much faster than SVG.  My original was writtn using an SVG, but this one is much faster using the HTML canvas.

Check out the embedded JavaScript in the HTML file. 

![image](https://user-images.githubusercontent.com/5507643/150658841-1deba9cc-b3a4-4ad5-8bb1-d1d3f29a025c.png)

```JavaScript
           var colors = [
                '(255, 255, 255)', 
                '(255, 0, 0)', 
                '(0, 255, 0)',
                '(0, 0, 255)',
                '(255, 255, 0)',
                '(0, 255, 255)', 
                '(255, 0, 255)',
                '(55, 180, 255)'],
            model = {
                pallette: []
            };

    canvas = document.getElementById('canvas');     
 
    
    function drawPixel(context, x, y, color) {
        context.beginPath();
        context.fillStyle = color;
  	context.fillRect(x, y, 1, 1);
        context.fill();
    };

    var render = function() {
        var ctx = canvas.getContext('2d');
        var i, j;
        for(i = 1; i < canvas.width; i++)
        {
            for(j = 1; j < canvas.height; j++)
            {
                drawPixel(ctx,i,j,model.pallette[i][j]);
            }
        }
    };
    
    var store = function (x, y, color){
        model.pallette[x][y] = 'rgb'+color;
    };
    
    var degrees_to_radians = function(degrees){
        var angle;
        while(degrees >=360)
            degrees = degrees-360;
        while(degrees < 0)
            degrees = degrees+360;
        angle = degrees * .0174533;
        return angle;
    };
    
    var plot = function(ctx, x, y, color){
        drawPixel(ctx,x,y,color);
    };
    var createCanvas = function(){
        var ctx = canvas.getContext('2d');
        var i, j;
        for(i = 1; i < canvas.width; i++)
        {
            for(j = 1; j < canvas.height; j++)
            {
                drawPixel(ctx,i,j,'#cccccc');
            }
        }
    };
    
    var init = function()
    {
        model.pallette[canvas.width] = undefined;
        var i = 0;
        for(i = 0; i < canvas.width; i++){
            model.pallette[i] = [];
            model.pallette[i][canvas.height] = undefined;
        }
    };
    
    var mandelbrot = function()
    {
        var maxcol = canvas.width,
            maxrow = canvas.height,
            maxColors = 8,
            maxSize = 4,
            maxIterations = 19,
            Q = [],
            Pmax = 1.75,
            Pmin = -1.75,
            Qmax = 1.5,
            Qmin = -1.5,
            P = 0,
            deltaP = (Pmax-Pmin)/(maxcol -1),
            deltaQ = (Qmax-Qmin)/(maxrow -1),
            col = 0, 
            row = 0,
            X = 0.0,
            Y = 0.0,
            color,
            tcolor,
            Xsquare,
            Ysquare;
            Q[350] = undefined;
            
            init();
            for(row = 1; row<= maxrow; row++){
                 Q[row] = Qmin + (row*deltaQ);
            }
            
            for(col = 1; col < maxcol; col++)
            {
                P = Pmin + (col* deltaP);
                for(row = 1; row< maxrow; row++)
                {
                    Y = 0.0;
                    X = Y;
                    color = 0;
                    for(color = 0; color< maxIterations;color++)
                    {
                        Xsquare = X*X;
                        Ysquare = Y*Y;
                        if((Xsquare + Ysquare) > maxSize)
                            break;
                        Y = 2*X*Y + Q[row];
                        X = Xsquare - Ysquare +P;
                    }
                    
                    tcolor = color%maxColors;
                    store(col, row, colors[tcolor]);
                }
            }
            render();
     };
     
```

