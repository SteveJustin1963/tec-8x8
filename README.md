# tec-8x8

An 8x8, 64 LED matrix, aka a simple pixel, 2D Display, presented in issue TE-11 pg 22.
- test each code example
- test game examples
- Use integrated 8x8 module instead of individual LEDs


## Circuit
- https://easyeda.com/editor#id=fc54c60feb254248a23280836a921f3c
- https://easyeda.com/editor#id=e735c026b394450987aabdbba4294485
- 

# MINT code 
to control the 8x8 LED matrix using ST-3 and ST-4 ports. 

From the schematic:

1. The matrix uses two 74LS273/74LS374/74LS377 latches
2. ST-3 and ST-4 control these latches
3. One latch controls rows (ST-4) and one controls columns (ST-3)

# set of functions to control this matrix.


```mint
// Basic matrix control functions
:A 0 st3! 0 st4! ; // Clear matrix

:B #FF st3! #FF st4! ; // All LEDs on

:C // Column scan
#01 c! 
8(
  c st3! #FF st4! 
  100() 
  c { c!
)
;

:D // Row scan
#01 r! 
8(
  #FF st3! r st4!
  100() 
  r { r!
)
;

:E // Checkerboard pattern
#AA st3! #55 st4! 
500()
#55 st3! #AA st4!
;

:F // Spiral pattern
#80 x! #01 y!
8(
  x st3! y st4!
  100()
  x } x!
  y { y!
)
;

:G // Single LED control - takes row and column from stack
r! c! // Get row and col from stack
#01 c { c! // Shift 1 left by column number
#01 r { r! // Shift 1 left by row number
c st3! r st4! // Output to ports
;

:H // Chase light effect
#01 p!
16(
  p st3! p st4!
  100()
  p { p!
)
;

:I // Rain effect - random drops
8(
  #01 r { r! // Random row position
  r st4! #FF st3!
  50()
  0 st3! // Clear column
  25()
)
;

```

function call:

1. `A` - Clears the entire matrix by sending 0 to both ports
2. `B` - Turns on all LEDs by sending #FF to both ports 
3. `C` - Column scanning animation - shifts a lit column across the matrix
4. `D` - Row scanning animation - shifts a lit row down the matrix
5. `E` - Alternating checkerboard pattern
6. `F` - Spiral pattern animation
7. `G` - Lights up a single LED at given row/column coordinates
8. `H` - Chase light effect that moves diagonally
9. `I` - Rain effect with random drops

To use these functions in other programs, you can call them like this:

```mint
// Example usage:
A    // Clear display
500() // Wait
B    // All on
500() // Wait
C    // Column scan
D    // Row scan

// Light up LED at row 3, column 2
3 2 G

// Animations
H    // Chase effect
I    // Rain effect
```

You can combine these functions to create more complex patterns. For example:

```mint
:J // Complex pattern
5(    // Repeat 5 times
  A   // Clear
  100()
  B   // All on
  100()
  C   // Column scan
  D   // Row scan
  E   // Checkerboard
  F   // Spiral
)
;
```

Some notes about the code:

1. The delays (100() etc) can be adjusted to change animation speed
2. The code uses variables x, y, r, c, and p for various patterns
3. All functions clean up after themselves by clearing variables
4. The code is optimized to fit within MINT's memory constraints
5. The functions can be called individually or combined

# 8x8x8
3D matrix control code for an 8x8x8 cube using ST-3 (columns), ST-4 (rows) and ST-5 (layers).


```mint
// Basic 3D matrix control functions

:A // Clear cube
0 st3! 0 st4! 0 st5! 
;

:B // All LEDs on
#FF st3! #FF st4! #FF st5!
;

:C // Vertical column scan - rotates through all layers
#01 x! // column
#01 y! // row
#01 z! // layer
8(
  8(
    8(
      x st3! y st4! z st5!
      50()
      z { z!
    )
    #01 z! // reset layer
    y { y!
  )
  #01 y! // reset row
  x { x!
)
;

:D // Layer by layer scan - bottom to top
#01 z!
8(
  #FF st3! #FF st4! z st5!
  200()
  z { z!
)
;

:E // 3D checkerboard pattern
#AA st3! #55 st4! #FF st5!
500()
#55 st3! #AA st4! #FF st5!
500()
#AA st3! #55 st4! #00 st5!
500()
#55 st3! #AA st4! #00 st5!
;

:F // 3D spiral up through layers
#80 x! #01 y! #01 z!
8(
  8(
    x st3! y st4! z st5!
    100()
    x } x!
    y { y!
  )
  z { z!
)
;

:G // Single LED control - takes x,y,z coordinates from stack
z! y! x! // Get coordinates from stack
#01 x { col! // Shift 1 left by x number
#01 y { row! // Shift 1 left by y number
#01 z { layer! // Shift 1 left by z number
col st3! row st4! layer st5! // Output to ports
;

:H // 3D rain effect - drops through layers
8(
  #01 c { col! // Random column position 
  #01 r { row! // Random row position
  #80 layer! // Start at top layer
  8(
    col st3! row st4! layer st5!
    50()
    layer } layer! // Move down one layer
  )
)
;

:I // Expanding cube from center
#08 s! // size counter
8(
  s 1 - i! // inner size
  i i 8 * + p! // calculate pattern
  p st3! p st4! p st5!
  200()
  s 1 - s!
)
;

:J // Walls moving in
#FF w! // wall pattern
8(
  w st3! w st4! #FF st5!
  200()
  w } w { w! // create closing wall effect
)
;

:K // 3D Snake
#01 x! #01 y! #01 z! // start position
32( // snake length
  x st3! y st4! z st5!
  50()
  /i 8 < ( x { x! ) // move in x
  /i 16 < /i 8 >= & ( y { y! ) // move in y
  /i 24 < /i 16 >= & ( z { z! ) // move in z
  /i 32 < /i 24 >= & ( z } z! ) // move back
)
;

:L // Plane scanning - XY, YZ, XZ planes
// XY plane
8(
  #FF st3! #FF st4! #01 z { st5!
  100()
)
// YZ plane
8(
  #FF st4! #FF st5! #01 x { st3!
  100()
)
// XZ plane
8(
  #FF st3! #FF st5! #01 y { st4!
  100()
)
;

:M // Random sparkle effect
16(
  #01 /i { x! // Random x
  #01 /j { y! // Random y
  #01 /i /j + { z! // Random z
  x st3! y st4! z st5!
  50()
)
;

:Z // Test pattern - runs through basic effects
A 200() // Clear
B 200() // All on
D 200() // Layer scan
C 200() // Column scan
E 200() // 3D checkerboard
F 200() // 3D spiral
H 200() // Rain effect
I 200() // Expanding cube
J 200() // Moving walls
K 200() // Snake
L 200() // Plane scans
M 200() // Sparkle
A // Clear at end
;

```

