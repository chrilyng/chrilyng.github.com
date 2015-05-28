---
layout: post
category : Web 
tagline: "Fun with javascript"
tags : [development]
---
{% include JB/setup %} 
I stumbled upon this Javascript online 2D graphics [editor](http://kvad.dk/). 
By using the [Tetris](http://kvad.dk/sketch/tetris) game as a source of inspiration I recreated [snake](http://kvad.dk/sketch/chrilyng/snake)

The following code is the most interesting part where the visuals are updated based on the user input. 
The apples and snake tail are both two dimensional arrays that I believe was the easiest way to store their coordinates.

{% highlight javascript %}
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
        if(collision(snake_block.pos_x + move_dir.dir[0], snake_block.pos_y + move_dir.dir[1]))
        {
            game_over=1;
        }
        else 
        {            
            moves++;
        
            if(moves % 7 === 0 && apples.length < 3)
                 spawnApple();
        
            time_to_move = 0;
            speedup = 0;
        
            snake_block.pos_x += move_dir.dir[0];
            snake_block.pos_y += move_dir.dir[1]; 
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
