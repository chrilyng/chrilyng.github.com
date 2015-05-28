---
layout: post
category : Web 
tagline: "Fun with javascript"
tags : [development]
---
{% include JB/setup %} 
I stumbled upon this Javascript online 2D graphics [editor](http://kvad.dk/). 
By using the [Tetris](http://kvad.dk/sketch/tetris) game as a source of inspiration I recreated [snake](http://kvad.dk/sketch/chrilyng/snake)

The following code is the most interesting excerpt.
Most of it is reworked using the framework of the Tetris game except the loop function which is almost the same.

Initialization code:

{% highlight javascript %}

var snake_block = {
    pos_x: 8,
    pos_y: 5}
    
var game_over = 1;
var moves = 0;
var move_speed = 10;
var apples = [];
var score = 0;
var move_dir = [1,0];
var speedup = 0;

var gr = color('green');
var bl = color('black');
var rd = color('red');

var wh = color(99,99,99);

// creation of game objects and their properties

function spawn()
{    
    snake_block.size = 4;
    snake_block.pos_x = 8;
    snake_block.pos_y = 5;
    snake_block.tail = [[snake_block.pos_x,snake_block.pos_y]]; 
    
    for(var x = 1; x < snake_block.size; x++)
    {
        snake_block.tail.push([snake_block.pos_x-x, snake_block.pos_y]);
    }
    
    apples = [];
    spawnApple(); 
}

function spawnApple() 
{
    apple = [random(3,13), random(3,13)];
    
    if(!tailCollision(apple[0], apple[1]))
        apples.push(apple);
    else
        spawnApple();
}

...

// drawing of dynamic objects
function draw(fill)
{
    function snakeDraw(c) {
        for(var x = 0; x < snake_block.tail.length; x++)
        {
            pixel(snake_block.tail[x][0], snake_block.tail[x][1], c);
        }
    }
    
    if(fill===0) {
        snakeDraw(bl);
    } else {
        snakeDraw(gr);        
        pixel(snake_block.pos_x, snake_block.pos_y, color(99,199,99));
    }
    for(var a = 0; a < apples.length; a++)
    {
        pixel(apples[a][0], apples[a][1], fill===0?bl:rd);
    }
}

// update of visuals
var time_to_move = 0;
function _move()
{
    time_to_move++;
    if(time_to_move >= move_speed || speedup) {
        moves++;
        
        if(moves % 7 === 0 && apples.length < 3)
            spawnApple();
        
        time_to_move = 0;
        speedup = 0;
            
        if(collision(snake_block.pos_x + move_dir[0], snake_block.pos_y + move_dir[1]))
        {
            game_over=1;
        }
        else 
        {            
            snake_block.pos_x += move_dir[0];
            snake_block.pos_y += move_dir[1]; 
            snake_block.tail.unshift([snake_block.pos_x, snake_block.pos_y]);
            i = appleCollision(snake_block.pos_x, snake_block.pos_y);
        
            if(i>-1)
            {
                apples.splice(i,1); 
                score += 1;
                snake_block.size += 1;               
                if(move_speed>5)
                    move_speed -= 1;
            }  else {            
                snake_block.tail.pop();
            }  
        }
    } 
}
{% endhighlight %}