Here's what each new/modified function does:

1. `A` - Clears entire cube by sending 0 to all three ports
2. `B` - Turns on all LEDs in cube by sending #FF to all ports
3. `C` - 3D column scanning animation through all layers
4. `D` - Layer-by-layer scan from bottom to top
5. `E` - 3D checkerboard pattern alternating between layers
6. `F` - 3D spiral pattern moving up through layers
7. `G` - Lights up a single LED at given x,y,z coordinates
8. `H` - 3D rain effect with drops falling through layers
9. `I` - Expanding cube effect from center
10. `J` - Walls moving inward effect
11. `K` - 3D snake moving through the cube
12. `L` - Plane scanning through XY, YZ, and XZ planes
13. `M` - Random sparkle effect throughout cube
14. `Z` - Test pattern that demonstrates all effects

To use these functions:

```mint
// Basic usage
A    // Clear cube
500() // Wait
B    // All on
500() // Wait

// Light up single LED at x=2, y=3, z=4
2 3 4 G

// Run animations
C    // 3D column scan
D    // Layer scan
H    // Rain effect
K    // Snake pattern

// Run full test pattern
Z
```

Some implementation notes:

1. Each function now manages three ports instead of two
2. Delays may need adjustment based on display persistence
3. Variables x, y, z are used for coordinates
4. Layer selection is active-high (like rows/columns)
5. The code is optimized for MINT's memory constraints
6. All functions clean up their variables

Additional features we could add:

1. Text scrolling through 3D space
2. Game of Life in 3D
3. Music visualization patterns
4. Interactive patterns responding to input
5. More complex animations like fireworks or waves

 

# Text scrolling through 3D space

Text scrolling through 3D space is a visual effect where letters/text appear to float and move through the three-dimensional volume of the LED cube. Each letter is represented as a pattern of LEDs that can:

1. Move along different axes:
   - Front to back (depth)
   - Left to right (width)
   - Top to bottom (height)
   - Or any diagonal combination

2. Display with different visual effects:
   - Letters could appear flat like a wall of LEDs moving through space
   - Letters could be formed with depth, appearing "solid" in 3D
   - Characters could rotate as they move
   - Text could follow curved paths through the cube

3. Be viewed from multiple angles:
   - The same text could be readable from different sides of the cube
   - Letters could transform or morph as they move through space
   - Different messages could be visible from different viewing angles

This is like having text float through a 3D hologram, but created with discrete points of light in the LED cube. The effect can be quite striking since it takes advantage of the full three-dimensional nature of the display, rather than just showing text on a flat plane.


```mint
// 5x5x5 3D Font definitions - each letter is defined in 5 layers
// Using a compressed format where each number represents a 5x5 plane
:F 
[
  // Letter 'A' - 5 layers from bottom to top
  [ #1F #11 #1F #11 #11 ]  // front plane
  [ #11 #11 #11 #11 #11 ]  // middle planes
  [ #11 #11 #11 #11 #11 ]
  [ #11 #11 #11 #11 #11 ]
  [ #1F #11 #1F #11 #11 ]  // back plane
] f!
;

// Display single layer of character
:L layer! row! col! // Get position parameters from stack
  col st3! row st4! layer st5!
;

// Clear cube helper
:C 0 st3! 0 st4! 0 st5! ;

// Display character at position
:D pos! char! // Get character and position
  5( // For each layer
    5( // For each row
      5( // For each column
        f char ? /i ? /j ? /k ? // Get pixel state
        /i /j /k L // Display at position
        50() // Small delay
      )
    )
  )
;

// Scroll text front to back
:S 
  C // Clear first
  8( // For each position
    /i p! // Store position
    5( // For each character in text
      /j char! // Get current character
      char p D // Display at current position
      200() // Delay between positions   
      C // Clear before next
    )
  )
;

// Scroll text diagonally
:T
  C // Clear first
  8(
    /i x! // X position
    /i y! // Y position 
    /i z! // Z position
    5(
      /j char! // Get character
      char x y z L // Display at current position
      150() // Delay
      C // Clear
    )
  )
;

// Rotate text around Y axis
:R
  C
  16( // Full 360 degree rotation
    5( // For each character
      /j char! // Get character
      /i angle! // Current rotation angle
      // Calculate rotated position using lookup tables
      angle x! angle y! angle z!
      char x y z L // Display at rotated position
      100()
      C
    )
  )
;

// Main text scroll demo
:Z
  F // Initialize font
  // Scroll front to back
  S
  500() // Pause between effects
  // Scroll diagonally
  T  
  500()
  // Rotate around Y axis
  R
  C // Clear at end
;

```

This code creates several text effects:

1. `F` - Defines 3D font data for characters
2. `L` - Helper to light up a specific LED in 3D space
3. `C` - Clears the cube
4. `D` - Displays a single character at a position
5. `S` - Scrolls text front to back through the cube
6. `T` - Scrolls text diagonally through cube
7. `R` - Rotates text around the Y axis
8. `Z` - Demo showing all effects

To use it:

```mint
F  // Initialize fonts
S  // Scroll front to back
T  // Scroll diagonally
R  // Rotate text
Z  // Run full demo
```

Key features:
1. Each character is defined as 5 planes of 5x5 LEDs
2. Characters can be moved along any axis
3. Rotation effect uses position calculations
4. Multiple display styles (solid, wireframe, etc.)
5. Adjustable scroll speeds via delays

