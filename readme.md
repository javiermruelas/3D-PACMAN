# Low-Poly 3D Pac-Man Game Implementation (React + Three Fiber + Zustand)

This project is a 3D implementation of the classic Pac-Man game using React for the user interface, React Three Fiber for 3D rendering, and Zustand for state management. The game features a low-poly aesthetic.

## Key Concepts and Considerations:

- **Zustand for Game State:** Zustand is used to manage the global game state (score, lives, level, player position, ghost positions, etc.). This makes it easy to access and update game data from any component.
- **React Three Fiber for 3D Rendering:** React Three Fiber handles the 3D scene rendering. Components like `<Canvas>`, `<mesh>`, and `<geometry>` are used to create the 3D world.
- **Component-Based Approach:** The game is broken down into reusable components (Pac-Man, Ghost, Maze, Pellet) for better organization and maintainability.
- **Maze Generation:** The `generateMazeData()` function in the Zustand store creates the 3D representation of the Pac-Man maze. **You will need to replace the example maze data with your actual maze data.**
- **Pellet Management:** The `generatePellets` function creates the initial set of pellets. The `eatPellet` function in the Zustand store handles removing pellets when Pac-Man eats them and updating the score.
- **Ghost AI:** The `Ghost` component includes basic movement, but you will need to implement more sophisticated AI logic for the ghosts' behavior (pathfinding, chasing, etc.).
- **Pac-Man Movement:** The `movePacman` function in the Zustand store updates Pac-Man's position based on input and handles wall collision detection. A very basic wall collision has been added, and **you will want to improve this.**
- **Camera Control:** The `useFrame` hook in `GameScene.tsx` can be used to control the camera's position and orientation. A simple camera follow has been added.
- **Game Loop:** The `useFrame` hook in `GameScene.tsx` runs on every frame and is used for animations, updating game logic, and rendering.
- **3D Maze Data:** The maze is represented as a 3D array of numbers. For example, `1` represents a wall, and `0` represents an empty space. The 3rd dimension is used to represent the height, but Pacman is a 2D game, so the height is usually `1`.

## Next Steps:

- **Implement Maze Data:** Replace the placeholder maze data in `generateMazeData()` with the actual data for your Pac-Man maze. **This is CRUCIAL.**
- **Flesh Out Ghost AI:** Implement the AI logic for the ghosts in the `Ghost` component and the Zustand store. This is the most complex part of the game.
- **Complete Pac-Man Movement:** Implement full Pac-Man movement logic, including handling user input (keyboard or gamepad) and preventing movement through walls. Improve wall collision.
- **Add Collision Detection:** Implement collision detection between Pac-Man and ghosts, Pac-Man and pellets, and Pac-Man and power pellets. Update the score and game state accordingly.
- **Implement Game Logic:** Implement the core game logic, including scoring, lives, levels, game over conditions, and starting/restarting the game. The Zustand store is the place for this.
- **Animate with Anime.js:** Use Anime.js to add visual effects, such as Pac-Man's mouth animation, ghost animations, and other visual enhancements.
- **Sound Effects:** Add sound effects for Pac-Man movement, eating pellets, and other game events.
- **Polish the UI:** Use Tailwind CSS to style the in-game UI (score, lives, level) and add any other UI elements you need.

## 3D Pac-Man Game Implementation Plan (Low-Poly)

Here's a structured plan to guide your development, broken down into key areas with tasks, assets, and considerations:

### I. Project Setup and Core Structure

**Tasks:**

- **Set up Vite Project:**
  ```bash
  npm create vite@latest my-pacman-game -- --template react-ts
  cd my-pacman-game
  npm install
  ```
- **Install Dependencies:**
  ```bash
  npm install three @types/three animejs @react-three/fiber @react-three/drei zustand tailwindcss postcss autoprefixer
  ```
- **Create Core Directories and Files:**
  - Create the directory structure as outlined in your project (e.g., `src/components`, `src/scenes`, `src/store`).
  - Create the initial files (e.g., `App.tsx`, `GameScene.tsx`, `Pacman.tsx`, `gameStore.ts`).
- **Configure Tailwind CSS:**
  - Set up `tailwind.config.js` and `postcss.config.js`.
- **Initial `App.tsx` and `<Canvas>` Setup:**
  - Implement the basic `App.tsx` with the `<Canvas>` component from `@react-three/fiber`.

### II. Maze Design and Implementation

**Tasks:**

- **Maze Data Representation:**
  - Define the 3D array structure for your maze data in `gameStore.ts` (e.g., `number[][][]`, where `1` = wall, `0` = path).
