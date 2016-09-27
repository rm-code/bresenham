# Bresenham
Bresenham's line algorithm written in Lua.

## Usage
```Lua
local Bresenham = require( 'Bresenham' )

local grid = {
    { '1', '1', '1', '1', '1', '1', '1', '1' },
    { '1', '0', '0', '0', '0', '0', '0', '0' },
    { '1', '0', '0', '0', '2', '0', '0', '0' },
    { '1', '0', '0', '0', '2', '0', '0', '0' },
    { '1', '0', '0', '2', '2', '0', '0', '0' },
    { '1', '0', '0', '2', '0', '0', '0', '0' },
    { '1', '0', '0', '0', '0', '0', '0', '0' },
    { '1', '1', '1', '1', '1', '1', '1', '1' }
}

print( 'Traverse grid.' )
Bresenham.calculateLine( 1, 1, 7, 7, function( x, y, counter )
    print( x, y, counter, grid[x][y] )
    return true
end)

print( 'Stop line early.' )
Bresenham.calculateLine( 1, 1, 7, 7, function( x, y, counter )
    print( x, y, counter, grid[x][y] )
    if grid[x][y] == '2' then
        return false;
    end
    return true;
end)
```