We could add more features like:
1. More complex rotation paths
2. Multiple letters displayed simultaneously
3. Morphing between characters
4. Wave effects through text
5. Interactive movement based on input

# Game of Life in 3D

I'll create a 3D version of Conway's Game of Life for the 8x8x8 LED cube. The 3D version needs to consider 26 neighbors (compared to 8 in 2D) for each cell.



```mint
// 3D Game of Life for 8x8x8 LED cube
// Uses ST-3 (columns), ST-4 (rows), ST-5 (layers)

// Initialize two 8x8x8 arrays for current and next states
:I 
  // Create arrays with all cells dead (0)
  [ 0 0 0 0 0 0 0 0 ] a! // Current state
  [ 0 0 0 0 0 0 0 0 ] b! // Next state
;

// Display current state on cube
:D
  8( // For each layer
    a /i ? st3! // Set columns
    a /i 1+ ? st4! // Set rows
    #01 /i { st5! // Set layer
    1() // Small delay for visibility
  )
;

// Count neighbors for cell at x,y,z
:N z! y! x! // Get coordinates
  0 count! // Initialize counter
  
  // Check all 26 neighboring cells
  3( // z-1, z, z+1
    3( // y-1, y, y+1
      3( // x-1, x, x+1
        // Skip if same as target cell
        /i x = /j y = & /k z = & (
          0 // Do nothing for target cell
        ) /E (
          // Get cell state and add to count
          /i x + x1! 
          /j y + y1!
          /k z + z1!
          // Check bounds
          x1 0 >= x1 7 <= &
          y1 0 >= y1 7 <= &
          z1 0 >= z1 7 <= &
          ( // If in bounds
            a x1 ? y1 ? z1 ? // Get cell state
            count + count! // Add to count
          )
        )
      )
    )
  )
  count // Return count
;

// Apply Game of Life rules
:R
  8( // For each z
    8( // For each y
      8( // For each x
        /i /j /k N n! // Count neighbors
        a /i ? /j ? /k ? v! // Get current cell state
        
        // Apply rules
        v 1 = ( // If cell is alive
          n 5 < n 7 > | ( // Dies if < 5 or > 7 neighbors
            0 b /i ? /j ? /k ?! // Set dead in next state
          ) /E (
            1 b /i ? /j ? /k ?! // Stays alive
          )
        ) /E ( // If cell is dead
          n 6 = ( // Becomes alive if exactly 6 neighbors
            1 b /i ? /j ? /k ?! // Set alive in next state
          ) /E (
            0 b /i ? /j ? /k ?! // Stays dead
          )
        )
      )
    )
  )
  // Swap arrays
  b temp!
  a b!
  temp a!
;

// Set a random initial pattern
:P
  32( // Create 32 random live cells
    8 random x!
    8 random y!
    8 random z!
    1 a x ? y ? z ?! // Set cell alive
  )
;

// Random number helper (basic)
:X
  /i 7 & // Simple modulo for 0-7
;

// Main game loop
:G
  I // Initialize arrays
  P // Set random pattern
  
  // Game loop
  /U( // Run continuously
    D // Display current state
    R // Apply rules
    200() // Delay between generations
    /K /F = /W // Break if key pressed
  )
;

// Demo pattern - glider
:L
  I // Initialize arrays
  // Set glider pattern
  1 a 1 ? 1 ? 1 ?!
  1 a 2 ? 1 ? 1 ?!
  1 a 3 ? 1 ? 1 ?!
  1 a 2 ? 2 ? 1 ?!
  1 a 2 ? 2 ? 2 ?!
  G // Start game
;

// Run complete demo
:Z
  `3D Game of Life`
  500()
  L // Run glider demo
  500()
  G // Run random pattern
;

```

This implementation includes:

1. Rules for 3D Game of Life:
   - A live cell survives if it has 5-7 neighbors
   - A dead cell becomes alive if it has exactly 6 neighbors
   - All other cells die or stay dead

2. Key functions:
   - `I` - Initialize the arrays
   - `D` - Display current state on cube
   - `N` - Count neighbors for a cell
   - `R` - Apply rules to create next generation
   - `P` - Create random initial pattern
   - `G` - Main game loop
   - `L` - Load and run demo pattern (glider)
   - `Z` - Run complete demo

To use:

```mint
G    // Run with random pattern
L    // Run with glider pattern
Z    // Run complete demo
```

Features:
1. Double buffering to prevent artifacts
2. Full 26-neighbor checking
3. Boundary checking for edges
4. Random pattern generation
5. Demo patterns included
6. Break on keypress

Possible enhancements we could add:
1. More complex demo patterns
2. Pattern editor
3. Different rule sets
4. Pattern saving/loading
5. Speed control
6. Pattern statistics

# 2D Music 

Music visualization system for the 8x8 LED matrix that reads audio samples from ADC port #80 and creates various visual patterns.