- **Maze Data Source:**
  - **Option A: Manual Design:**
    - Create a 2D representation of your maze in a text file, JSON, or a simple editor.
    - Write a function to convert this 2D representation into the 3D array format.
  - **Option B: Procedural Generation (Less Recommended for Pac-Man):**
    - If you want some procedural elements, research simple maze generation algorithms (e.g., Randomized Depth-First Search, Recursive Backtracking). Adapt the algorithm to your 3D representation. **This is generally not how Pac-Man mazes are made, as they are very specific.**
- **`Maze.tsx` Component:**
  - Implement the `Maze.tsx` component to render the maze from the 3D array data.
  - Use `<mesh>` and `<boxGeometry>` for walls.
  - Apply a low-poly material (`MeshStandardMaterial` with a flat shading).

**Assets:**

- Basic Cube (for walls)

**Data:**

- 3D array representation of the maze.

**Considerations:**

- Pac-Man mazes are very specific. Procedural generation is usually not appropriate. Manual design is preferred.
- Ensure the maze data is easily editable.
- Plan for the ghost house and how it's represented in the data.

### III. Pac-Man Movement and Control

**Tasks:**

- **`Pacman.tsx` Component:**
  - Create the `Pacman.tsx` component.
  - Use `<mesh>` and `<sphereGeometry>` (or a slightly modified sphere for a more Pac-Man-like shape).
  - Apply a yellow, low-poly material.
- **Zustand State for Pac-Man:**
  - In `gameStore.ts`, manage Pac-Man's:
    - Position (`x`, `y`, `z`)
    - Direction (`x`, `z`)
    - Mouth animation state (open/closed)
- **`movePacman()` Function in Zustand:**
  - Implement the `movePacman()` function in `gameStore.ts`:
    - Update Pac-Man's position based on direction and speed.
    - Implement **improved wall collision detection**:
      - Check the next position for wall collisions before updating Pac-Man's position.
      - Handle cases where Pac-Man is partially in a wall.
      - Consider using `Math.floor()` and `Math.ceil()` to check the grid cells around Pac-Man.
- **Input Handling:**
  - In `Pacman.tsx` or a separate input handler:
    - Use `useEffect` to listen for keyboard (or gamepad) input.
    - Update Pac-Man's direction in the Zustand store based on input.
- **Mouth Animation:**
  - Use `useFrame` in `Pacman.tsx` and `animejs` to animate Pac-Man's mouth opening and closing. Control the `morphTargets` of the sphere, or scale it.

**Assets:**

- Low-Poly Sphere (or modified sphere) for Pac-Man

**Data:**

- Pac-Man's position and direction in the Zustand store.

**Algorithms:**

- Improved wall collision detection (grid-based checking).

**Considerations:**

- Smooth movement.
- Correctly handling turns and wall collisions.
- Mouth animation synced with movement.

### IV. Ghost Behavior and AI

**Tasks:**

- **`Ghost.tsx` Component:**
  - Create the `Ghost.tsx` component.
  - Use `<mesh>` and `<boxGeometry>` (or a custom low-poly ghost shape).
  - Apply different colors for each ghost.
- **Ghost Data in Zustand:**
  - In `gameStore.ts`, define the data structure for each ghost:
    - `name`
    - `color`
    - `position` (`x`, `y`, `z`)
    - `direction` (`x`, `z`)
    - `speed`
    - `aiState` (e.g., chasing, scatter, frightened)
- **Ghost AI Logic:**
  - Implement the ghost AI logic in `gameStore.ts` and update ghost positions in `useFrame` of `GameScene.tsx`:
    - **Basic Movement:** Random movement with wall avoidance.
    - **Scatter Mode:** Ghosts move to their designated corners of the maze.
    - **Chase Mode:** Ghosts target Pac-Man's position (different strategies for each ghost).
    - **Frightened Mode:** Ghosts move randomly and can be eaten by Pac-Man.
- **Pathfinding (for Chasing):**
  - **Option A: Simple Following:** For a very basic game, ghosts can move directly towards Pac-Man, but this gets stuck on walls.
  - **Option B: Breadth-First Search (BFS):** A good algorithm for finding the shortest path in an unweighted graph (your maze grid).
  - **Option C: A\* Search (A-Star):** More efficient than BFS if you want to add heuristics (e.g., distance to Pac-Man). A\* is generally preferred for game pathfinding.
- **Wall Collision:** Implement wall collision for ghosts, similar to Pac-Man.
  - Implement wall collision detection for ghosts using the same grid-based approach as Pac-Man.

**Assets:**

- Low-Poly Ghost models (can be simple variations of a cube)

**Data:**

- Ghost data in the Zustand store (position, direction, AI state).

**Algorithms:**

- Basic movement with wall avoidance
- BFS or A\* for pathfinding (recommended for good AI)

**Considerations:**

- Different AI personalities for each ghost.
- Transitions between AI states (scatter, chase, frightened).
- Efficient pathfinding.

### V. Pellet and Power Pellet Management

**Tasks:**

