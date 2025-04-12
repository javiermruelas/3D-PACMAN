# Low-Poly 3D Pac-Man Game Implementation (React + Three Fiber + Zustand)

This project aims to recreate the classic Pac-Man game in a 3D environment with a distinctive low-poly aesthetic. It leverages the power of React for the user interface, React Three Fiber for seamless 3D rendering within React, and Zustand for robust global state management.

## Core Architecture and Technologies

This game's foundation rests on the following key technologies and architectural choices:

- **React for UI:** Provides a component-based structure for building the user interface elements of the game (score display, lives, etc.).
- **React Three Fiber for 3D Rendering:** Acts as the bridge between React and Three.js, enabling the creation and manipulation of 3D scenes and objects using React components (`<Canvas>`, `<mesh>`, `<geometry>`, etc.).
- **Zustand for Global State Management:** Centralizes the game's dynamic data (score, lives, level, player/ghost positions, etc.), making it easily accessible and modifiable across different components.

## Key Game Elements and Their Implementation

The game world and its inhabitants are structured as reusable components, each with specific responsibilities:

- **Maze (`Maze.tsx`):**
  - The static environment of the game. Its layout is defined by a 3D array of numbers within the Zustand store.
  - **Important:** The `generateMazeData()` function in the Zustand store is responsible for creating this 3D representation. **You MUST replace the example maze data with your actual Pac-Man maze data.** The convention is that `1` represents a wall, `0` an empty space, and the third dimension typically has a height of `1` for this 2D gameplay.
- **Pac-Man (`Pacman.tsx`):**
  - The player-controlled character. Its movement and state (position, direction, mouth animation) are managed within its component and the Zustand store.
  - The `movePacman` function in the Zustand store updates Pac-Man's position based on player input and incorporates basic wall collision detection. **This initial collision detection is rudimentary and requires significant improvement.**
- **Ghost (`Ghost.tsx`):**
  - The antagonists of the game. Each ghost will have its own component and data within the Zustand store (position, color, AI state).
  - The component currently includes basic movement. **Implementing sophisticated AI logic (pathfinding, chasing, scatter behavior, etc.) is a crucial next step.**
- **Pellet (`Pellet.tsx`) and Power Pellet (`PowerPellet.tsx`):**
  - Collectible items scattered throughout the maze. Their generation and consumption are managed by functions in the Zustand store.
  - The `generatePellets` function creates the initial pellet layout.
  - The `eatPellet` function in the Zustand store handles removing eaten pellets, updating the score, and triggering power-up effects for power pellets.

## Core Game Loop and Dynamics

The game's real-time aspects are handled within the `GameScene.tsx` component:

- **Game Loop (`useFrame`):** This React Three Fiber hook executes on every frame, driving animations, updating game logic (like ghost movement and collision checks), and re-rendering the 3D scene.
- **Camera Control (`useFrame`):** The camera's position and orientation are managed here, currently implementing a simple follow behavior for Pac-Man.

## State Management (Zustand)

Zustand plays a central role in managing the game's dynamic data:

- **Global Game State:** Stores all essential game information, including score, lives, level, player and ghost positions, and the maze and pellet data.
- **State Update Functions:** Provides functions like `movePacman`, `eatPellet`, and `generateMazeData` to modify the game state in a controlled manner.

## Visual Enhancements and User Interface

- **Low-Poly Aesthetic:** The game will utilize simple geometries and flat shading to achieve a distinct low-polygon visual style.
- **Anime.js for Animations:** This library will be used to create visual effects such as Pac-Man's mouth movement and potentially ghost animations.
- **Tailwind CSS for UI:** Tailwind CSS will style the in-game user interface elements (score, lives, level display).

## Development Roadmap and Next Steps

To guide the development process, here's a structured plan outlining key areas and their respective tasks:

### I. Core Project Setup

1.  **Initialize Project:**
    ```bash
    npm create vite@latest
    ```
    Select `React` and `TypeScript + SWC`.
2.  **Install Dependencies:**
    ```bash
    npm install three @types/three animejs @react-three/fiber @react-three/drei zustand tailwindcss postcss autoprefixer
    ```