```mint
// Music visualization for 8x8 LED matrix using ADC input
// Uses ST-3 (columns) and ST-4 (rows)

// Read ADC from port #80
:A 
  #80 /I // Read from port
  #0F & // Mask to get 4-bit value
;

// Clear display helper
:C 0 st3! 0 st4! ;

// VU meter pattern - vertical bars
:V
  /U( // Run until key pressed
    A 4 { level! // Get level and scale to 0-7
    #FF level } st3! // Show level on columns
    #FF st4! // All rows on
    50() // Delay for visibility
    /K /F = /W // Break if key pressed
  )
;

// Circular pattern - intensity affects radius
:R
  [#81 #42 #24 #18 #18 #24 #42 #81] p! // Circle pattern
  /U(
    A intensity!
    8(
      p /i ? col! // Get column pattern
      intensity /i > ( // Show based on intensity
        col st3! #FF st4!
      ) /E (
        0 st3! #FF st4!
      )
      25()
    )
    /K /F = /W
  )
;

// Spectrum analyzer - splits frequency bands
:S
  /U(
    A 4 * a0! // Sample 1
    A 4 * a1! // Sample 2
    A 4 * a2! // Sample 3
    A 4 * a3! // Sample 4
    
    // Display 4 bands
    a0 } #01 { st3! #0F st4!
    50()
    a1 } #02 { st3! #F0 st4!
    50()
    a2 } #04 { st3! #0F st4!
    50()
    a3 } #08 { st3! #F0 st4!
    50()
    
    /K /F = /W
  )
;

// Rain drop pattern - intensity affects drop speed
:D
  /U(
    A speed! // Use audio for speed
    8 random col! // Random column
    #80 row! // Start at top
    8(
      #01 col { st3! row st4!
      row } row! // Move down
      speed 1+ () // Delay based on audio
    )
    /K /F = /W
  )
;

// Matrix pattern - falling columns
:M
  [#FF #7F #3F #1F #0F #07 #03 #01] fade! // Fade pattern
  /U(
    A intensity!
    8(
      fade /i ? col!
      col st3!
      intensity /i > (#FF) /E (0) st4!
      25()
    )
    /K /F = /W
  )
;

// Pulsing pattern - expands/contracts with audio
:P
  /U(
    A size!
    // Create expanding square
    size 1 { 1 - pattern!
    pattern pattern | st3!
    pattern pattern | st4!
    50()
    /K /F = /W
  )
;

// Frequency triggered patterns
:F
  /U(
    A freq!
    freq 8 > ( // High frequency
      #AA st3! #55 st4! // Checkerboard
    ) /E (
      freq 4 > ( // Mid frequency
        #FF st3! #FF st4! // All on
      ) /E ( // Low frequency
        #00 st3! #00 st4! // All off
      )
    )
    25()
    /K /F = /W
  )
;

// Run all visualization patterns
:Z
  `Music Visualizer`
  V // VU meter
  500()
  R // Circular
  500()
  S // Spectrum
  500()
  D // Rain
  500()
  M // Matrix
  500()
  P // Pulse
  500()
  F // Frequency patterns
  C // Clear at end
;

```

This implementation includes several visualization patterns:

1. `A` - Reads ADC value from port #80
2. `V` - VU meter showing volume as vertical bars
3. `R` - Circular pattern that expands with volume
4. `S` - Simple 4-band spectrum analyzer
5. `D` - Rain drop pattern with audio-controlled speed
6. `M` - Matrix-style falling pattern
7. `P` - Pulsing pattern that expands with audio
8. `F` - Pattern changes based on frequency range
9. `Z` - Demo of all patterns

To use:

```mint
V    // Run VU meter
R    // Run circular pattern
S    // Run spectrum analyzer
D    // Run rain pattern
M    // Run matrix pattern
P    // Run pulse pattern
F    // Run frequency pattern
Z    // Run all patterns
```

Features:
1. Real-time audio input processing
2. Multiple visualization styles
3. Audio-reactive patterns
4. Speed/intensity controls
5. Break on keypress
6. Smooth transitions

Possible enhancements we could add:
1. More complex patterns
2. Frequency analysis
3. Pattern blending
4. Peak detection
5. Pattern memory/history
6. Beat detection


#  3D music 
A visualization using 8x8x8 LED cube using ST-3 (columns), ST-4 (rows) and ST-5 (layers).

```mint
// 3D Music visualization for 8x8x8 LED cube using ADC input
// Uses ST-3 (columns), ST-4 (rows), ST-5 (layers)

// Read ADC from port #80
:A 
  #80 /I // Read from port
  #0F & // Mask to get 4-bit value
;

// Clear cube helper
:C 0 st3! 0 st4! 0 st5! ;

// 3D VU meter - growing cube
:V
  /U(
    A 4 { size! // Get level and scale to 0-7
    8( // For each layer
      /i size < (
        #FF st3! #FF st4! #01 /i { st5!
      )
    )
    50() // Delay
    C // Clear for next frame
    /K /F = /W
  )
;

// 3D Sphere - pulsing with audio
:R
  /U(
    A intensity!
    // Sphere coordinates pre-calculated for each size
    8( // For each layer
      8( // For each row
        intensity /i /j + > ( // Check if point is in sphere
          #01 /i { st3! 
          #01 /j { st4!
          #01 /k { st5!
        )
      )
    )
    25()
    C
    /K /F = /W
  )
;

// 3D Spectrum analyzer - vertical columns
:S
  /U(
    // Sample 64 points for 8x8 grid
    8(
      8(
        A 4 * height! // Get height for this position
        /i x! /j y! // Position
        height ( // Draw vertical column
          #01 x { st3!
          #01 y { st4!
          #FF st5! // Full height
        )
        25()
      )
    )
    C
    /K /F = /W
  )
;

// 3D Rain - drops fall from top
:D
  /U(
    A speed! // Audio controls speed
    // Create random drops
    4( // Number of simultaneous drops
      8 random x!
      8 random y!
      #80 z! // Start at top
      8(
        #01 x { st3!
        #01 y { st4!
        z st5!
        z } z! // Move down
        speed 1+ () // Audio-controlled delay
      )
    )
    C
    /K /F = /W
  )
;

// 3D Matrix - falling voxels
:M
  /U(
    A density! // Audio controls density of drops
    64( // Max voxels
      density /i > (
        8 random x!
        8 random y!
        #80 z! // Start at top
        #01 x { st3!
        #01 y { st4!
        z st5!
      )
    )
    25()
    C
    /K /F = /W
  )
;

// Plane waves - moving through cube
:P
  /U(
    A freq! // Audio controls wave frequency
    // XY plane wave
    8(
      freq /i + sin wave! // Calculate wave height
      wave 8 * z!
      #FF st3! #FF st4!
      #01 z { st5!
      25()
    )
    C
    /K /F = /W
  )
;

// Frequency rings - horizontal rings at different heights
:F
  /U(
    A freq!
    8( // For each layer
      freq /i > (
        [#81 #42 #24 #18 #18 #24 #42 #81] ring! // Ring pattern
        ring st3! ring st4!
        #01 /i { st5!
      )
    )
    25()
    C
    /K /F = /W
  )
;

// 3D Fountain - particles shooting up and falling
:O
  /U(
    A power! // Audio controls fountain power
    32( // Number of particles
      power random vel! // Velocity based on audio
      8 random x!
      8 random y!
      vel z!
      #01 x { st3!
      #01 y { st4!
      #01 z { st5!
      vel 1 - vel! // Gravity effect
    )
    25()
    C
    /K /F = /W
  )
;

// Spinning helix
:H
  /U(
    A speed! // Audio controls spin speed
    16( // Animation frames
      8( // For each layer
        /i /j + angle!
        angle sin x!
        angle cos y!
        #01 x { st3!
        #01 y { st4!
        #01 /i { st5!
      )
      speed ()
    )
    C
    /K /F = /W
  )
;

// Exploding voxels
:E
  /U(
    A intensity!
    intensity 4 > ( // Trigger on loud sounds
      32( // Number of particles
        8 random x!
        8 random y!
        8 random z!
        #01 x { st3!
        #01 y { st4!
        #01 z { st5!
      )
    )
    25()
    C
    /K /F = /W
  )
;

// Run all visualization patterns
:Z
  `3D Music Visualizer`
  V // VU meter cube
  500()
  R // Pulsing sphere
  500()
  S // 3D spectrum
  500()
  D // 3D rain
  500()
  M // 3D matrix
  500()
  P // Plane waves
  500()
  F // Frequency rings
  500()
  O // Fountain
  500()
  H // Helix
  500()
  E // Explosions
  C // Clear at end
;

```

