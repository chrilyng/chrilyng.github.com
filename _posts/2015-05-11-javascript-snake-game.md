---
layout: post
category : Web 
tagline: "Fun with javascript"
tags : [development]
---
{% include JB/setup %} 
I stumbled upon this Javascript online 2D graphics [editor](http://kvad.dk/). 
By using the [Tetris](http://kvad.dk/sketch/tetris) game as a source of inspiration I recreated [snake](http://kvad.dk/sketch/chrilyng/snake)

I have posted the code here split into the main sections.
Most of it is reworked using the framework of the Tetris game except the loop function which is almost the same.

Initialization code:

{% highlight javascript %}

snake_block = {
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
function restart()
{
    game_over = 0;
    move_speed = 10;
    move_dir = [1,0];
    moves = 0;    
    score = 0;
    background('black');
    

    for(var x = 2; x < 14; x++) {
        pixel(x,1,wh);
        pixel(x,14,wh);
        pixel(1,x,wh);
        pixel(14,x,wh);
    }
        
    spawn();
}

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

// control
function keydown()
{
    if(key_id=='Up' && move_dir[1] != -1) {
        if(move_dir[1] == 1)
            speedup = 1;
        move_dir = [0,1];            
    }      
    if(key_id=='Down' && move_dir[1] != 1) {
        if(move_dir[1] == -1)
            speedup = 1;
        move_dir = [0,-1];
    }
    if(key_id=='Left' && move_dir[0] != 1) {
        if(move_dir[0] == -1)
            speedup = 1;
        move_dir = [-1,0];
    }
    if(key_id=='Right' && move_dir[0] != -1) {
        if(move_dir[0] == 1)
            speedup = 1;
        move_dir = [1,0];
    }
}

// collision logic
function collision(x,y)
{    
    if(x<2||y<2||x>13||y>13)
    {
        return true;
    }
    return tailCollision(x, y);    
}


function tailCollision(x, y) 
{    
    for(var i = 0; i < snake_block.tail.length; i++)
    {
        if(x == snake_block.tail[i][0] && 
            y == snake_block.tail[i][1]) 
        {
            return true;
        }
    }
    return false;
}

function appleCollision(x, y) 
{    
    for(var i = 0; i < apples.length; i++)
    {
        if(x == apples[i][0] && 
            y == apples[i][1]) 
        {
            return i;
        }
    }
    return -1;
}

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

function loop()
{
    write("score: "+score,0,15);
    if(game_over == 1) {
        for(var x = 0; x < 16; x++) {
            for(var y=4; y < 11; y++) {
                pixel(x,y, rd);            
            }
        }
        write("GAME  OVER", 3,9);
        write("KLIK FOR START", 1, 7);        
        write("STYR MED PILE", 1, 5);
        if(mouse_pressed===true)
            restart();
        return;
    }
    draw(0);
    _move();
    draw(1);
}

restart();
{% endhighlight %}
