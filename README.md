# Bresenham
Bresenham's line algorithm written in Lua.

## Overview

#### Arguments
The `Bresenham.line` function expects the following arguments:
- __ox (number)__ The origin's x-coordinates. The line will start here.
- __oy (number)__ The origin's y-coordinates. The line will start here.
- __tx (number)__ The target's x-coordinates. The line will end here.
- __ty (number)__ The target's y-coordinates. The line will end here.
- __callback (function)__ A callback function being called for every tile the line passes. The line algorithm will stop if the callback returns false __(optional)__.
- __... (varargs)__ Additional variables that should be passed to the callback function __(optional)__.

#### Return values
- __(boolean)__ True if the line has reached its target, false if it stopped early.
- __(number)__ The number of tiles traversed by the line.

## Usage

If you don't provide a callback function the line algorithm will always return `true` and can be used to count distances on the grid:

```Lua
local _, counter = Bresenham.line( 1, 1, 7, 7 )
```


By providing a callback you can control how the line algorithm behaves on the grid. For example you could tell it to stop early if it hits certain tiles, objects, monsters and so on ...

```lua
local function callback( x, y, counter, ... )
    -- Unpack the varargs passed after the callback.
    local vararg1, vararg2 = ...;

    -- Check if the line algorithm should stop early.
    if not grid[x][y]:isPassable() then
        return false
    end
    return true
end

Bresenham.line( 1, 1, 10, 10, callback, 'foo', 'bar' )
```

Complete example:
```Lua
local Bresenham = require( 'Bresenham' )

local grid = {
    { '#', '#', '#', '#', '#', '#', '#', '#' },
    { '#', '.', '.', '.', '.', '.', '.', '#' },
    { '#', '.', '.', '.', '.', '.', '.', '#' },
    { '#', '.', '.', '#', '#', '.', '.', '#' },
    { '#', '.', '.', '#', '#', '.', '.', '#' },
    { '#', '.', '.', '.', '.', '.', '.', '#' },
    { '#', '.', '.', '.', '.', '.', '.', '#' },
    { '#', '#', '#', '#', '#', '#', '#', '#' }
}

print( 'Traverse grid if no obstacles are hit:' )
local success, counter = Bresenham.line( 2, 2, 6, 2, function( x, y, counter )
    print( string.format( 'x: %d, y: %d, steps: %d, tile: %s', x, y, counter, grid[x][y] ))
    if grid[x][y] == '#' then
        return false
    end
    return true
end)
print( string.format( 'Reached target: %s after %d steps.\n', tostring( success ), counter ))

print( 'Stop line early if obstacles are hit:' )
local success, counter = Bresenham.line( 2, 2, 6, 6, function( x, y, counter )
    print( string.format( 'x: %d, y: %d, steps: %d, tile: %s', x, y, counter, grid[x][y] ))
    if grid[x][y] == '#' then
        return false
    end
    return true
end)
print( string.format( 'Reached target: %s after %d steps.\n', tostring( success ), counter ))

print( 'Without a callback just count the steps from start to finish:' )
local _, counter = Bresenham.line( 1, 1, 7, 7 )
print( counter )
```