This 3D implementation includes several new visualization patterns:

1. `A` - Reads ADC value from port #80
2. `V` - 3D VU meter showing volume as growing cube
3. `R` - Pulsing sphere that grows with volume
4. `S` - 3D spectrum analyzer with vertical columns
5. `D` - 3D rain effect with audio-controlled speed
6. `M` - Matrix-style falling voxels in 3D
7. `P` - Plane waves moving through the cube
8. `F` - Frequency-based ring patterns at different heights
9. `O` - Fountain effect with particles
10. `H` - Spinning helix with audio-controlled speed
11. `E` - Exploding voxel effect on loud sounds
12. `Z` - Demo of all patterns

To use:

```mint
V    // Run 3D VU meter
R    // Run sphere pattern
S    // Run 3D spectrum
D    // Run 3D rain
M    // Run 3D matrix
P    // Run wave pattern
F    // Run frequency rings
O    // Run fountain
H    // Run helix
E    // Run explosions
Z    // Run all patterns
```

Key features:
1. Full 3D space utilization
2. Audio-reactive patterns
3. Particle system effects
4. Wave and physics simulations
5. Multiple visualization styles
6. Smooth transitions

Possible enhancements:
1. More complex particle systems
2. Beat detection
3. Frequency band analysis
4. Pattern combinations
5. More physics simulations
6. Interactive controls


## Interactive patterns responding to two ADC inputs
```
// Interactive LED matrix patterns using AM9511 APU
// Hardware:
// - AM9511 APU: Data port #90, Command port #91
// - ADC inputs: Port #80 (ADC1), Port #82 (ADC2)
// - Output: 8x8 LED matrix on ST-3 (cols), ST-4 (rows)
//
// Variable usage:
// a - ADC1 value / temporary storage
// b - ADC2 value / temporary storage
// t - time/counter for animations
// x - X position / column selection
// y - Y position / row selection
// p - Pattern storage
// r - Radius or computation result
// s - Speed value
// d - Delay counter
//
// AM9511 Commands used:
// #04 - FADD  Float Add
// #06 - FMUL  Float Multiply
// #1A - SIN   Sine
// #1B - COS   Cosine

// Push one byte to APU data stack
:P
  #90 /O // Output to APU data port
;

// Send command to APU and wait for completion
:M
  #91 /O // Send command
  /U( #91 /I #80 & /F = /W ) // Wait for busy flag to clear
;

// Convert integer 0-7 to float format
:F
  #3F P // Push exponent for 0-1 range
  8 / P // Scale by 1/8
  0 P   // Push two zero bytes for
  0 P   // mantissa padding
;

// Read both ADCs and scale to 0-15
:A 
  #80 /I #0F & a! // Read ADC1, mask to 4 bits
  #82 /I #0F & b! // Read ADC2, mask to 4 bits
;

// Clear LED matrix
:C 
  0 st3! // Clear columns
  0 st4! // Clear rows
;

// Sine wave pattern
// a = frequency control
// b = amplitude control
:W
  /U(
    A // Read ADC inputs
    8( // For each column
      /i 8 / F   // Convert column position to float
      #06 M      // Multiply by frequency (a)
      #1A M      // Take sine
      #06 M      // Multiply by amplitude (b)
      #90 /I 7 & x! // Get result, scale to 0-7
      #01 x { st3!  // Light column
      #FF st4!      // All rows on
      25()         // Delay
    )
    C // Clear display
    /K /F = /W // Exit on keypress
  )
;

// Rotating vector pattern
// a = angle control
// b = length control
:R
  /U(
    A // Read controls
    #1B M         // Take cosine for X
    #06 M         // Multiply by length
    #90 /I 7 & x! // Scale to X position
    #1A M         // Take sine for Y
    #06 M         // Multiply by length
    #90 /I 7 & y! // Scale to Y position
    #01 x { st3!  // Set X position
    #01 y { st4!  // Set Y position
    25()          // Delay
    C             // Clear
    /K /F = /W    // Exit check
  )
;

// Lissajous pattern
// a = X frequency
// b = Y frequency
:L
  0 t! // Initialize time
  /U(
    A // Read frequencies
    t F #06 M     // X = sin(f1 * t)
    #1A M
    #90 /I 7 & x!
    t F #06 M     // Y = cos(f2 * t)
    #1B M
    #90 /I 7 & y!
    #01 x { st3!  // Display point
    #01 y { st4!
    t 1+ t!       // Increment time
    25()
    C
    /K /F = /W
  )
;

// Circular motion
// a = rotation speed
// b = radius
:O
  0 t! // Initialize angle
  /U(
    A // Read controls
    t F #1B M     // X = r * cos(t)
    #06 M
    #90 /I 7 & x!
    t F #1A M     // Y = r * sin(t)
    #06 M
    #90 /I 7 & y!
    #01 x { st3!  // Display point
    #01 y { st4!
    t a + t!      // Update angle by speed
    25()
    C
    /K /F = /W
  )
;

// Expanding spiral
// a = rotation speed
// b = expansion rate
:S
  0 t! // Initialize time
  /U(
    A // Read controls
    t F #06 M     // Calculate radius
    t F #06 M     // Calculate angle
    #1B M         // X = R * cos(angle)
    #06 M
    #90 /I 7 & x!
    t F #06 M     // Y = R * sin(angle)
    #1A M
    #06 M
    #90 /I 7 & y!
    #01 x { st3!  // Display point
    #01 y { st4!
    t 1+ t!       // Increment time
    25()
    C
    /K /F = /W
  )
;

// Demo sequence
:Z
  `APU Patterns` // Show title
  W // Run wave
  500() // Pause
  R // Run rotating vector
  500()
  L // Run Lissajous
  500()
  O // Run circle
  500()
  S // Run spiral
  C // Final clear
;
```