3.  **Set up ShadCN UI:** Follow the instructions at [[https://ui.shadcn.com/docs/installation/vite](https://ui.shadcn.com/docs/installation/vite)].
4.  **Create Directory Structure:** Organize your project files into logical directories (e.g., `src/components`, `src/scenes`, `src/store`).
5.  **Create Initial Files:** Set up essential files like `App.tsx`, `GameScene.tsx`, `Pacman.tsx`, and `gameStore.ts`.
6.  **Configure Tailwind CSS:** Configure `tailwind.config.js` and `postcss.config.js`.
7.  **Basic `<Canvas>` Setup:** Implement the initial `App.tsx` with the `<Canvas>` component from `@react-three/fiber` to establish the 3D rendering context.

### II. Maze Implementation

1.  **Define Maze Data Structure:** In `gameStore.ts`, finalize the 3D array structure for representing the maze (e.g., `number[][][]`).
2.  **Implement Maze Data Source:**
    - **Crucially:** Design your Pac-Man maze layout (e.g., in a text file or JSON).
    - Write the `generateMazeData()` function in `gameStore.ts` to convert your maze design into the required 3D array format.
3.  **Create `Maze.tsx` Component:**
    - Render the maze walls using `<mesh>` and `<boxGeometry>` based on the `mazeData`.
    - Apply a `MeshStandardMaterial` with flat shading for the low-poly look.

### III. Pac-Man Movement and Control

1.  **Create `Pacman.tsx` Component:**
    - Render Pac-Man using `<mesh>` and `<sphereGeometry>` (or a slightly modified shape).
    - Apply a yellow, low-poly material.
2.  **Manage Pac-Man State in Zustand:**
    - Store Pac-Man's `position` (x, y, z), `direction` (x, z), and mouth animation state in `gameStore.ts`.
3.  **Implement `movePacman()` Function:**
    - Update Pac-Man's position in `gameStore.ts` based on its direction and speed.
    - **Significantly improve wall collision detection:** Implement robust checks against the maze data before allowing movement, considering Pac-Man's size and potential partial overlaps.
4.  **Handle User Input:**
    - In `Pacman.tsx` or a dedicated input handler, listen for keyboard or gamepad input.
    - Update Pac-Man's `direction` in the Zustand store accordingly.
5.  **Implement Mouth Animation:**
    - Use `useFrame` in `Pacman.tsx` and Anime.js to animate Pac-Man's mouth opening and closing, potentially by manipulating `morphTargets` or scaling.

### IV. Ghost Behavior and AI

1.  **Create `Ghost.tsx` Component:**
    - Render each ghost using `<mesh>` and a suitable low-poly geometry (e.g., a stylized cube).
    - Apply distinct colors for each ghost.
2.  **Manage Ghost State in Zustand:**
    - Store data for each ghost: `name`, `color`, `position`, `direction`, `speed`, and `aiState` (chasing, scatter, frightened).
3.  **Implement Ghost AI Logic:**
    - Within `gameStore.ts` and the `useFrame` in `GameScene.tsx`, implement the different AI behaviors:
      - **Basic Movement:** Implement random movement with wall avoidance.
      - **Scatter Mode:** Define target corners for each ghost and implement movement towards them.
      - **Chase Mode:** Implement pathfinding algorithms (BFS or, ideally, A\*) to navigate the maze towards Pac-Man. Consider different targeting strategies for each ghost.
      - **Frightened Mode:** Implement random movement and the ability for Pac-Man to eat them.
4.  **Implement Ghost Wall Collision:** Use a similar grid-based approach as Pac-Man to prevent ghosts from moving through walls.

### V. Pellet and Power Pellet Management

1.  **Create `Pellet.tsx` and `PowerPellet.tsx`:**
    - Render pellets and power pellets using `<mesh>` and `<sphereGeometry>`.
    - Apply distinct materials.
2.  **Manage Pellet Data in Zustand:**
    - Store arrays for `pellets` and `powerPellets`, each containing their unique `id` and `position`.
3.  **Implement `generatePellets()` and `generatePowerPellets()`:**
    - Populate the `pellets` and `powerPellets` arrays in `gameStore.ts` based on the `mazeData`, ensuring they are placed in empty spaces.
4.  **Implement `eatPellet()`:**
    - Remove the eaten pellet from the corresponding array in the Zustand store.
    - Increment the `score`.
    - If a power pellet is eaten, change the ghosts' `aiState` to frightened and start a timer.

### VI. Core Game Logic Implementation

1.  **Manage Game State in Zustand:**
    - Add state variables for `gameStarted`, `gameOver`, `score`, `lives`, `level`, and `gameState` to `gameStore.ts`.
2.  **Implement `initializeGame()`:**
    - Reset game state variables.
    - Call `generateMazeData()`, `generatePellets()`, and position Pac-Man and the ghosts in their starting locations.
3.  **Implement `startGame()` and `restartGame()`:**
    - Control the flow of the game (e.g., setting `gameStarted` to true).
4.  **Implement Scoring Logic:**
    - Update the `score` in `eatPellet()` and when Pac-Man eats a frightened ghost.
5.  **Manage Lives and Game Over:**
    - Decrement `lives` upon collision with a non-frightened ghost.
    - Set `gameOver` to true when lives reach zero.
6.  **Implement Level Progression:**
    - Define conditions for advancing to the next level (e.g., eating all pellets).
    - Implement logic to increase the `level`, potentially changing the maze, ghost speed, etc.
7.  **Implement Collision Detection:**
    - **Pac-Man and Pellets:** Check for proximity in `movePacman()` or a separate collision detection function.
    - **Pac-Man and Ghosts:** Check for proximity in the `useFrame` of `GameScene.tsx`, considering the ghosts' `aiState`.
    - **Ghosts and Walls:** Already addressed in the Ghost AI section.

### VII. Visual Enhancements and Polish

1.  **Refine Camera Control:**
    - Smoothly follow Pac-Man's movement using `useFrame`.
    - Experiment with different camera angles for the best perspective.
2.  **Implement Animations with Anime.js:**
    - Animate Pac-Man's mouth opening and closing.
    - Animate ghosts (e.g., flickering in frightened mode).
    - Add a pulsing effect to power pellets.
    - Consider animations for game transitions.
3.  **Add Lighting and Materials:**
    - Use `AmbientLight` and potentially `DirectionalLight` to illuminate the scene.
    - Ensure all objects use `MeshStandardMaterial` with appropriate colors and flat shading.
4.  **Style UI with Tailwind CSS:**
    - Create and style the in-game UI (score, lives, level display).
    - Design and implement start and game over screens.
5.  **(Optional) Add Sound Effects:** Integrate sound effects for key game events.

### VIII. Testing and Refinement

1.  **Thorough Playtesting:** Play the game extensively to identify bugs and areas for improvement in gameplay and visuals.
2.  **Debugging:** Use browser developer tools and React DevTools to diagnose and fix issues.
3.  **Performance Optimization:** If performance becomes an issue, optimize rendering and game logic.
4.  **Code Refactoring:** Improve code readability and maintainability through refactoring.
5.  **Gameplay Polishing:** Fine-tune movement, collision detection, AI behavior, and overall game feel.

### IX. Further Development (Optional)

- Implement more complex and varied ghost AI.
- Add new game modes or maze variations.
- Explore the possibility of a multiplayer mode.
- Introduce additional power-ups with unique effects.