- **`Pellet.tsx` and `PowerPellet.tsx` Components:**
  - Create the `Pellet.tsx` and `PowerPellet.tsx` components.
  - Use `<mesh>` and `<sphereGeometry>` for both.
  - Apply different materials (white for regular, a distinct color for power).
- **Pellet Data in Zustand:**
  - In `gameStore.ts`, store the pellet data:
    ```typescript
    pellets: {
      id: string;
      position: {
        x: number;
        y: number;
        z: number;
      }
    }
    [];
    powerPellets: {
      id: string;
      position: {
        x: number;
        y: number;
        z: number;
      }
    }
    [];
    ```
- **`generatePellets()` and `generatePowerPellets()` Functions:**
  - Implement these functions in `gameStore.ts` to:
    - Place pellets in the maze (based on the `mazeData`).
    - Ensure pellets are not placed on walls.
- **`eatPellet()` Function in Zustand:**
  - Implement the `eatPellet()` function in `gameStore.ts`:
    - Remove the eaten pellet from the `pellets` array.
    - Update the score.
- **Power Pellet Effects:**
  - In `eatPellet()`, add logic for power pellet effects:
    - Change ghost states to frightened.
    - Allow Pac-Man to eat ghosts.
    - Set a timer for the power-up duration.

**Assets:**

- Low-Poly Spheres (different colors/sizes)

**Data:**

- Pellet and power pellet arrays in the Zustand store.

**Considerations:**

- Pellet placement based on maze layout.
- Power pellet effects and duration.

### VI. Game Logic and State Management

**Tasks:**

- **Zustand Store (`gameStore.ts`):**
  - Manage the following game state in Zustand:
    - `gameStarted`
    - `gameOver`
    - `score`
    - `lives`
    - `level`
    - `gameState` (e.g., playing, paused, gameOver)
- **`initializeGame()` Function:**
  - Implement this function in `gameStore.ts` to:
    - Reset the game state.
    - Generate the maze.
    - Place Pac-Man, ghosts, and pellets.
- **`startGame()` and `restartGame()` Functions:**
  - Implement these functions in `gameStore.ts` to control the game flow.
- **Scoring:**
  - Update the score in `eatPellet()` and `eatGhost()`.
- **Lives and Game Over:**
  - Manage Pac-Man's lives.
  - Determine when the game is over.
- **Levels:**
  - Implement level progression (new maze layouts, increased ghost speed, etc.).
- **Collision Detection:**
  - Implement collision detection:
    - Pac-Man and pellets (in `movePacman()` or a separate function).
    - Pac-Man and ghosts (in `useFrame` of `GameScene.tsx`).
    - Ghosts and walls (in `useFrame` of `GameScene.tsx`).

**Data:**

- Game state in the Zustand store.

**Considerations:**

- Clear separation of game logic and rendering.
- Properly updating the game state.
- Handling game over and level transitions.

### VII. Visual Enhancements and Polish

**Tasks:**

- **Camera Control:**
  - Use `useFrame` in `GameScene.tsx` to control the camera.
  - Smoothly follow Pac-Man.
  - Consider different camera angles.
- **Animations (Anime.js):**
  - Use Anime.js for:
    - Pac-Man's mouth animation.
    - Ghost animations (e.g., flickering when frightened).
    - Power pellet pulsing effect.
    - Transitions.
- **Lighting and Materials:**
  - Use Three.js lights (`AmbientLight`, `DirectionalLight`) to illuminate the scene.
  - Use `MeshStandardMaterial` with appropriate colors and shading (flat shading for low-poly).
- **UI (Tailwind CSS):**
  - Style the in-game UI (score, lives, level) using Tailwind CSS.
  - Create a start screen and game over screen.
- **Sound Effects:**
  - (Optional) Add sound effects for:
    - Pac-Man movement.
    - Eating pellets.
    - Eating ghosts.
    - Game start/end.

**Assets:**

- (Optional) Sound files

**Considerations:**

- Low-poly aesthetic consistency.
- Smooth animations.
- Clear and informative UI.

### VIII. Testing and Refinement

**Tasks:**

- **Playtesting:** Thoroughly test the game to identify bugs and areas for improvement.
- **Debugging:** Use browser developer tools and React DevTools to debug issues.
- **Performance Optimization:** Optimize the game's performance if needed (e.g., reduce the number of draw calls, optimize Three.js materials).
- **Code Refactoring:** Refactor your code for better readability and maintainability.
- **Polishing:** Fine-tune the gameplay, visuals, and UI.

### IX. Further Development (Optional)

**Tasks:**

- More Complex Ghost AI: Implement more advanced ghost behavior, such as coordinated attacks and path prediction.
- New Game Modes: Add new game modes or variations.
- Multiplayer: Explore adding a multiplayer mode.
- Power-Ups: Add more power-ups with different effects.