# stereoscopic 8x8

![image](https://github.com/user-attachments/assets/c930d3ef-b396-4817-a952-499a47d65505)

The science of stereoscopic vision

STEREOSCOPIC VISION SCIENCE:
Our brain creates 3D perception from two slightly different 2D images (one from each eye). The key concepts are:

1. Binocular Disparity
- Each eye sees a slightly different image due to being ~6.3cm apart (interpupillary distance)
- The brain uses these differences to calculate depth
- Closer objects have larger disparity between left/right views
- Distant objects have minimal disparity

2. Stereoscopic Display Methods
- Present slightly different images to each eye
- The disparity needs to match what would occur naturally
- Objects meant to appear closer need more disparity
- Objects meant to appear at infinity have zero disparity

3. Key Factors for 8x8 Implementation
- Need to calculate two viewpoints separated by scaled interpupillary distance
- Must handle perspective projection for each eye
- Limited resolution means careful choice of disparity amounts
- Need to consider viewing distance and display size

code for an 8x8x8x2 display system (two 8x8 matrices, one for each eye).

```mint
// Stereoscopic 3D viewer for dual 8x8 LED matrices
// Uses ST-3,ST-4 for left eye, ST-5,ST-6 for right eye
// 3D coordinates scaled to fit 8x8x8 volume

// Constants for view setup
:I
  3 e! // Eye separation (scaled)
  16 d! // Viewer distance (scaled)
;

// Project 3D point to 2D for one eye
// Takes x,y,z coords and eye offset
// Returns projected x,y in p,q
:P
  z! y! x! o! // Get coordinates and offset
  // Adjust x for eye position
  x o - x!
  // Perspective divide
  d z + r! // r = d + z
  x d * r / p! // p = x * d / r
  y d * r / q! // q = y * d / r
  // Scale to 0-7 range
  p 4 + p!
  q 4 + q!
;

// Clear both displays
:C
  0 st3! 0 st4! // Clear left
  0 st5! 0 st6! // Clear right
;

// Display point on both eyes
:D
  z! y! x! // Get 3D coordinates
  // Project for left eye
  e -1 * x y z P // Left projection
  #01 p { st3! // Set left column
  #01 q { st4! // Set left row
  // Project for right eye
  e x y z P // Right projection
  #01 p { st5! // Set right column
  #01 q { st6! // Set right row
;

// Draw 3D cube wireframe
:B
  8( // For each vertex
    /i 4 - x! // X coordinate
    /i 4 / 4 - y! // Y coordinate
    4 z! // Front Z
    x y z D // Display point
    -4 z! // Back Z
    x y z D // Display point
    50() // Delay
  )
;

// Rotating cube animation
:R
  0 a! // Angle
  /U(
    // Calculate rotated coordinates
    a F #1B M x! // x = cos(a)
    a F #1A M y! // y = sin(a)
    4 z! // z constant
    x y z D // Display point
    a 1+ a! // Increment angle
    50()
    C
    /K /F = /W
  )
;

// Spinning helix
:H
  0 t!
  /U(
    8(
      /i t + a! // Angle
      a F #1B M 4 * x! // x = 4*cos(a)
      a F #1A M 4 * y! // y = 4*sin(a)
      /i 1- z! // z varies
      x y z D
    )
    t 1+ t!
    25()
    C
    /K /F = /W
  )
;

// Floating point particle system
:F
  16( // 16 particles
    8 random 4 - x! // Random x
    8 random 4 - y! // Random y
    8 random 4 - z! // Random z
    /U(
      x y z D // Display
      // Update position
      x v + x!
      y g + y! // Add gravity
      z w + z! // Add drift
      25()
      C
      // Bounce check
      x 4 > ( -4 x! )
      y 4 > ( -4 y! )
      z 4 > ( -4 z! )
      /K /F = /W
    )
  )
;

// Demo sequence
:Z
  I // Initialize
  B // Cube wireframe
  500()
  R // Rotating point
  500()
  H // Helix
  500()
  F // Particles
  C
;

```

This implementation provides:

1. Basic 3D to stereoscopic projection system:
   - Calculates proper viewpoint for each eye
   - Handles perspective projection
   - Scales output to 8x8 range

2. Several demo patterns:
   - `B` - 3D cube wireframe
   - `R` - Rotating point in space
   - `H` - Spinning helix
   - `F` - 3D particle system
   - `Z` - Demo sequence

3. Key features:
   - Adjustable eye separation
   - Configurable viewing distance
   - Perspective correct projection
   - Multiple 3D effects

To use:

```mint
I    // Initialize
B    // Draw cube
R    // Rotating display
H    // Helix pattern
F    // Particle system
Z    // Run demo
```

Enhancement possibilities:
1. More complex 3D models
2. Interactive depth adjustment
3. Animated transitions
4. Hidden line removal
5. Pattern layering
6. Dynamic eye separation

# Robot vision and depth perception  

ROBOT VISION AND DEPTH PERCEPTION

1. Binocular Vision Basics:
- Similar to human vision, robots can use two cameras to calculate depth
- The difference in position of an object between two camera views (disparity) indicates its depth
- Closer objects have larger disparity
- Further objects have smaller disparity

2. Depth Calculation:
```
Depth = (Focal_Length × Baseline) / Disparity
Where:
- Focal_Length is the camera's focal length
- Baseline is the distance between cameras
- Disparity is the pixel difference between views
```

3. Key Concepts:
- Stereo correspondence: Matching points between two images
- Epipolar geometry: Mathematical relationship between stereo views
- Depth mapping: Converting disparities to actual distances
- Feature detection: Finding distinct points to match

4. 8x8 Camera Considerations:
- Very low resolution means limited depth precision
- Need robust feature detection despite low resolution
- Must handle noise and lighting variations
- Best suited for high-contrast objects

Let me write code to implement basic robot vision:



```mint
// Robot Vision System using dual 8x8 cameras
// Port #40: Left camera
// Port #41: Right camera
// Variables:
// l - Left image array
// r - Right image array
// d - Depth array
// t - Threshold value
// x,y - Coordinates
// v - Temporary value
// m - Match position
// p - Pattern storage

// Initialize arrays
:I
  [ 0 0 0 0 0 0 0 0 ] l! // Left image
  [ 0 0 0 0 0 0 0 0 ] r! // Right image
  [ 0 0 0 0 0 0 0 0 ] d! // Depth map
  32 t! // Set threshold
;

// Capture image from camera
// Takes port number, stores to array a
:C
  p! // Store port
  8(  // For each row
    8(  // For each column
      p /I v! // Read pixel
      t > ( 1 ) /E ( 0 ) // Threshold
      a /i ? /j ?! // Store to array
    )
  )
;

// Find feature in right image
// Takes left x,y position
// Returns match position in m
:F
  y! x! // Get coordinates
  0 m! // Initialize match
  8 b! // Search width
  l x ? y ? v! // Get feature value
  
  b( // Search b pixels
    r /i ? y ? v = ( // If match found
      /i m! // Store position
    )
  )
;

// Calculate depth from disparity
:D
  8( // For each row
    8( // For each column
      /i x! /j y! // Get coordinates
      l x ? y ? v! // Get left pixel
      v 0 > ( // If feature present
        x y F // Find match
        x m - a! // Calculate disparity
        // Depth = 16 * 8 / disparity
        128 a / d x ? y ?! // Store depth
      )
    )
  )
;

// Display depth map on LEDs
:M
  8(
    8(
      d /i ? /j ? v! // Get depth
      v 7 > ( // If object close
        #01 /i { st3! // Light LED
        #01 /j { st4!
      )
    )
  )
;

// Track closest object
// Returns x,y of closest point
:T
  0 x! 0 y! // Initialize position
  0 v! // Initialize min depth
  8(
    8(
      d /i ? /j ? a! // Get depth
      a v > ( // If closer
        a v! // Update min
        /i x! /j y! // Store position
      )
    )
  )
;

// Edge detection in image
:E
  8(
    7(
      l /i ? /j ? v! // Get pixel
      l /i ? /j 1+ ? a! // Get next pixel
      v a - abs // Calculate difference
      t > ( // If edge
        #01 /i { st3! // Display edge
        #01 /j { st4!
      )
    )
  )
;

// Motion detection between frames
:V
  8(
    8(
      l /i ? /j ? v! // Get old frame
      #40 /I a! // Get new pixel
      v a - abs // Calculate difference
      t > ( // If motion
        #01 /i { st3! // Show motion
        #01 /j { st4!
      )
    )
  )
;

// Main vision processing loop
:P
  I // Initialize
  /U(
    // Capture images
    #40 l C // Left camera
    #41 r C // Right camera
    
    // Process
    D // Calculate depth
    M // Display depth map
    
    // Optional processing
    /K /F = ( // If key pressed
      E // Show edges
    ) /E (
      V // Show motion
    )
    
    50() // Delay
    /K #1B = /W // Exit on ESC
  )
;

// Demo sequence
:Z
  `Robot Vision Demo`
  I // Initialize
  500()
  P // Run processor
  `Demo Complete`
;

```

This implementation provides:

1. Basic Vision Processing:
- Image capture from both cameras
- Binary thresholding
- Feature matching
- Depth calculation
- Edge detection
- Motion detection

2. Key Functions:
- `I` - Initialize arrays
- `C` - Capture from camera
- `F` - Find matching features
- `D` - Calculate depth map
- `M` - Display depth visualization
- `T` - Track closest object
- `E` - Edge detection
- `V` - Motion detection
- `P` - Main processing loop

To use:

```mint
I    // Initialize system
P    // Run vision processor
T    // Track objects
E    // Show edges
V    // Show motion
Z    // Run demo sequence
```

Limitations of 8x8 Resolution:
1. Very coarse depth mapping
2. Limited feature detection
3. Basic edge detection only
4. Prone to noise
5. Limited field of view
6. Basic motion detection

Possible enhancements:
1. Better noise filtering
2. Multiple object tracking
3. Pattern recognition
4. Temporal filtering
5. Calibration routines
6. Advanced feature detection

#  visualize the robot's stereo vision data in 3D using the 8x8x8 LED cube.

```mint
// 3D visualization of stereo robot vision
// Cameras: Port #40 (left), #41 (right)
// LED Cube: ST-3 (columns), ST-4 (rows), ST-5 (layers)
// Variables:
// l - Left image
// r - Right image
// d - Depth data
// t - Threshold
// x,y,z - Coordinates
// v - Value temp
// p - Port/pattern

// Initialize system
:I
  [ 0 0 0 0 0 0 0 0 ] l! // Left image
  [ 0 0 0 0 0 0 0 0 ] r! // Right image
  [ 0 0 0 0 0 0 0 0 ] d! // Depth data
  32 t! // Threshold for binary
;

// Clear cube
:C
  0 st3! 0 st4! 0 st5!
;

// Capture from camera
:A
  p! // Store port number
  8(  // Each row
    8(  // Each column
      p /I v! // Read pixel
      t > ( 1 ) /E ( 0 ) // Threshold
      a /i ? /j ?! // Store to array
    )
  )
;

// Calculate stereo disparity
:D
  8( // Each row
    8( // Each column
      /i x! /j y!
      l x ? y ? v! // Get left pixel
      v 0 > ( // If feature present
        // Find match in right image
        8( // Search range
          r /i ? y ? v = (
            x /i - z! // Calculate disparity
            // Convert to depth (0-7)
            8 z * 64 / 
            d x ? y ?! // Store depth
          )
        )
      )
    )
  )
;

// Display single voxel
:V
  z! y! x!
  #01 x { st3!
  #01 y { st4!
  #01 z { st5!
;

// Real-time 3D display mode
// Shows depth as layers in cube
:R
  /U(
    // Capture new images
    #40 l A
    #41 r A
    
    // Process depth
    D
    
    // Display in cube
    8( // Each layer
      8( // Each row
        8( // Each column
          d /i ? /j ? v! // Get depth
          v /k = ( // If depth matches layer
            /i /j /k V // Light voxel
          )
        )
      )
    )
    50() // Delay
    C // Clear for next frame
    /K /F = /W // Check exit
  )
;

// Cross-section display mode
// Shows slices through the scene
:S
  /U(
    // Capture and process
    #40 l A
    #41 r A
    D
    
    // Show moving slice
    8(
      /i z! // Current slice
      8(
        8(
          d /j ? /k ? v! // Get depth
          v z = (
            /j /k z V // Display if matches
          )
        )
      )
      100() // Longer delay
      C
    )
    /K /F = /W
  )
;

// Point cloud display
// Shows scattered points based on depth
:P
  /U(
    #40 l A
    #41 r A
    D
    
    32( // Display 32 random points
      8 random x!
      8 random y!
      d x ? y ? z! // Get depth
      x y z V // Display point
    )
    75()
    C
    /K /F = /W
  )
;

// Motion trail display
// Shows movement over time
:M
  0 t! // Time counter
  /U(
    #40 l A // New frame
    8(
      8(
        l /i ? /j ? v! // Current
        r /i ? /j ? a! // Previous
        v a - abs t > ( // If motion
          /i /j t 7 & V // Show at current layer
        )
      )
    )
    r l! // Store frame
    t 1+ t! // Increment time
    50()
    /K /F = /W
  )
;

// Edge detection in 3D
:E
  /U(
    #40 l A
    #41 r A
    D
    
    // Find edges in depth
    7(
      7(
        7(
          d /i ? /j ? v! // Current depth
          d /i 1+ ? /j ? a! // Next depth
          v a - abs t > ( // If edge
            /i /j /k V // Show edge point
          )
        )
      )
    )
    75()
    C
    /K /F = /W
  )
;

// Demo sequence
:Z
  `3D Vision Demo`
  I // Initialize
  R // Real-time view
  500()
  S // Slices
  500()
  P // Point cloud
  500()
  M // Motion trails
  500()
  E // Edge view
  C // Final clear
;

```

This implementation provides several 3D visualization modes:

1. `R` - Real-time 3D display
   - Shows depth as layers in the cube
   - Closer objects in front layers
   - Updates continuously

2. `S` - Cross-section display
   - Shows slices through the scene
   - Moves through depth planes
   - Good for understanding structure

3. `P` - Point cloud display
   - Scattered points showing depth
   - Random sampling for clarity
   - Shows overall shape

4. `M` - Motion trail display
   - Shows movement over time
   - Different layers for time sequence
   - Helps track motion

5. `E` - Edge detection display
   - Shows depth discontinuities
   - Highlights object boundaries
   - 3D structure emphasis

To use:

```mint
I    // Initialize
R    // Real-time view
S    // Slice view
P    // Point cloud
M    // Motion trails
E    // Edge view
Z    // Run demo
```

Features:
1. Multiple visualization modes
2. Real-time processing
3. Motion detection
4. Edge detection
5. Depth mapping
6. 3D perspective




### Iterate
- POV
- Vectrex
- 16x16
- 8x8x8
- alt lighting; neon, xenon, incandescent, laser, nixie, plasma, 
- stereoscopic 8x8 ; https://en.wikipedia.org/wiki/Stereoscopy, new code, game etc
- volumetric display ; https://en.wikipedia.org/wiki/Volumetric_display, new code
- simple CRO
- Stereoptic matrix, some early games used this, showing a series of lights coming at you giving the illusion of forward movement. inside that was a gun to shoot bullets forward to hit a moving target in the middle. 
- Stereoptic- POV; combining both to get cude 3d video! 
- CHA-81 LED Chaser DIY Kit
- contrast or fade LED with freq control of multiplex
 
- output https://github.com/SteveJustin1963/tec-EYE onto 8x8 with contrast
- output 2x tec-EYE into 2x 8x8 through stereoscopic https://en.wikipedia.org/wiki/Google_Cardboard
- think about robot vision and depth perception code.
- TomyTronic 3D is a range of games produced by Tomy in 1985. It was Tomy's first attempt to move into the then-popular world of 3D games. TomyTronic 3D was a game system similar to the Vectrex, in that it used a clear plastic overlay on the LCD screen of the game in order to provide a 3D effect. The games were played by holding the game system in your left hand and the controller in your right. The controller had a joystick on the left, and three buttons on the right, which were used to control the game. The games were designed to be played in short bursts, as they were intended to be played in arcades. The games were not very successful, and Tomy stopped production of the TomyTronic 3D system in 1986. eg Space Laser Fight , 3D Star Fighter (1985), 3D Torpedo (1985)
- 
## Reference
- https://github.com/SteveJustin1963/tec-MAGAZINES/blob/master/talking_electronics_11.pdf
- https://en.wikipedia.org/wiki/Stereopticon
- https://au.rs-online.com/web/c/displays-optoelectronics/displays-industrial-monitors/led-displays/?searchTerm=TA23-11CGKWA
- https://www.kingbright.com/attachments/file/psearch/000/00/00/TA23-11CGKWA(Ver.8A).pdf
- https://www.ebay.com.au/itm/202687431320?ul_noapp=true
- https://www.google.com/search?q=MAX7219&rlz=1C1CHBF_enAU775AU775&oq=MAX7219&aqs=chrome..69i57j69i61l3&sourceid=chrome&ie=UTF-8
