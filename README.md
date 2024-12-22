import java.util.Scanner;
import java.util.Random;

class MazeSolver {
    private static final int SIZE = 10; // Size of the maze
    private char[][] maze;
    private boolean[][] visited;
    private int startX, startY, endX, endY;

    public MazeSolver() {
        maze = new char[SIZE][SIZE];
        visited = new boolean[SIZE][SIZE];
    }

    public void generateMaze() {
        Random random = new Random();
        for (int i = 0; i < SIZE; i++) {
            for (int j = 0; j < SIZE; j++) {
                maze[i][j] = (random.nextInt(4) == 0) ? '#' : '.'; // 25% walls
            }
        }
    }

    public void setStartAndEnd(int startX, int startY, int endX, int endY) {
        this.startX = startX;
        this.startY = startY;
        this.endX = endX;
        this.endY = endY;
        maze[startX][startY] = 'S';
        maze[endX][endY] = 'E';
    }

    public void displayMaze() {
        for (int i = 0; i < SIZE; i++) {
            for (int j = 0; j < SIZE; j++) {
                System.out.print(maze[i][j] + " ");
            }
            System.out.println();
        }
    }

    public boolean solveMaze() {
        return dfs(startX, startY);
    }

    private boolean dfs(int x, int y) {
        // Out of bounds or hitting a wall or already visited
        if (x < 0 || x >= SIZE || y < 0 || y >= SIZE || maze[x][y] == '#' || visited[x][y]) {
            return false;
        }

        visited[x][y] = true; // Mark current cell as visited

        // If we reach the end
        if (x == endX && y == endY) {
            maze[x][y] = '*'; // Mark path
            return true;
        }

        // Try moving in all four directions
        if (dfs(x - 1, y) || dfs(x + 1, y) || dfs(x, y - 1) || dfs(x, y + 1)) {
            maze[x][y] = '*'; // Mark path
            return true;
        }

        return false; // No path found
    }

    public void resetVisited() {
        for (int i = 0; i < SIZE; i++) {
            for (int j = 0; j < SIZE; j++) {
                visited[i][j] = false;
                if (maze[i][j] == '*') { // Reset path markings
                    maze[i][j] = '.';
                }
            }
        }
    }
}

public class MazeGame {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        MazeSolver mazeSolver = new MazeSolver();

        System.out.println("Generating a random maze...");
        mazeSolver.generateMaze();

        System.out.println("Enter the starting point (x y):");
        int startX = scanner.nextInt();
        int startY = scanner.nextInt();

        System.out.println("Enter the ending point (x y):");
        int endX = scanner.nextInt();
        int endY = scanner.nextInt();

        mazeSolver.setStartAndEnd(startX, startY, endX, endY);

        System.out.println("\nGenerated Maze:");
        mazeSolver.displayMaze();

        System.out.println("\nSolving the maze...");
        if (mazeSolver.solveMaze()) {
            System.out.println("Maze Solved! Here's the solution:");
            mazeSolver.displayMaze();
        } else {
            System.out.println("No path found from start to end.");
        }

        mazeSolver.resetVisited();

        System.out.println("\nYou can try solving the maze again with different points.");
        System.out.println("Enter new starting point (x y):");
        startX = scanner.nextInt();
        startY = scanner.nextInt();

        System.out.println("Enter new ending point (x y):");
        endX = scanner.nextInt();
        endY = scanner.nextInt();

        mazeSolver.setStartAndEnd(startX, startY, endX, endY);

        System.out.println("\nGenerated Maze:");
        mazeSolver.displayMaze();

        System.out.println("\nSolving the maze again...");
        if (mazeSolver.solveMaze()) {
            System.out.println("Maze Solved! Here's the solution:");
            mazeSolver.displayMaze();
        } else {
            System.out.println("No path found from start to end.");
        }

        scanner.close();
    }
}
